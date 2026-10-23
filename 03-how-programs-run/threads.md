# Threads

## What Is a Thread?

A thread is the smallest unit of execution within a process. Every process has at least one thread (the main thread). A process can spawn multiple threads that run concurrently, sharing the same memory space.

```
Process
├── Thread 1 (main)  → handles UI
├── Thread 2         → downloads a file
└── Thread 3         → writes to a log
```

All three threads run "at the same time" (or appear to), sharing the same heap and global variables.

---

## Why Use Threads?

Threads let you do multiple things at once within a single program:

- **Responsiveness**: Keep a UI responsive while doing heavy work in the background
- **Parallelism**: Use multiple CPU cores to process data faster
- **I/O concurrency**: While one thread waits for a network response, others keep working

Without threads, a single slow operation (reading a file, waiting for a database) would freeze your entire program.

---

## Threads vs Processes

| | Thread | Process |
|---|---|---|
| Memory | Shared with parent process | Own private space |
| Creation | Fast, lightweight | Slower, heavyweight |
| Communication | Direct (shared variables) | Needs IPC (pipes, sockets) |
| Crash impact | Can crash the whole process | Isolated from other processes |
| Use case | Concurrent tasks in one app | Separate apps or strong isolation |

---

## The Danger: Race Conditions

Because threads share memory, they can step on each other's data:

```python
counter = 0

def increment():
    global counter
    for _ in range(1000000):
        counter += 1  # NOT thread-safe!

# Two threads both run increment()
# Expected: counter = 2,000,000
# Actual: counter = ~1,400,000 (data race)
```

`counter += 1` is actually three operations: read, add, write. If two threads interleave these steps, updates get lost. This is a **race condition**.

---

## Synchronization: Locks and Mutexes

A **mutex** (mutual exclusion lock) ensures only one thread accesses a resource at a time:

```python
import threading

counter = 0
lock = threading.Lock()

def increment():
    global counter
    for _ in range(1000000):
        with lock:       # only one thread enters at a time
            counter += 1
```

Now only one thread can execute the critical section at a time — correct, but slower.

---

## Deadlock

A deadlock happens when two threads each hold a lock the other needs, and both wait forever:

```
Thread A holds Lock 1, waits for Lock 2
Thread B holds Lock 2, waits for Lock 1
→ both wait forever
```

Avoiding deadlocks requires careful lock ordering and design.

---

## Concurrency Models

Different languages take different approaches to safe concurrent programming:

| Model | Description | Languages |
|---|---|---|
| Threads + Locks | Manual synchronization | Java, C++, Python |
| Actor Model | Threads communicate via messages, no shared state | Erlang, Elixir, Akka |
| Async/Await | Single thread, cooperative multitasking | JavaScript, Python asyncio |
| Goroutines | Lightweight threads with channels | Go |
| Ownership | Compile-time thread safety guarantees | Rust |

---

## Threads vs Async

**Threads** are OS-level and truly parallel (on multiple cores). They're good for CPU-intensive work.

**Async** (async/await, event loop) is a single-threaded model that efficiently handles many I/O operations by pausing and resuming tasks without blocking. It's good for network-heavy work (web servers, APIs).

Most web servers use async or thread pools — they spend most time waiting on I/O, not computing.
