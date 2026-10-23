# Process

## What Is a Process?

A process is a running instance of a program. When you double-click an app or run a command, the OS creates a process to manage its execution.

The program on disk is just a file. The process is that program brought to life — with its own memory, CPU time, and resources.

---

## What a Process Contains

The OS tracks each process with a **Process Control Block (PCB)**, which stores:

- **Process ID (PID)** — unique identifier
- **State** — running, waiting, sleeping, zombie
- **Memory map** — the layout of its memory regions (code, stack, heap, data)
- **Open file handles** — which files the process has open
- **CPU registers** — the current execution state
- **Priority** — how much CPU time it should get

---

## Process Memory Layout

Every process gets its own private virtual address space:

```
┌────────────────────┐
│      Stack         │  ← grows downward (local vars, function frames)
├────────────────────┤
│        ↓  ↑        │
│      (gap)         │
├────────────────────┤
│       Heap         │  ← grows upward (dynamic allocation)
├────────────────────┤
│  Data Segment      │  ← global and static variables
├────────────────────┤
│  Code Segment      │  ← the compiled instructions
└────────────────────┘
```

Processes are isolated — one process can't read or write another process's memory (without special OS permissions). This is a key security boundary.

---

## Process Lifecycle

```
        created
           ↓
        [New]
           ↓
       [Ready]  ←──────────────────────┐
           ↓                           │
       [Running]  ── I/O wait ──►  [Waiting]
           ↓
       [Terminated]
```

- **New**: Process is being created
- **Ready**: Waiting for CPU time
- **Running**: Currently executing on a CPU core
- **Waiting**: Paused, waiting for I/O (disk read, network response, etc.)
- **Terminated**: Finished, resources being cleaned up

The OS **scheduler** decides which ready processes get CPU time and for how long.

---

## Context Switching

Modern computers run many processes "simultaneously" by rapidly switching between them. This is called **context switching**.

When the scheduler switches from Process A to Process B:
1. Save A's CPU registers and state
2. Load B's CPU registers and state
3. B resumes from exactly where it left off

This happens hundreds of times per second, creating the illusion of parallelism even on a single CPU core.

---

## Process vs Thread

A process is heavyweight — it has its own memory space and resources.

A **thread** is a lighter unit of execution that lives inside a process. Multiple threads share the same memory space.

| | Process | Thread |
|---|---|---|
| Memory | Own private space | Shared with parent process |
| Creation cost | High | Low |
| Isolation | Strong | Weak (shared bugs) |
| Communication | Complex (IPC) | Simple (shared memory) |

See `threads.md` for more.

---

## Inter-Process Communication (IPC)

Processes are isolated, so they need special mechanisms to communicate:

- **Pipes** — one process writes, another reads (like `ls | grep .txt`)
- **Sockets** — network-style communication, even on localhost
- **Shared memory** — a mapped region both processes can access
- **Message queues** — structured messages passed through the OS
- **Files** — one process writes a file, another reads it (simple but slow)
