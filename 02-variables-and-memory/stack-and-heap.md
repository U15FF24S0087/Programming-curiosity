# Stack and Heap

## Two Regions of Memory

Every running program has two main areas for storing data at runtime: the **stack** and the **heap**. They serve different purposes and behave very differently.

---

## The Stack

The stack is a region of memory that operates like a stack of plates — last in, first out (LIFO).

**What lives on the stack:**
- Local variables inside functions
- Function parameters
- Return addresses (where to go after a function finishes)

**How it works:**

```python
def greet(name):
    message = "Hello, " + name   # 'message' lives on the stack
    return message

def main():
    user = "Alice"               # 'user' lives on the stack
    result = greet(user)         # new stack frame created for greet()
    print(result)                # greet()'s frame is popped off
```

Each function call creates a new **stack frame**. When the function returns, that frame is popped off and the memory is instantly reclaimed.

**Characteristics:**
- Very fast — just move a pointer
- Automatically managed — no cleanup needed
- Limited size (typically 1–8 MB)
- Data is gone when the function returns

---

## The Heap

The heap is a large, unstructured region of memory used for dynamic allocation — when you need memory that outlives a single function call, or when the size isn't known at compile time.

**What lives on the heap:**
- Objects you create with `new` (Java, C++) or that are implicitly allocated (Python, JS)
- Large data structures (lists, dicts, strings)
- Anything that needs to persist across function calls

```python
def create_list():
    my_list = [1, 2, 3, 4, 5]   # allocated on the heap
    return my_list               # reference returned — heap data persists

data = create_list()
print(data)  # still accessible — heap data outlives the function
```

**Characteristics:**
- Slower — allocation requires finding a free block
- Manually managed in C/C++, automatically managed in most modern languages
- Much larger than the stack (limited only by available RAM)
- Can become fragmented over time

---

## Stack vs Heap Summary

| | Stack | Heap |
|---|---|---|
| Speed | Very fast | Slower |
| Size | Small (~MB) | Large (~GB) |
| Lifetime | Until function returns | Until explicitly freed (or GC) |
| Management | Automatic | Manual or GC |
| Access | LIFO | Random |
| Overflow | Stack overflow | Out-of-memory error |

---

## Pointers and References

When an object lives on the heap, your variable doesn't hold the object itself — it holds a **reference** (or pointer) to the object's memory address.

```
Stack                 Heap
┌─────────┐          ┌──────────────────┐
│  list → │ ──────►  │  [1, 2, 3, 4, 5] │
└─────────┘          └──────────────────┘
```

This is why in many languages, assigning an object to another variable doesn't copy it — both variables point to the same heap location.

```python
a = [1, 2, 3]
b = a          # b points to the SAME list
b.append(4)
print(a)       # [1, 2, 3, 4] — a was also "changed"
```

---

## Stack Overflow

When functions call other functions too deeply (or infinitely recurse), the stack grows until it runs out of space:

```python
def infinite():
    return infinite()  # each call adds a new frame

infinite()  # RecursionError: maximum recursion depth exceeded
```

This is called a **stack overflow** — the same error that inspired the name of the famous Q&A website.
