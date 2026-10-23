# Compiler

## What Is a Compiler?

A compiler is a program that translates your entire source code into machine code (or another lower-level form) before the program runs. The output is a standalone executable that can be run directly by the OS.

```
source.c  →  [compiler]  →  program.exe
                               ↓
                           run it later, as many times as you want
```

The compilation happens once. Running happens separately, and the original source code is no longer needed.

---

## What Does a Compiler Actually Do?

Compilation is a multi-stage pipeline:

### 1. Lexing (Tokenization)
The source text is broken into tokens — the smallest meaningful units.

```c
int x = 5 + 3;
```
Becomes: `[int] [x] [=] [5] [+] [3] [;]`

### 2. Parsing
Tokens are arranged into an **Abstract Syntax Tree (AST)** — a tree structure that captures the grammatical relationships.

```
      =
     / \
    x   +
       / \
      5   3
```

### 3. Semantic Analysis
The compiler checks that the code makes sense:
- Are variables declared before use?
- Are types compatible?
- Are functions called with the right number of arguments?

### 4. Optimization
The compiler rewrites the code to be more efficient without changing what it does:
- Remove unused variables
- Pre-compute constant expressions (`5 + 3` → `8`)
- Inline small functions

### 5. Code Generation
The optimized AST is translated into machine code (or assembly) for the target CPU architecture.

---

## Compiled Languages

| Language | Notes |
|---|---|
| C / C++ | Compiled to native machine code |
| Go | Compiles to a single static binary |
| Rust | Compiled, with ownership checked at compile time |
| Swift | Compiled via LLVM |

---

## Advantages of Compilation

- **Fast execution** — machine code runs directly on the CPU, no translation overhead at runtime
- **Errors caught early** — type errors, syntax issues surface before the program ever runs
- **Distributable** — ship the binary, not the source code
- **Optimization** — the compiler can spend time finding the best machine code

---

## Disadvantages

- **Slower development cycle** — must recompile after every change
- **Platform-specific** — a binary compiled for Windows won't run on Linux (without recompiling)
- **Longer build times** for large codebases

---

## Cross-Compilation

You can compile code on one platform (e.g., your Mac) to produce a binary for another (e.g., a Linux server). This is called **cross-compilation** and is common in embedded systems and mobile development.

---

## Compiler vs Interpreter

| | Compiler | Interpreter |
|---|---|---|
| When translation happens | Before running | While running |
| Speed | Faster at runtime | Slower at runtime |
| Error detection | At compile time | At runtime |
| Output | Binary/executable | No persistent output |

See `interpreter.md` for the other side of this comparison.
