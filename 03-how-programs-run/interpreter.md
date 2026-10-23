# Interpreter

## What Is an Interpreter?

An interpreter is a program that reads and executes source code directly, line by line, at runtime. There's no separate compilation step — you run the source file and the interpreter handles translation on the fly.

```
source.py  →  [Python interpreter reads + executes line by line]
```

---

## How It Works

```python
x = 5
y = 3
print(x + y)
```

1. Interpreter reads `x = 5` → allocates memory, stores 5
2. Reads `y = 3` → allocates memory, stores 3
3. Reads `print(x + y)` → looks up x and y, computes 8, prints it

Each line is parsed, translated to an internal representation, and executed before moving to the next.

---

## Pure Interpretation vs Bytecode

Most modern "interpreted" languages don't actually interpret source text directly — they first compile to an intermediate **bytecode**, then execute that bytecode on a virtual machine.

**Python example:**
```
source.py  →  [compiler]  →  bytecode (.pyc)  →  [Python VM executes]
```

This is faster than interpreting raw text, but the bytecode still requires a runtime to execute (unlike a fully compiled binary).

**Java is similar** — compiled to `.class` bytecode, executed by the JVM. But Java is typically called "compiled" because the programmer explicitly runs a compile step.

---

## JIT Compilation (Just-In-Time)

Modern interpreters often use **JIT compilation** to close the performance gap with compiled languages:

1. The interpreter starts executing bytecode normally
2. It monitors which code runs frequently ("hot paths")
3. Those hot paths are compiled to native machine code on the fly
4. Subsequent calls hit the compiled version directly

JavaScript engines (V8, SpiderMonkey) and the JVM use JIT heavily. This is why JavaScript can be surprisingly fast despite being "interpreted."

---

## Interpreted Languages

| Language | Notes |
|---|---|
| Python | Bytecode + CPython VM |
| Ruby | Bytecode + YARV VM |
| JavaScript | Bytecode + JIT (V8, etc.) |
| PHP | Interpreted / bytecode |
| Bash | Line-by-line interpretation |

---

## Advantages of Interpretation

- **Fast development** — no compile step, just run the file
- **Portability** — the same source code runs anywhere the interpreter is installed
- **Interactive** — can run code in a REPL (Read-Eval-Print Loop), great for exploration
- **Dynamic features** — easier to implement dynamic typing, eval, metaprogramming

---

## Disadvantages

- **Slower at runtime** — translation overhead at execution time (mitigated by JIT)
- **Errors surface later** — a bug in line 500 isn't found until that line actually runs
- **Requires runtime to be installed** — can't ship a standalone binary easily
- **Less optimization opportunity** — the interpreter doesn't see the whole program upfront

---

## REPL

One of the best features of interpreted languages is the **REPL** (Read-Eval-Print Loop) — an interactive shell where you can type code and see results immediately.

```python
>>> 2 + 2
4
>>> name = "Alice"
>>> print("Hello, " + name)
Hello, Alice
```

This makes exploration and debugging much faster. Most interpreted languages have one: `python`, `node`, `irb` (Ruby), `ghci` (Haskell).
