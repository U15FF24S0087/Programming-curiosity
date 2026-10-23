# Python vs JavaScript vs Java

## The Big Three

These are three of the most popular programming languages in the world. Each has a different philosophy, ecosystem, and sweet spot. Here's a direct comparison.

---

## At a Glance

| | Python | JavaScript | Java |
|---|---|---|---|
| **Created** | 1991 | 1995 | 1995 |
| **Typing** | Dynamic | Dynamic | Static |
| **Execution** | Interpreted (bytecode + VM) | Interpreted + JIT | Compiled to bytecode + JVM |
| **Paradigm** | Multi-paradigm | Multi-paradigm | Object-oriented |
| **Memory** | GC (ref counting + cycle detector) | GC (mark-and-sweep) | GC (JVM) |
| **Concurrency** | Threads, async (limited by GIL) | Single-threaded event loop, async | Threads, executors, locks |
| **Syntax** | Minimal, indentation-based | C-like, flexible | C-like, verbose |
| **Learning curve** | Easiest | Easy to start, hard to master | Steepest |

---

## The Same Task in All Three

Print a list of users older than 25, sorted by name.

### Python
```python
users = [
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 22},
    {"name": "Charlie", "age": 35},
]

result = sorted(
    [u for u in users if u["age"] > 25],
    key=lambda u: u["name"]
)
for u in result:
    print(f"{u['name']}, {u['age']}")
```

### JavaScript
```javascript
const users = [
    { name: "Alice", age: 30 },
    { name: "Bob", age: 22 },
    { name: "Charlie", age: 35 },
];

const result = users
    .filter(u => u.age > 25)
    .sort((a, b) => a.name.localeCompare(b.name));

result.forEach(u => console.log(`${u.name}, ${u.age}`));
```

### Java
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<User> users = Arrays.asList(
            new User("Alice", 30),
            new User("Bob", 22),
            new User("Charlie", 35)
        );

        users.stream()
            .filter(u -> u.getAge() > 25)
            .sorted(Comparator.comparing(User::getName))
            .forEach(u -> System.out.println(u.getName() + ", " + u.getAge()));
    }
}

class User {
    private String name;
    private int age;
    public User(String name, int age) { this.name = name; this.age = age; }
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

Notice the progression: Python is the most concise, Java is the most verbose (you need a class definition, getters, imports). JavaScript sits in between.

---

## Where Each Language Wins

### Python
| Domain | Why |
|---|---|
| Data science / ML / AI | NumPy, Pandas, PyTorch — no competition |
| Scripting / automation | Quick to write, batteries included |
| Prototyping | Fastest from idea to working code |
| Education | Simplest syntax, most readable |

### JavaScript
| Domain | Why |
|---|---|
| Frontend web | Only language that runs in browsers |
| Full-stack (same language both sides) | Node.js + React/Angular/Vue |
| Mobile (cross-platform) | React Native |
| Real-time apps (chat, collaboration) | Event loop + async/await |

### Java
| Domain | Why |
|---|---|
| Enterprise backend | Mature, scalable, battle-tested |
| Android | Historically dominant (Kotlin now preferred) |
| Large systems | Strong typing + mature tooling |
| High-performance servers | JVM JIT is very fast |

---

## Typing Comparison

```python
# Python — dynamic, no declarations
x = 5          # x is an int
x = "hello"    # now x is a string (allowed but risky)
```

```javascript
// JavaScript — dynamic, no declarations
let x = 5;
x = "hello";   // allowed, but risky
```

```typescript
// TypeScript — static (optional types)
let x: number = 5;
x = "hello";   // Error
```

```java
// Java — static, strict
int x = 5;
x = "hello";   // Compile error
```

Java catches type errors at compile time. Python and JavaScript catch them at runtime (when the code runs). TypeScript brings static checking to JavaScript.

---

## Concurrency Comparison

### Python (GIL limits true parallelism)
```python
import threading
# Threads can't use multiple cores for CPU work
# Use multiprocessing instead
from multiprocessing import Pool

with Pool(4) as p:
    results = p.map(heavy_function, data)
```

### JavaScript (single-threaded event loop)
```javascript
// No true parallelism, but excellent I/O concurrency
const data = await Promise.all([
    fetch("/api/users"),
    fetch("/api/products"),
    fetch("/api/orders")
]);
// All three requests happen concurrently
```

### Java (true threads, multiple cores)
```java
ExecutorService executor = Executors.newFixedThreadPool(4);
Future<Integer> result = executor.submit(() -> {
    return heavyComputation();
});
// Runs on a separate thread, can use multiple cores
```

---

## When to Choose Which

| If you want to... | Choose |
|---|---|
| Do data science or ML | **Python** |
| Build a website frontend | **JavaScript** |
| Build a backend for a large company | **Java** (or Kotlin) |
| Build a quick prototype | **Python** |
| Build a real-time web app | **JavaScript** (Node.js) |
| Work in enterprise / banking | **Java** |
| Automate tasks | **Python** |
| Build a mobile app | **JavaScript** (React Native) or **Kotlin** (Android) |
| Learn your first language | **Python** |

---

## The Honest Truth

There's no "best" language. The best language is the one that:
1. Fits your problem
2. You can be productive in
3. Has the libraries and community you need
4. You can get a job with (if that's your goal)

Most professional developers know more than one. Start with one, learn the concepts, and the second language becomes much easier — the fundamentals transfer even if the syntax changes.
