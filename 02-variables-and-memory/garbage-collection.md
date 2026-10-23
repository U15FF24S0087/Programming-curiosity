# Garbage Collection

## The Problem: Memory Leaks

When a program allocates memory on the heap, that memory needs to be freed eventually. In languages like C, the programmer does this manually:

```c
int* arr = malloc(100 * sizeof(int));  // allocate
// ... use arr ...
free(arr);  // manually free — if you forget, it's a memory leak
```

If you forget to free memory, the program keeps consuming more and more RAM until it crashes or brings the system to a halt. This is a **memory leak**.

Manual memory management is error-prone. So most modern languages handle it automatically.

---

## What Is Garbage Collection?

Garbage collection (GC) is an automatic memory management process. The runtime periodically finds objects that are no longer reachable (no variable points to them) and frees their memory.

```python
def do_work():
    data = [1, 2, 3, 4, 5]   # allocated on heap
    process(data)
    # function ends — 'data' goes out of scope
    # GC will eventually reclaim this memory automatically

do_work()
# the list is now unreachable — garbage collector can free it
```

---

## How Does GC Know What to Free?

The key concept is **reachability**. An object is "alive" if it can be reached from any active variable, directly or indirectly. If nothing points to it, it's garbage.

### Mark and Sweep (common algorithm)

1. **Mark phase**: Start from all known "root" references (global variables, stack variables). Follow every pointer and mark everything reachable.
2. **Sweep phase**: Scan all heap memory. Anything not marked is garbage — free it.

```
Roots: [a, b]

a → Object1 → Object3
b → Object2

Object4 (unreachable — no path from roots)

After sweep: Object4 is freed
```

### Reference Counting (alternative)

Each object tracks how many references point to it. When the count drops to zero, the object is immediately freed.

```python
x = [1, 2, 3]   # ref count = 1
y = x            # ref count = 2
del x            # ref count = 1
del y            # ref count = 0 → freed immediately
```

**Problem with reference counting**: circular references.

```python
a = {}
b = {}
a['ref'] = b
b['ref'] = a
del a
del b
# Both objects still reference each other — counts never reach 0
# Python handles this with a cycle detector
```

---

## GC in Different Languages

| Language | Memory Management |
|---|---|
| C / C++ | Manual (`malloc`/`free`, `new`/`delete`) |
| Rust | Ownership system (compile-time, no GC) |
| Java | Mark-and-sweep GC (JVM) |
| Python | Reference counting + cycle detector |
| JavaScript | Mark-and-sweep (V8 engine) |
| Go | Concurrent mark-and-sweep GC |

---

## The Tradeoffs

GC is convenient but not free:

**Pros:**
- No manual memory management
- Eliminates use-after-free bugs and most memory leaks
- Safer code by default

**Cons:**
- GC pauses can cause latency spikes (the program stops briefly while GC runs)
- Higher memory usage (GC needs headroom to operate efficiently)
- Less predictable performance

For most applications, GC is the right tradeoff. For real-time systems (game engines, operating systems, trading systems), languages with manual control or Rust's ownership model are preferred.
