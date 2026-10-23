# Variables

## What Is a Variable?

A variable is a named container for storing a value. Instead of writing the same value over and over, you give it a name and refer to that name throughout your code.

```python
age = 25
name = "Alice"
is_logged_in = True
```

The name (`age`) points to a location in memory where the value (`25`) is stored.

---

## Why Variables Exist

Without variables, code would be unreadable and unmaintainable:

```python
# Without variables
print(3.14159 * 5 * 5)

# With variables
pi = 3.14159
radius = 5
area = pi * radius * radius
print(area)
```

Variables make code:
- **Readable** — names communicate intent
- **Reusable** — change the value in one place
- **Maintainable** — no magic numbers scattered everywhere

---

## Data Types

Every variable holds a value of a particular type. Common types:

| Type | Example | Description |
|---|---|---|
| Integer | `42` | Whole numbers |
| Float | `3.14` | Decimal numbers |
| String | `"hello"` | Text |
| Boolean | `True` / `False` | True or false |
| List/Array | `[1, 2, 3]` | Ordered collection |
| Object/Dict | `{"key": "value"}` | Key-value pairs |
| Null/None | `None` / `null` | Absence of a value |

Languages handle types differently:
- **Statically typed** (Java, C): you must declare the type explicitly
- **Dynamically typed** (Python, JavaScript): the type is inferred at runtime

---

## Variable Scope

Scope determines where a variable is accessible:

```python
x = 10  # global scope — accessible everywhere

def my_function():
    y = 20  # local scope — only accessible inside this function
    print(x)  # can access global x
    print(y)  # can access local y

my_function()
print(y)  # ERROR — y doesn't exist here
```

**Global scope**: accessible throughout the program  
**Local scope**: accessible only within a function or block

---

## Mutability

Some variables can be changed after creation (mutable), others cannot (immutable):

```python
# Mutable
my_list = [1, 2, 3]
my_list.append(4)  # [1, 2, 3, 4] — changed in place

# Immutable
my_string = "hello"
my_string[0] = "H"  # ERROR — strings are immutable in Python
```

Understanding mutability is important for avoiding subtle bugs, especially when passing values between functions.

---

## Under the Hood

When you write `age = 25`:
1. The computer allocates a small piece of memory
2. Stores the value `25` there
3. Associates the name `age` with that memory address

When you later write `print(age)`, the program looks up the memory address for `age` and reads the value stored there.

See `memory.md` and `stack-and-heap.md` for how this plays out at the hardware level.
