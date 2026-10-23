# Memory

## What Is Memory?

Memory (RAM — Random Access Memory) is where your running program lives. It's fast, temporary storage that the CPU can read from and write to almost instantly.

When you run a program:
- The OS loads the program's code into memory
- Your variables are stored in memory as the program runs
- When the program ends, that memory is released

Think of memory as a giant array of numbered slots (addresses), each holding one byte of data.

---

## Memory Address

Every byte of memory has a unique address — a number that identifies its location.

```
Address | Value
--------|------
0x1000  |  25      ← variable 'age' lives here
0x1001  |  0
0x1002  |  65      ← first byte of 'name' ("Alice")
0x1003  |  108
...
```

When your code does `age = 25`, the runtime finds a free slot, stores `25` there, and remembers that `age` refers to that address.

---

## How Much Memory Does a Value Take?

Different types take different amounts of space:

| Type | Typical Size |
|---|---|
| Boolean | 1 byte |
| Integer (32-bit) | 4 bytes |
| Integer (64-bit) | 8 bytes |
| Float (64-bit) | 8 bytes |
| Character | 1–4 bytes (depends on encoding) |
| Pointer/Reference | 8 bytes (on 64-bit systems) |

A string like `"Alice"` takes 5+ bytes — one for each character, plus overhead.

---

## Memory Layout of a Process

When your program runs, its memory is divided into regions:

```
┌────────────────────┐  High address
│      Stack         │  ← local variables, function calls
├────────────────────┤
│        ↓           │
│      (gap)         │
│        ↑           │
├────────────────────┤
│       Heap         │  ← dynamically allocated objects
├────────────────────┤
│       Data         │  ← global/static variables
├────────────────────┤
│       Code         │  ← your compiled instructions
└────────────────────┘  Low address
```

See `stack-and-heap.md` for a detailed breakdown of each region.

---

## RAM vs Disk

| RAM | Disk (SSD/HDD) |
|---|---|
| Temporary (lost on shutdown) | Persistent |
| Very fast (nanoseconds) | Slower (microseconds to milliseconds) |
| Small (8–64 GB typical) | Large (256 GB–10+ TB) |
| Used while program runs | Used for long-term storage |

Your code runs in RAM. Your files live on disk. When you open a file, the OS reads it from disk into RAM.

---

## What Happens When You Run Out of Memory?

- If the **stack** overflows (too many nested function calls), you get a **stack overflow**
- If the **heap** fills up, memory allocation fails — your program crashes or throws an out-of-memory error
- The OS can also use **virtual memory** (using disk as overflow RAM), but this is very slow
