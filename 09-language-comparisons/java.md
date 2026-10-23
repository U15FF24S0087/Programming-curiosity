# Java

## What Is Java?

Java is a high-level, statically typed, object-oriented language created by James Gosling at Sun Microsystems in 1995. Its slogan was "Write Once, Run Anywhere" — compile your code once, and it runs on any device with a JVM (Java Virtual Machine).

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

---

## Key Characteristics

| Property | Java |
|---|---|
| Typing | Static (types declared explicitly) |
| Execution | Compiled to bytecode → JVM |
| Memory management | Automatic (garbage collection) |
| Paradigm | Object-oriented (everything is a class) |
| Concurrency | Threads, locks, executors |

---

## How Java Runs

```
Source.java → [javac compiler] → bytecode (.class) → [JVM] → execution
```

1. You write `.java` source files
2. The `javac` compiler translates them to `.class` bytecode files
3. The **JVM** (Java Virtual Machine) loads and executes the bytecode

The bytecode is platform-independent. The JVM is platform-specific (there's a Windows JVM, a Mac JVM, a Linux JVM). This is what makes Java portable — the same `.class` file runs on any OS with a JVM installed.

### JIT Compilation
The JVM doesn't just interpret bytecode — it uses JIT (Just-In-Time) compilation to translate hot code paths into native machine code at runtime, making frequently-run code nearly as fast as compiled C/C++.

---

## Why Java Is Popular

- **Enterprise standard**: used by banks, insurance, government, healthcare — systems that need reliability and longevity
- **Strong typing**: catches errors at compile time
- **Massive ecosystem**: Maven Central has millions of libraries
- **Mature tooling**: IntelliJ IDEA, Eclipse, Maven, Gradle, Spring Boot
- **Huge job market**: one of the most in-demand languages
- **Performance**: JVM is fast and highly optimized after decades of engineering

---

## Where Java Shines

### Enterprise Backend
- **Spring Boot** — the dominant Java web framework, used for microservices and enterprise APIs
- **Jakarta EE** (formerly Java EE) — enterprise specification

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
}
```

### Android Development
Java was the primary Android language until Kotlin became the default. Many existing Android apps are still written in Java.

### Big Data
- **Hadoop** — written in Java
- **Apache Kafka** — written in Java/Scala
- **Apache Spark** — runs on the JVM (Scala/Java)

---

## Strengths and Weaknesses

| Strengths | Weaknesses |
|---|---|
| Strong typing catches bugs early | Verbose — lots of boilerplate |
| Excellent tooling and IDEs | Slower startup than native or Node.js |
| Mature, battle-tested | Memory-heavy (JVM overhead) |
| Great performance (JIT) | More ceremony for simple tasks |
| Huge job market | Language evolution is slow (backward compatibility) |

### Verbosity Example
```java
// Java — a simple class
public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}
```
```python
# Python — the same thing
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Java requires getters, setters, and explicit typing. Python (and modern Java with records) can be more concise.

---

## The JVM Ecosystem

Java isn't the only language on the JVM:
- **Kotlin** — modern, concise, interoperable with Java. Now the preferred language for Android and many new backend projects.
- **Scala** — functional + OOP, used in big data (Spark)
- **Clojure** — Lisp on the JVM, functional
- **Groovy** — dynamic, scripting-friendly

All these languages compile to JVM bytecode and can use Java libraries. The JVM is the real platform — Java is just one language on it.

---

## Who Uses Java

- **Banks and financial institutions** — trading systems, back office
- **Google** — Android (historically), many backend services
- **Amazon** — massive Java backend
- **Netflix** — backend microservices (Spring Boot)
- **LinkedIn** — backend services
- **Government agencies** — large-scale systems

See `python-vs-javascript-vs-java.md` for a direct comparison of all three.
