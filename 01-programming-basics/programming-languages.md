# Programming Languages

## What Is a Programming Language?

A programming language is a formal set of rules and syntax used to write instructions for a computer. Like human languages, they have vocabulary (keywords) and grammar (syntax). Unlike human languages, they are completely unambiguous — every statement has exactly one meaning.

---

## Why Are There So Many Languages?

Different languages were designed for different purposes:

- Some prioritize **speed** (C, Rust)
- Some prioritize **ease of use** (Python, Ruby)
- Some target **web browsers** (JavaScript)
- Some target **enterprise systems** (Java, C#)
- Some are designed for **data and statistics** (R, Julia)

No single language is best for everything. Choosing a language is an engineering tradeoff.

---

## Levels of Abstraction

Languages exist on a spectrum from low-level (close to hardware) to high-level (close to human thinking):

```
Machine Code  →  Assembly  →  C  →  C++  →  Java/C#  →  Python/Ruby
   (lowest)                                               (highest)
```

**Low-level languages:**
- More control over hardware
- Faster, but harder to write
- Example: C, Assembly

**High-level languages:**
- More readable and expressive
- Slower (relatively), but much faster to develop with
- Example: Python, JavaScript

---

## Compiled vs Interpreted

Languages are also categorized by how they run:

- **Compiled**: Source code is transformed into machine code before running (C, Go, Rust)
- **Interpreted**: Source code is read and executed line-by-line at runtime (Python, Ruby)
- **Hybrid**: Compiled to an intermediate bytecode, then interpreted (Java, C#)

See `../03-how-programs-run/compiler.md` and `interpreter.md` for details.

---

## Programming Paradigms

Languages also encourage different styles of thinking:

| Paradigm | Description | Example Languages |
|---|---|---|
| Imperative | Step-by-step instructions | C, Python |
| Object-Oriented | Model the world as objects | Java, C++ |
| Functional | Compose pure functions, avoid state | Haskell, Elixir |
| Declarative | Describe what you want, not how | SQL, HTML |

Most modern languages support multiple paradigms.

---

## Choosing a Language

For a beginner, the best language is whichever one:
1. Has a large learning community
2. Matches your goal (web? data? apps?)
3. You can actually get started with today

Python and JavaScript are the most common starting points — both have enormous ecosystems and are applicable across many domains.
