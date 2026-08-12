# Core Java Interview Questions

---

## 1. What is Java?

Java is a high-level, object-oriented, platform-independent programming language developed by Sun Microsystems (now owned by Oracle).

Java follows the principle: **"Write Once, Run Anywhere (WORA)"**

Java code is compiled into bytecode, which runs on the JVM (Java Virtual Machine).

---

## 2. What are the main features of Java?

- Platform Independent
- Object-Oriented
- Robust
- Secure
- Multithreaded
- Distributed
- High Performance (via JIT Compiler)
- Automatic Memory Management (Garbage Collection)

---

## 3. What is JIT Compiler?

A JIT (Just-In-Time) compiler is a technique that combines interpretation and compilation.

Instead of compiling the entire program before execution (like a traditional compiler), a JIT compiler:

- Starts running the program
- Identifies code that is executed frequently ("hot" code)
- Compiles that code into native machine instructions while the program is running
- Reuses the compiled code for faster execution

### Benefits

- **Faster execution** — Compiled machine code runs much faster than interpreted code
- **Runtime optimizations** — The JIT can use information only available during execution, such as actual data types and usage patterns
- **Platform Independence** — Distribute portable bytecode and compile it for the specific CPU at runtime

### Drawbacks

- **Startup overhead** — Time is spent compiling code while the application is running
- **More memory usage** — Both bytecode and generated machine code may need to be stored

---

## 4. What is Garbage Collection?

Garbage Collection (GC) is an automatic memory management process in Java that identifies and removes objects that are no longer being used by the application, freeing memory for new objects.

### (i) Why Garbage Collection is needed?

- When you create objects in Java, they are stored in Heap Memory
- The object remains in memory as long as it is reachable through a reference
- When the object is no longer reachable, the object becomes eligible for garbage collection

### (ii) How Garbage Collection Works?

The Garbage Collector periodically:

- Finds objects that are still reachable
- Identifies unreachable objects
- Reclaims memory occupied by unreachable objects
- Compacts memory (depending on the GC algorithm)

**Heap memory structure:**

Java Heap is divided into generations:

- **Young generation** — New objects are created here
- **Old generation** — Objects that survive multiple GC cycles are promoted here

### (iii) Types of Garbage Collection

- **Minor GC** — Happens in Young generation
- **Major GC** — Happens in Old generation
- **Full GC** — Happens in both

### (iv) Types of Garbage Collectors

| Collector | Description |
|-----------|-------------|
| Serial GC | Single-threaded collector |
| Parallel GC | Multi-threaded collector |
| G1 GC | Default in modern Java |
| Z GC | Suitable for high-performance applications |
| Shenandoah GC | Concurrent collection |

### (v) Garbage Collection Roots

- Static references are GC roots (e.g., `public static Employee employee;`)
- Active thread stacks
- Local variables
- Static fields
- JNI references

### (vi) Mark and Sweep Algorithm

A fundamental GC algorithm:

1. **Mark** — Mark all reachable objects
2. **Sweep** — Remove all unreachable objects

### (vii) Requesting Garbage Collection

- We can suggest GC to JVM (e.g., `System.gc();`)
- Does not guarantee that GC will run immediately — the JVM decides whether to execute it

### 4.1 Can we force Garbage Collection?

**No.** `System.gc()` only requests GC; the JVM decides whether to execute it.

---

## 5. What is the difference between JDK, JRE, and JVM?

| Component | Description |
|-----------|-------------|
| **JVM** | Executes Java bytecode |
| **JRE** | Provides libraries required to run Java applications |
| **JDK** | Provides everything needed to develop and run Java applications |

**Hierarchy:**

```
JDK
├── JRE
│   └── JVM
├── Compiler (javac)
├── Debugger
├── Documentation Tools (javadoc)
└── Packaging Tools
```

### (i) Java Virtual Machine (JVM)

The JVM is a virtual machine that executes Java bytecode:

- Loads the bytecode
- Verifies it
- Interprets or JIT compiles it
- Executes it on the underlying OS

**JVM Responsibilities:**

| Responsibility | Description |
|----------------|-------------|
| Class loading | Loads `.class` files into memory |
| Bytecode verification | Checks bytecode is valid and secure |
| Memory management | Manages heap and stack memory |
| Garbage Collection | Removes unused objects |
| JIT Compilation | Converts frequently used bytecode into native machine code |

### (ii) Java Runtime Environment (JRE)

The JRE provides everything required to run Java applications:

```
JRE
├── JVM
├── Core Libraries
└── Supporting Files
```

**Examples of core libraries:** `java.lang`, `java.util`, `java.io`, `java.net`

### (iii) Java Development Kit (JDK)

The JDK is a complete toolkit for Java developers:

```
JDK
├── JRE
│   └── JVM
├── Compiler
├── Debugger
├── Documentation Tools
└── Packaging Tools
```

**Common JDK Tools:**

| Tool | Purpose |
|------|---------|
| `javac` | Compile Java code |
| `java` | Run Java applications |
| `jar` | Create JAR files |
| `javadoc` | Generate documentation |
| `jdb` | Debug Java programs |
| `jps` | List Java processes |
| `jstack` | Thread dump analysis |
| `jmap` | Heap analysis |
| `jcmd` | JVM diagnostics |

---

## 6. What is JVM?

JVM (Java Virtual Machine) is responsible for:

- Loading classes
- Verifying bytecode
- Executing bytecode
- Memory management
- Garbage collection

---

## 7. What is Platform Independence?

Java source code is compiled into bytecode:

```
Java Code
    ↓
Bytecode (.class)
    ↓
JVM
    ↓
Operating System
```

Since every OS has its own JVM implementation, the same bytecode runs everywhere.

---

# OOP Concepts

## 8. What are the four pillars of OOP?

### (i) Encapsulation

Wrapping data and methods together.

```java
class Employee {
    private String name;

    public String getName() {
        return name;
    }
}
```

### (ii) Inheritance

Acquiring properties from a parent class.

```java
class Animal {}
class Dog extends Animal {}
```

### (iii) Polymorphism

One interface, multiple implementations.

```java
Animal a = new Dog();
```

### (iv) Abstraction

Hiding implementation details.

```java
abstract class Shape {
    abstract void draw();
}
```

---

## 9. Difference between Abstraction and Encapsulation

| Aspect | Abstraction | Encapsulation |
|--------|-------------|---------------|
| Hides | Implementation | Data |
| Achieved using | Abstract classes / interfaces | Access modifiers |
| Focuses on | Behavior | Security |

---

## 10. Can Java support Multiple Inheritance?

| Type | Support |
|------|---------|
| Classes | ❌ No |
| Interfaces | ✅ Yes |

```java
interface A {}
interface B {}

class C implements A, B {}
```

**Reason:** To avoid the Diamond Problem.

---

# String Questions

## 11. Difference between String, StringBuilder, and StringBuffer

| Feature | String | StringBuilder | StringBuffer |
|---------|--------|---------------|--------------|
| Mutable | No | Yes | Yes |
| Thread Safe | No | No | Yes |
| Performance | Slow | Fast | Medium |

---

## 12. Why is String immutable?

**Reasons:**

- Security
- Thread Safety
- String Pool optimization
- Hashcode caching

**Example:**

```java
String s = "Java";
s.concat("8");
System.out.println(s);
```

**Output:** `Java`

---

## 13. What is String Pool?

A memory area inside the heap where string literals are stored.

```java
String a = "Java";
String b = "Java";
System.out.println(a == b);
```

**Output:** `true`

Both references point to the same object.

---

# Collections Framework

## 14. What is Collection Framework?

A set of classes and interfaces used to store and manipulate data.

**Main Interfaces:**

```
Collection
 ├── List
 ├── Set
 └── Queue

Map (separate hierarchy)
```

---

## 15. Difference between List and Set

| Aspect | List | Set |
|--------|------|-----|
| Duplicates | Allowed | Not allowed |
| Ordering | Ordered | May not be ordered |
| Indexing | Indexed | Not indexed |

---

## 16. Difference between ArrayList and LinkedList

| Aspect | ArrayList | LinkedList |
|--------|-----------|------------|
| Structure | Dynamic Array | Doubly Linked List |
| Retrieval | Fast | Slow |
| Insertion | Slow | Fast |

---

## 17. Difference between HashMap and Hashtable

| Aspect | HashMap | Hashtable |
|--------|---------|-----------|
| Synchronization | Not synchronized | Synchronized |
| Null keys | Allows one null key | No null keys |
| Performance | Faster | Slower |

---

## 18. How does HashMap work internally?

1. Calculate hashcode
2. Find bucket
3. Store key-value pair
4. Handle collisions using:
   - Linked List (Java 7)
   - Linked List + Red-Black Tree (Java 8+)

```java
map.put("A", 1);
```

---

## 19. What is the difference between HashMap and ConcurrentHashMap?

| Aspect | HashMap | ConcurrentHashMap |
|--------|---------|-------------------|
| Thread safety | Not thread-safe | Thread-safe |
| Locking | Entire map accessible | Segment / bucket locking |
| Performance | Faster (single thread) | Better concurrency |

---

# Exception Handling

## 20. What is Exception?

An event that disrupts normal program execution.

```java
int x = 10 / 0;
```

**Throws:** `ArithmeticException`

---

## 21. Difference between Checked and Unchecked Exceptions

### Checked Exceptions

Checked at compile time:

- `IOException`
- `SQLException`

### Unchecked Exceptions

Runtime exceptions — not checked at compile time:

- `NullPointerException`
- `ArrayIndexOutOfBoundsException`

---

## 22. Difference between `throw` and `throws`

| Aspect | `throw` | `throws` |
|--------|---------|----------|
| Purpose | Used to throw exception | Declares exception |
| Location | Inside method | Method signature |
| Example | `throw new Exception();` | `public void test() throws Exception {}` |

---

# Multithreading

## 23. What is Thread?

A lightweight sub-process.

```java
Thread t = new Thread();
t.start();
```

---

## 24. Difference between `start()` and `run()`

### `start()`

Creates a new thread.

```java
t.start();
```

### `run()`

Executes in the current thread.

```java
t.run();
```

---

## 25. What is Synchronization?

Controls access to shared resources.

```java
synchronized void increment() {
    count++;
}
```

---

## 26. What is Deadlock?

When two threads wait indefinitely for each other.

- Thread A waits for B
- Thread B waits for A

---

# Java 8 Questions

## 27. What are Lambda Expressions?

Anonymous functions.

```java
(a, b) -> a + b
```

**Example:**

```java
List<String> names = List.of("A", "B");
names.forEach(name -> System.out.println(name));
```

---

## 28. What is Functional Interface?

An interface with exactly one abstract method.

```java
@FunctionalInterface
interface Test {
    void execute();
}
```

**Examples:** `Runnable`, `Comparator`, `Predicate`, `Consumer`

---

## 29. What are Streams?

Used for functional data processing.

```java
List<Integer> nums = List.of(1, 2, 3, 4);

nums.stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);
```

**Output:**

```
2
4
```

---

## 30. Difference between `map()` and `flatMap()`

### `map()`

One-to-one transformation.

```java
names.stream()
     .map(String::toUpperCase);
```

### `flatMap()`

Flattens nested structures.

```java
list.stream()
    .flatMap(Collection::stream);
```

---

## 31. What is Optional?

Used to avoid `NullPointerException`.

```java
Optional<String> name = Optional.ofNullable(value);
name.ifPresent(System.out::println);
```

---

# Memory Management

## 32. What are Heap and Stack Memory?

| Aspect | Heap | Stack |
|--------|------|-------|
| Stores | Objects | Local variables |
| Scope | Shared | Thread-specific |
| Management | GC managed | Auto released |

---

## 33. What causes Memory Leak in Java?

Objects remain referenced and cannot be garbage collected.

**Example:**

```java
static List<Object> cache = new ArrayList<>();
```

Continuously adding objects causes memory leaks.

---

# Advanced Java Questions

## 34. What is Reflection?

Allows inspection and modification of classes at runtime.

```java
Class<?> cls = Class.forName("Employee");
```

---

## 35. What is Serialization?

Converting an object into a byte stream.

```java
class Employee implements Serializable {}
```

---

## 36. What is `transient` keyword?

Prevents field serialization.

```java
transient String password;
```

---

## 37. What is `volatile` keyword?

Ensures visibility of variable changes across threads.

```java
volatile boolean running;
```

---

## 38. Difference between Comparable and Comparator

### Comparable

Natural ordering.

```java
class Employee implements Comparable<Employee>
```

### Comparator

Custom ordering.

```java
Comparator<Employee> bySalary
```

---

## 39. What is Immutable Class?

**Example:**

```java
public final class Employee {

    private final String name;

    public Employee(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

**Characteristics:**

- `final` class
- `final` fields
- No setters

---

# Frequently Asked for Senior Java Developers

## 40. What are SOLID Principles?

| Principle | Name |
|-----------|------|
| **S** | Single Responsibility |
| **O** | Open Closed |
| **L** | Liskov Substitution |
| **I** | Interface Segregation |
| **D** | Dependency Inversion |

---

## 41. Explain `equals()` and `hashCode()`

**Rule:** If two objects are equal using `equals()`, they must return the same `hashCode()`.

```java
@Override
public boolean equals(Object o)

@Override
public int hashCode()
```

**Used heavily in:** `HashMap`, `HashSet`, `ConcurrentHashMap`

---

## 42. What is the difference between `==` and `equals()`?

| Aspect | `==` | `equals()` |
|--------|------|------------|
| Compares | References | Content |
| Type | Operator | Method |

```java
String a = new String("Java");
String b = new String("Java");

a == b;      // false
a.equals(b); // true
```

---

## 43. What is fail-fast and fail-safe iterator?

### Fail-Fast

Throws `ConcurrentModificationException`

**Examples:** `ArrayList`, `HashMap`

### Fail-Safe

Works on a copy of the collection.

**Examples:** `ConcurrentHashMap`, `CopyOnWriteArrayList`
