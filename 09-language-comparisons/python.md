# Python

## What Is Python?

Python is a high-level, dynamically typed, interpreted programming language created by Guido van Rossum in 1991. Its design philosophy emphasizes readability and simplicity — the code looks almost like plain English.

```python
numbers = [1, 2, 3, 4, 5]
total = sum(numbers)
print(f"The total is {total}")
```

---

## Key Characteristics

| Property | Python |
|---|---|
| Typing | Dynamic (no type declarations needed) |
| Execution | Interpreted (bytecode + CPython VM) |
| Memory management | Automatic (reference counting + GC) |
| Paradigm | Multi-paradigm (OOP, functional, procedural) |
| Indentation | Significant (no braces, whitespace defines blocks) |

---

## Why People Use Python

- **Readable**: minimal syntax, few special characters
- **Batteries included**: large standard library
- **Huge ecosystem**: PyPI has 500,000+ packages
- **Versatile**: web, data science, AI/ML, automation, scripting, gaming
- **Beginner-friendly**: common first language in schools and bootcamps
- **Strong community**: abundant tutorials, Stack Overflow answers, courses

---

## Where Python Shines

### Data Science and Machine Learning
Python is the dominant language for data work, powered by libraries like:
- **NumPy** — numerical computing
- **Pandas** — data analysis
- **Matplotlib** — visualization
- **Scikit-learn** — traditional ML
- **PyTorch / TensorFlow** — deep learning

### Automation and Scripting
```python
import os

for file in os.listdir("."):
    if file.endswith(".txt"):
        print(f"Found text file: {file}")
```

Quick to write, runs anywhere Python is installed.

### Web Development
- **Django** — full-featured web framework ("batteries included")
- **Flask** — lightweight, minimal web framework
- **FastAPI** — modern, fast, async web framework with auto-generated docs

---

## How Python Runs

```
source.py → [Python compiler] → bytecode (.pyc) → [CPython VM] → execution
```

Python compiles to bytecode behind the scenes, then the CPython interpreter executes the bytecode. This makes Python portable (same .pyc runs anywhere CPython is installed) but slower than compiled languages.

---

## Strengths and Weaknesses

| Strengths | Weaknesses |
|---|---|
| Easy to read and write | Slower than compiled languages |
| Huge ecosystem | Global Interpreter Lock (GIL) limits true threading |
| Great for prototyping | Mobile development is weak (Kivy exists but niche) |
| Dominant in AI/ML/data | Not ideal for embedded systems |
| Cross-platform | Packaging/distribution can be tricky |

### The GIL (Global Interpreter Lock)
CPython has a lock that allows only one thread to execute Python code at a time. This means Python threads can't use multiple CPU cores for CPU-bound work. For parallelism, you use **multiprocessing** (separate processes, not threads) or C extensions that release the GIL.

---

## Python Code Example

```python
# A simple class
class Dog:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed

    def bark(self):
        return f"{self.name} says woof!"

# Using it
rex = Dog("Rex", "Labrador")
print(rex.bark())  # Rex says woof!

# List comprehension
squares = [x ** 2 for x in range(10)]
print(squares)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

---

## Who Uses Python

- **Google** — originally, many core systems used Python
- **Instagram** — backend runs on Django
- **Netflix** — data analysis and microservices
- **Spotify** — data pipelines and backend services
- **NASA** — scientific computing
- **Researchers** — scientific computing, data analysis

See `python-vs-javascript-vs-java.md` for a head-to-head comparison.
