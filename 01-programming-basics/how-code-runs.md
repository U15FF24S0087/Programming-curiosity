# How Code Runs

## The Journey from Text to Execution

When you write code, you're writing text in a file. That text can't run directly — a CPU only understands binary instructions (machine code). So there's a translation process that turns your human-readable code into something the hardware can execute.

---

## Step 1: You Write Source Code

```python
name = "Alice"
print("Hello, " + name)
```

This is just a text file. It has no special powers yet. The computer has no idea what to do with it as-is.

---

## Step 2: Translation

Depending on the language, this translation happens in one of two ways:

### Compiled
A **compiler** reads your entire source file and translates it into machine code (a `.exe`, a binary) before you run it.

```
source.c  →  [compiler]  →  program.exe  →  run
```

### Interpreted
An **interpreter** reads your source code line by line and executes it directly, translating on the fly.

```
source.py  →  [interpreter reads + runs simultaneously]
```

### Bytecode (hybrid)
Some languages (Java, Python) compile to an intermediate format called **bytecode**, then run that bytecode on a virtual machine.

```
source.java  →  [compiler]  →  bytecode (.class)  →  [JVM]  →  runs
```

---

## Step 3: The Operating System Gets Involved

When you run a program, the OS:
1. Loads the program into memory (RAM)
2. Creates a **process** to manage it
3. Allocates resources (CPU time, memory, file handles)
4. Starts execution at the program's entry point (e.g., `main()`)

---

## Step 4: The CPU Executes Instructions

The CPU fetches instructions from memory one at a time, decodes them, and executes them. This is the **fetch-decode-execute cycle** — happening billions of times per second.

---

## What Happens to Your Variables?

When your code runs:
- Variables get stored in **memory** (RAM)
- The CPU reads and writes these memory locations as it executes your instructions
- When the program ends, that memory is freed

See `../02-variables-and-memory/` for a deeper look.

---

## Summary

```
You write text
    ↓
Compiler/Interpreter translates it
    ↓
OS loads it into memory and creates a process
    ↓
CPU executes instructions one by one
    ↓
Program produces output or side effects
```

The whole thing happens in milliseconds (or less).
