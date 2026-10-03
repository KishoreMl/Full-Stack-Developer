# Connect infrastructure and request path

Plain-language reference for how client and service traffic reaches ECS, data stores, and async messaging. Complements [CONNECT_REQUEST_FLOWS.md](./CONNECT_REQUEST_FLOWS.md) (API-level flows and Mermaid diagrams). This file uses **ASCII diagrams** so they render in any Markdown viewer.

Evidence is from Connect Platform monolith IAC, Issue Management / BCF / Auth service Terraform, and application config in this workspace.

---

## 1. End-to-end path (mental model)

**One line:** DNS → ALB → listener rule → target group → ECS task → (sync: HTTP/DB) → (async: Lambda/SNS/SQS).

```
┌──────────┐
│  CLIENT  │  Browser / desktop / SDK — HTTPS + Bearer token
└────┬─────┘
     │  (1) Resolve hostname
     ▼
┌──────────┐
│   DNS    │  Public: app.*.connect…  |  Internal: *.ms.internal.connect…
└────┬─────┘
     │  (2) Alias → ALB
     ▼
┌──────────────────────────────────────────────────────────────┐
│  ALB (Application Load Balancer)                             │
│  TLS :443 · listener rules by PRIORITY (path, method, query) │
└────┬─────────────────────────────────────────────────────────┘
     │  (3) Forward to target group
     ▼
┌──────────────────────────────────────────────────────────────┐
│  TARGET GROUP                                                │
│  Registered healthy task IPs:ports · health e.g. /v1/health  │
└────┬─────────────────────────────────────────────────────────┘
     │  (4) One healthy ECS task
     ▼
┌──────────────────────────────────────────────────────────────┐
│  ECS TASK  (one container replica — NOT a separate microservice)│
│  JWT filter → controller → service → outbound HTTP / AWS SDK │
└────┬─────────────────────────────────────────────────────────┘
     │
     ├─ SYNC (request often waits)
     │    Internal ALB → Auth, Project, User, Permissions, Ecom…
     │    MySQL/Aurora (Platform) · DynamoDB (Issues) · object storage (files)
     │
     └─ ASYNC (after HTTP response, usually)
          DynamoDB Stream → Lambda → SNS · outbox → SNS · SQS → Lambda
     │
     ▼
  Response ← task ← ALB ← client
```

**Legend:** solid arrows = synchronous · dashed/async described in section 6

---

## 2. ALB per environment (not one global ALB)

| Scope | What you get |
|--------|----------------|
| Per **environment + region** (e.g. stg-us1, prod-us1) | One shared **external** ALB + one shared **internal** ALB (typical) |
| External | Customer/SDK traffic — e.g. `stage-us1-tc-fips-alb`, `prod-us1-app-fips-alb` |
| Internal | Service-to-service — e.g. `stage-us1-tc-fips-internal-alb`, `prod-us1-ms-fips-internal-alb` |

Many microservices **share** the same ALB; **listener rules** (path + priority) pick the **target group**. Platform monolith Terraform also forwards some paths (e.g. Files Service) via `microservice-forward-rules-*.json` under `trimble-connect-platform/iac/monolith_infra/`.

### External vs internal

| | **External ALB** | **Internal ALB** |
|---|------------------|------------------|
| **Reachable from** | Internet / public API hostnames | VPC only (private) |
| **Typical callers** | UI, SDKs, partners | Platform, Issue Management, BCF calling Auth/Project/etc. |
| **Example paths** | `/tc/api/...`, public issue/BCF routes | `/v1/auth/*`, project membership APIs |
| **Why separate** | Edge security, WAF, customer traffic | No hairpin through public edge; narrower path rules |

Backends use internal DNS such as `auth.stage.ms.internal.connect.trimble.com` (see Issue Management `application-stg-us1.yml`).

---

## 3. How the ALB picks a microservice

The ALB does not know service names. It matches **listener rules**:

1. Evaluate rules in **priority order** (lower number first).
2. Conditions (often **AND**): path pattern, HTTP method, sometimes query string.
3. Action: **forward** to **target group ARN**.
4. Target group → healthy **ECS task** IP:port.

Rules come from:

- **Platform:** `monolith-forward-rules`, `microservice-forward-rules` (monolith infra).
- **Each service Terraform:** `external_path_pattern` / `internal_path_pattern`, `external_priority`, listener ARN (e.g. Issue Management `iac/service_infra/envs/*.tfvars`).

If no rule matches, the listener **default action** applies (often a fixed error response).

---

## 4. Target group vs ECS vs microservice

| Concept | What it is |
|---------|------------|
| **Microservice** | One app (e.g. Issue Management) = one **ECS service** |
| **Task / instance** | **One running container** of that app (replica) |
| **`desired_count = 3`** | **Three copies of the same** microservice, not three different services |
| **ECS cluster** | Shared pool (e.g. `tc-stage-ms-fargate-cluster`) — **many** ECS services |
| **Target group** | ALB’s **list of healthy task endpoints** for one routing path — **not** the whole microservice stack (no DB, IAM, SNS in the TG) |

```
Microservice "Issue Management"
│
├── ECS service          ← "run 3 copies"
│     ├── Task A  (10.0.1.10:6800)  ──┐
│     ├── Task B  (10.0.1.11:6800)  ──┼── registered in Target group(s)
│     └── Task C  (10.0.1.12:6800)  ──┘
│
├── DynamoDB, IAM, SNS, …   ← NOT in the target group
│
└── Target group            ← ALB forwards HTTP here when rule matches
```

**“ECS runs the task”** = ECS starts the container (Fargate allocates compute), registers IP:port with the target group, restarts failed tasks, performs rolling deploys, and honors `desired_count` / auto scaling. Application code runs **inside** the container.

---

## 5. ECS vs EC2 (short)

Connect microservices in tfvars use **Fargate** (`tc-*-ms-fargate-cluster`).

| Fargate ECS | App on EC2 |
|-------------|------------|
| Define image + CPU/RAM; AWS runs compute | You manage VMs, OS patch, deploy scripts |
| Native ALB target groups, desired count, rolling deploy | You wire ASG/ALB yourself |
| Good fit for many stateless APIs | Can be cheaper at huge steady scale or special host needs |

ECS on **EC2 launch type** still uses EC2 instances; Fargate hides that layer.

---

## 6. Example flows (ASCII)

### A — Create issue (Issue Management)

```
Client
  → DNS (public API)
  → EXTERNAL ALB          path: /v1/context/*/issues
  → TG: issue-management
  → ECS task :6800
        → internal ALB → Auth GET /v1/auth/validate
        → internal ALB → Project GET .../users/{userId}
        → DynamoDB PutItem (issue_*)
  ← 201

  (async, after response)
  DynamoDB Stream → Lambda (issue_event_publisher) → SNS (issue.created)
```

See [CONNECT_REQUEST_FLOWS.md § Create issue](./CONNECT_REQUEST_FLOWS.md#3-create-issue--topic--end-to-end-flow).

### B — Create project (Platform monolith)

```
Client
  → DNS
  → EXTERNAL ALB          POST /tc/api/2.0/projects
  → TG: monolith_app
  → ECS (Platform / gteamapp)
        → Auth validate · TAM · Ecom (if ecom_enforcement)
        → MySQL insert project + roles + outbox
  ← 201

  (async)
  outbox → SNS com.trimble.connect.project.created.v1
```

Project Service does **not** create the project row; Platform MySQL does.

### C — Upload file (Files Service + metadata)

```
Client
  → EXTERNAL ALB          POST .../files/fs/initiate | commit
  → TG: files_service
  → ECS (Files Service — source not in this workspace)
  → presigned PUT → object storage (bytes)
  ← commit response

  (async)
  SNS (file commit) → Platform MySQL metadata · processing callbacks → Platform ECS
```

### D — Service-to-service (internal ALB)

```
ECS task A (e.g. Issue Management)
  → DNS auth.stage.ms.internal.connect…
  → INTERNAL ALB          path: /v1/auth/*
  → TG: auth-service
  → ECS task B :4100
```

---

## 7. Scaling and limits

| Question | Answer |
|----------|--------|
| **3 instances = 3 microservices?** | No — **3 replicas of one** ECS service. |
| **Max tasks per service?** | No small fixed cap; bounded by **Fargate vCPU/memory quotas** (account/region), optional **Cloud Map ~1000** if service discovery, and your **auto scaling max** in tfvars. |
| **Who health-checks tasks?** | **ALB** against targets registered in the **target group** (e.g. `/v1/health`). Unhealthy tasks stop receiving traffic; ECS replaces them. |

---

## 8. Key takeaways

| Question | Answer |
|----------|--------|
| Client path? | DNS → external ALB → rule → TG → ECS task |
| Backend path? | Internal DNS → internal ALB → rule → TG → ECS task |
| Target group? | ALB’s health-checked pool of task IPs — not full service infrastructure |
| DB on user request? | Often **yes, sync** (MySQL/DynamoDB) for the APIs in CONNECT_REQUEST_FLOWS |
| Lambda/SNS? | Usually **async** after HTTP response (streams, outbox, queues) |
| One ALB for everything? | **Per env+region**, shared external + shared internal — not one global ALB |

---

## 9. Related docs

| Doc | Purpose |
|-----|---------|
| [CONNECT_REQUEST_FLOWS.md](./CONNECT_REQUEST_FLOWS.md) | Create project / issue / file, auth, ecom, service tables, Mermaid |
| [CONNECT_REQUEST_FLOWS canvas](file:///C:/Users/kishoml/.cursor/projects/c-Users-kishoml-Documents-GitHub-issue-management-service/canvases/connect-request-flows.canvas.tsx) | Interactive flow diagrams in Cursor |

Platform IAC references: `trimble-connect-platform/iac/monolith_infra/listener_rules.tf`, `monolith_alb_rules_config/*/microservice-forward-rules-*.json`.

---

## 10. AWS references (optional)

- [Amazon ECS endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ecs-service.html)
- [Fargate throttling / launch rates](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/throttling.html)
