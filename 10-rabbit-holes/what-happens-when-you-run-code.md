# What Happens When You Run Code

## The Full Journey

You write `python hello.py` and press Enter. In the fraction of a second before "Hello, World!" appears, an astonishing number of things happen. Let's trace the full path.

---

## Step 1: The Shell Parses Your Command

When you type `python hello.py` in a terminal:

1. The **shell** (bash, PowerShell, cmd) reads your input
2. It parses the command into: program name (`python`) + arguments (`hello.py`)
3. The shell asks the OS to find the `python` executable by searching your `PATH`

```
PATH = /usr/bin:/usr/local/bin:/home/alice/.local/bin:...
         ↓
/usr/local/bin/python → found!
```

---

## Step 2: The OS Creates a Process

The OS (`execve` system call) launches the Python interpreter as a new **process**:

1. Allocates a new **process control block** (PID, state, memory map)
2. Loads the `python` executable into memory (code segment)
3. Sets up the process memory layout (stack, heap, data, code segments)
4. Opens `hello.py` as a file, reads it into memory
5. Starts executing the interpreter at its entry point

```
OS creates process
    ↓
Loads python executable into memory
    ↓
Python reads hello.py into memory
    ↓
Python begins executing
```

---

## Step 3: Python Compiles to Bytecode

Python doesn't execute your source text directly. It compiles it first:

### 3a. Lexing (Tokenization)
```
print("Hello, World!")
```
Becomes tokens: `[NAME: print] [LPAREN] [STRING: "Hello, World!"] [RPAREN] [NEWLINE]`

### 3b. Parsing
Tokens become an **Abstract Syntax Tree (AST)**:
```
  ExprStatement
      │
   Call
   /    \
 Name    Constant
(print)  ("Hello, World!")
```

### 3c. Bytecode Compilation
The AST is compiled to **bytecode** — a lower-level representation:
```
LOAD_NAME     print        # push the print function
LOAD_CONST    "Hello..."   # push the string
CALL_FUNCTION 1            # call print with 1 argument
POP_TOP                    # discard the return value
```

Python caches this bytecode as a `.pyc` file so it doesn't have to recompile next time (unless the source changes).

---

## Step 4: The CPython VM Executes Bytecode

The Python Virtual Machine is a big loop that reads bytecode instructions one at a time and executes them:

```
while True:
    instruction = fetch_next_bytecode()
    execute(instruction)
```

For `LOAD_NAME print`:
1. Look up `print` in the local namespace
2. Not found? Check global namespace
3. Not found? Check built-in namespace → found! Push onto the stack

For `LOAD_CONST "Hello, World!"`:
1. Push the string object onto the stack

For `CALL_FUNCTION 1`:
1. Pop the string and the function off the stack
2. Call the C function that implements `print()`

---

## Step 5: print() Talks to the OS

`print()` needs to write text to your screen. But Python itself can't draw pixels on a monitor — it asks the OS to do it:

1. Python calls the C library function `puts()` or `write()`
2. That calls the OS **system call** `write(fd=1, buffer="Hello, World!\n")`
3. The OS kernel receives the syscall

**fd=1** is **stdout** — the standard output file descriptor. By convention, process output goes to file descriptor 1.

---

## Step 6: The Kernel Writes to the Terminal

The OS kernel:
1. Checks that file descriptor 1 is valid and writable
2. File descriptor 1 is connected to your terminal (or terminal emulator)
3. The kernel writes the bytes `"Hello, World!\n"` to the terminal device
4. The terminal emulator receives the bytes
5. The terminal renders the text as pixels on your screen

```
Python → C library → syscall → kernel → terminal driver → terminal emulator → pixels on screen
```

---

## Step 7: The Process Ends

After `print()` finishes, Python:
1. Has no more bytecode to execute
2. Begins the interpreter shutdown sequence
3. Runs any cleanup hooks (`atexit` functions, `__del__` methods)
4. The OS reclaims all memory (stack, heap, code segments)
5. Closes all file descriptors
6. The process transitions to "terminated" state
7. The shell (which was waiting) sees the process exited and displays a new prompt

---

## The Full Timeline

```
You press Enter
    ↓ (~1ms)
Shell parses command, finds python
    ↓ (~1ms)
OS creates process, loads python into memory
    ↓ (~5ms)
Python reads hello.py from disk
    ↓ (~1ms)
Lexing → parsing → AST
    ↓ (~1ms)
Bytecode compilation
    ↓ (~0.1ms)
CPython VM executes bytecode
    ↓ (~0.01ms)
print() → C library → syscall
    ↓ (~0.01ms)
Kernel writes to terminal
    ↓ (~1ms)
Terminal renders pixels
    ↓
"Hello, World!" appears on screen
    ↓ (~0.1ms)
Python exits, OS reclaims memory
    ↓
Shell shows new prompt
```

Total: about 10-30 milliseconds from Enter to output. And that's considered "slow" for a compiled language where the same output would appear in under 1ms.

---

## What Makes This Mind-Blowing

You typed 21 characters. In response:
- A process was created
- Memory was allocated and laid out
- Your text was tokenized, parsed, and compiled
- A virtual machine was spun up
- Bytecode was executed
- A system call crossed the user-kernel boundary
- The kernel talked to hardware drivers
- Pixels were painted on your screen
- All of that was cleaned up and freed

Every time you run a program, this entire chain executes. We just never see it because it happens so fast.
