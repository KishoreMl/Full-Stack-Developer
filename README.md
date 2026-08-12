# Full-Stack Developer — Learning Notes

A personal knowledge base of **learning notes, roadmaps, and interview prep material** for becoming a full-stack developer. This repository is organized by topic and skill area so concepts can be revisited, tracked, and expanded over time.

---

## Overview

The notes cover the full software development lifecycle—from frontend UI and JavaScript fundamentals, through backend services in Java/Spring Boot, to databases, cloud (AWS), system design, and data structures & algorithms.

Use this repo as:

- A **structured study guide** with phased roadmaps per topic
- A **quick reference** for concepts like HTTP, JWT, OAuth, CORS, and SOLID principles
- An **interview preparation checklist** to track what is done, partial, or still to learn

> Start with [`Interview Roadmap.txt`](./Interview%20Roadmap.txt) for a high-level checklist across all areas.

---

## Repository Structure

```
Full-Stack-Developer/
├── Interview Roadmap.txt          # Master checklist for interview prep
│
├── Frontend/
│   ├── JavaScript/
│   │   ├── Roadmap.txt            # Phased JS learning path
│   │   ├── Basics.txt
│   │   ├── Promises.txt
│   │   └── Async & Await.txt
│   └── ReactJs/
│       ├── Notes.txt
│       ├── ReactRendering.txt
│       └── Interview-questions.md
│
├── Backend/
│   ├── Java/
│   │   ├── Roadmap.txt
│   │   ├── Notes.txt
│   │   └── Interview Questions.md
│   └── SpringBoot/
│       ├── Notes.txt
│       └── Lombok_Package.MD
│
├── Web Development/
│   ├── Browser APIs.txt
│   ├── BrowserPaint.txt
│   ├── Paginations.txt
│   └── UI Performance improvement.md
│
├── Common/                        # Cross-cutting concepts (frontend + backend)
│   ├── Authentication & Authorization.txt
│   ├── CORS.txt
│   ├── Debouncing & Throtlling.txt
│   ├── HTTP_Protocol.txt
│   ├── JWT.txt
│   ├── OAuth.txt
│   ├── REST API.txt
│   └── SOLID Principles.txt
│
├── Database/
│   └── Roadmap.txt                # DBMS, SQL, normalization, indexing, etc.
│
├── DSA/
│   ├── Roadmap.txt                # Data structures & algorithms phases
│   └── Interview-Questions.txt
│
├── System Design/
│   └── Roadmap.txt                # Scalability, caching, CAP, design patterns
│
└── AWS/
    ├── Roadmap.txt
    ├── Notes.txt
    └── SNS.txt
```

---

## Topics Covered

### Frontend

| Area | Topics |
|------|--------|
| **JavaScript** | Syntax, control flow, functions, arrays/objects, DOM, async (Promises, async/await), scope & closures |
| **React** | Lifecycle, Virtual DOM, reconciliation, Fiber, hooks, state management (Redux/Thunk), routing, lazy loading, pagination, virtualization, code splitting |
| **Build tools** | Vite, Webpack, Babel (planned in roadmap) |
| **Web fundamentals** | Browser APIs, rendering/paint, pagination patterns, UI performance |

### Backend

| Area | Topics |
|------|--------|
| **Java** | JVM/JDK/JRE, OOP, collections, exceptions, multithreading, immutability, Comparable/Comparator |
| **Spring Boot** | Dependency injection, bean lifecycle, Spring MVC, REST controllers, validation, JPA, exception handling, logging, scheduling |
| **Caching** | Redis, TTL, distributed cache (roadmap) |

### Shared / Full-Stack Concepts

| Topic | File |
|-------|------|
| HTTP protocol | [`Common/HTTP_Protocol.txt`](./Common/HTTP_Protocol.txt) |
| REST APIs | [`Common/REST API.txt`](./Common/REST%20API.txt) |
| Authentication & Authorization | [`Common/Authentication & Authorization.txt`](./Common/Authentication%20%26%20Authorization.txt) |
| JWT | [`Common/JWT.txt`](./Common/JWT.txt) |
| OAuth | [`Common/OAuth.txt`](./Common/OAuth.txt) |
| CORS | [`Common/CORS.txt`](./Common/CORS.txt) |
| SOLID principles | [`Common/SOLID Principles.txt`](./Common/SOLID%20Principles.txt) |
| Debouncing & throttling | [`Common/Debouncing & Throtlling.txt`](./Common/Debouncing%20%26%20Throtlling.txt) |

### Data & Infrastructure

| Area | Focus |
|------|-------|
| **Database** | DBMS basics, ER modeling, SQL, normalization, ACID, transactions, joins, indexing, query optimization |
| **DSA** | Arrays, strings, linked lists, stacks/queues, hashing, trees, graphs, Big-O, common algorithms (DFS, BFS, Dijkstra, etc.) |
| **System Design** | Scalability, load balancing, caching, CAP theorem, reliability, security, common system design patterns |
| **AWS** | S3, EC2, VPC, RDS, Lambda, API Gateway, SNS, SQS, ECS, CloudFront, CloudWatch, IaC (Terraform/CloudFormation) |

---

## Suggested Learning Path

A practical order if you are building skills from the ground up:

1. **Foundations** — [`DSA/Roadmap.txt`](./DSA/Roadmap.txt), [`Common/HTTP_Protocol.txt`](./Common/HTTP_Protocol.txt), [`Common/REST API.txt`](./Common/REST%20API.txt)
2. **Frontend** — [`Frontend/JavaScript/Roadmap.txt`](./Frontend/JavaScript/Roadmap.txt) → [`Frontend/ReactJs/Notes.txt`](./Frontend/ReactJs/Notes.txt)
3. **Backend** — [`Backend/Java/Roadmap.txt`](./Backend/Java/Roadmap.txt) → [`Backend/SpringBoot/Notes.txt`](./Backend/SpringBoot/Notes.txt)
4. **Data layer** — [`Database/Roadmap.txt`](./Database/Roadmap.txt)
5. **Auth & security** — JWT, OAuth, CORS, Authentication & Authorization (under `Common/`)
6. **Scale & deploy** — [`System Design/Roadmap.txt`](./System%20Design/Roadmap.txt), [`AWS/Roadmap.txt`](./AWS/Roadmap.txt)
7. **Interview prep** — [`Interview Roadmap.txt`](./Interview%20Roadmap.txt) + topic-specific interview question files

Each roadmap file is split into **phases** (e.g., fundamentals → intermediate → advanced) so you can study incrementally without jumping ahead.



## Progress Tracking

The master checklist in [`Interview Roadmap.txt`](./Interview%20Roadmap.txt) uses simple status labels:

| Label | Meaning |
|-------|---------|
| `Done` | Topic studied and notes captured |
| `Partial` | Started but needs more depth |
| *(blank)* | Not yet started |

Examples of completed areas (as of the current notes): Browser APIs, JWT, HTTP Protocol, debouncing/throttling, SOLID Principles.

---

## Tech Stack Focus

This collection is centered on a common full-stack profile:

| Layer | Technologies |
|-------|--------------|
| Frontend | JavaScript, React |
| Backend | Java, Spring Boot, Lombok |
| Database | SQL / relational DBMS (MySQL, PostgreSQL via RDS) |
| Cloud | AWS (serverless, containers, storage, networking) |
| Practices | REST, OAuth/JWT, caching (Redis), system design |


