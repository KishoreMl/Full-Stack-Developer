# Lombok in Java

## What is Lombok?

Project Lombok is a Java library that helps reduce boilerplate code by automatically generating commonly used code during compilation using annotations.

Instead of manually writing:
- getters and setters
- constructors
- `toString()`
- `equals()` and `hashCode()`
- builders
- logging setup

Lombok can generate them automatically with simple annotations.

---

# Why Lombok is Used

Without Lombok, Java classes can become very verbose.

## Without Lombok

```java
public class Employee {

    private String name;
    private int age;

    public Employee() {
    }

    public Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Employee{name='" + name + "', age=" + age + "}";
    }
}
```

---

## With Lombok

```java
import lombok.Data;
import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Employee {

    private String name;
    private int age;
}
```

### Usage Example

```java
public class Main {

    public static void main(String[] args) {

        Employee emp = new Employee("Kishore", 25);

        System.out.println(emp.getName());

        emp.setAge(26);

        System.out.println(emp);
    }
}
```

---

# Common Lombok Annotations

# 1. @Getter and @Setter

## Example

```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class User {

    private String username;
    private String email;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        User user = new User();

        user.setUsername("kishore123");
        user.setEmail("kishore@test.com");

        System.out.println(user.getUsername());
        System.out.println(user.getEmail());
    }
}
```

---

# 2. @ToString

## Example

```java
import lombok.ToString;

@ToString
public class Product {

    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Product product = new Product("Laptop", 65000);

        System.out.println(product);
    }
}
```

### Output

```text
Product(name=Laptop, price=65000.0)
```

---

# 3. @EqualsAndHashCode

## Example

```java
import lombok.EqualsAndHashCode;

@EqualsAndHashCode
public class Customer {

    private int id;
    private String name;

    public Customer(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Customer c1 = new Customer(1, "Kishore");
        Customer c2 = new Customer(1, "Kishore");

        System.out.println(c1.equals(c2));
    }
}
```

### Output

```text
true
```

---

# 4. @NoArgsConstructor

## Example

```java
import lombok.NoArgsConstructor;

@NoArgsConstructor
public class Student {

    private String name;
    private int age;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Student student = new Student();

        System.out.println(student);
    }
}
```

---

# 5. @AllArgsConstructor

## Example

```java
import lombok.AllArgsConstructor;

@AllArgsConstructor
public class Student {

    private String name;
    private int age;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Student student = new Student("Kishore", 25);

        System.out.println(student);
    }
}
```

---

# 6. @RequiredArgsConstructor

## Example

```java
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
public class Service {

    private final Repository repository;
    private final Logger logger;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Repository repository = new Repository();
        Logger logger = new Logger();

        Service service = new Service(repository, logger);

        System.out.println(service);
    }
}
```

---

# 7. @Data

`@Data` combines:
- `@Getter`
- `@Setter`
- `@ToString`
- `@EqualsAndHashCode`
- `@RequiredArgsConstructor`

## Example

```java
import lombok.Data;

@Data
public class Employee {

    private int id;
    private String name;
    private double salary;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Employee employee = new Employee();

        employee.setId(1);
        employee.setName("Kishore");
        employee.setSalary(85000);

        System.out.println(employee);

        System.out.println(employee.getName());
    }
}
```

### Output

```text
Employee(id=1, name=Kishore, salary=85000.0)
Kishore
```

---

# 8. @Builder

`@Builder` implements the Builder Pattern.

## Example

```java
import lombok.Builder;
import lombok.ToString;

@Builder
@ToString
public class User {

    private String name;
    private int age;
    private String city;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        User user = User.builder()
                .name("Kishore")
                .age(25)
                .city("Chennai")
                .build();

        System.out.println(user);
    }
}
```

### Output

```text
User(name=Kishore, age=25, city=Chennai)
```

---

# 9. @Slf4j

`@Slf4j` creates a logger automatically.

## Example

```java
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class OrderService {

    public void processOrder() {

        log.info("Order processing started");

        log.warn("Low stock warning");

        log.error("Payment failed");
    }
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        OrderService service = new OrderService();

        service.processOrder();
    }
}
```

### Console Output

```text
INFO  - Order processing started
WARN  - Low stock warning
ERROR - Payment failed
```

---

# 10. @Builder with Immutable Object Example

## Example

```java
import lombok.Builder;
import lombok.Getter;

@Getter
@Builder
public class Employee {

    private final int id;
    private final String name;
    private final String department;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Employee emp = Employee.builder()
                .id(1001)
                .name("Kishore")
                .department("Engineering")
                .build();

        System.out.println(emp.getName());
    }
}
```

---

# 11. @Value

`@Value` creates immutable classes.

## Example

```java
import lombok.Value;

@Value
public class Address {

    String city;
    String country;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Address address = new Address("Chennai", "India");

        System.out.println(address);

        System.out.println(address.getCity());
    }
}
```

---

# 12. @SneakyThrows

Allows throwing checked exceptions without declaring them.

## Example

```java
import lombok.SneakyThrows;

public class FileService {

    @SneakyThrows
    public void readFile() {

        Thread.sleep(1000);

        System.out.println("File read completed");
    }
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        FileService service = new FileService();

        service.readFile();
    }
}
```

---

# 13. @Cleanup

Automatically closes resources.

## Example

```java
import lombok.Cleanup;

import java.io.BufferedReader;
import java.io.FileReader;

public class FileReaderService {

    public void read() throws Exception {

        @Cleanup
        BufferedReader br =
                new BufferedReader(new FileReader("test.txt"));

        System.out.println(br.readLine());
    }
}
```

---

# How Lombok Works

Lombok works at compile time using annotation processing.

It modifies the Java Abstract Syntax Tree (AST) and injects generated code before compilation completes.

Your `.class` files contain the generated methods even though you don't see them in source code.

---

# Adding Lombok to a Project

# Maven Dependency

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.38</version>
    <scope>provided</scope>
</dependency>
```

---

# Gradle Dependency

```gradle
dependencies {

    compileOnly 'org.projectlombok:lombok:1.18.38'

    annotationProcessor 'org.projectlombok:lombok:1.18.38'
}
```

---

# IDE Setup

# IntelliJ IDEA

1. Install Lombok Plugin
2. Go to:
   `Settings → Build → Compiler → Annotation Processors`
3. Enable:
   `Enable annotation processing`

---

# Eclipse

1. Download Lombok jar
2. Run:

```bash
java -jar lombok.jar
```

3. Select Eclipse installation
4. Restart Eclipse

---

# Advantages of Lombok

- Reduces boilerplate code
- Cleaner and shorter classes
- Faster development
- Improves readability
- Useful in DTOs, Entities, and Models
- Simplifies Builder Pattern implementation
- Reduces manual constructor creation

---

# Disadvantages / Concerns

- Hidden generated code can confuse beginners
- Debugging may become harder
- Some teams avoid it for maintainability
- Requires IDE annotation processing support
- Excessive usage can reduce code clarity

---

# Best Practices

Use Lombok mainly for:
- DTOs
- request/response models
- configuration classes
- entity helper classes

Avoid excessive use in:
- complex domain models
- critical business logic classes

Prefer:
- `@Getter`
- `@Setter`
- `@Builder`

instead of blindly using `@Data` everywhere.

---

# Common Lombok Combination in Spring Boot

## Example

```java
import lombok.*;

@Getter
@Setter
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class UserDto {

    private Long id;
    private String name;
    private String email;
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        UserDto user = UserDto.builder()
                .id(1L)
                .name("Kishore")
                .email("kishore@test.com")
                .build();

        System.out.println(user);
    }
}
```

---

# Lombok vs Traditional Java

| Feature | Traditional Java | Lombok |
|---|---|---|
| Getters/Setters | Manual | Auto-generated |
| Constructors | Manual | Annotation-based |
| Builder Pattern | Large code | Simple |
| Logging | Manual logger creation | `@Slf4j` |
| Boilerplate | High | Low |
| Readability | Verbose | Cleaner |

---

# Official Website

https://projectlombok.org
