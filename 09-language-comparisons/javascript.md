# JavaScript

## What Is JavaScript?

JavaScript is a high-level, dynamically typed language created by Brendan Eich in 1995 in just 10 days. Originally designed to make web pages interactive, it has grown into one of the most widely used languages in the world, running everywhere from browsers to servers to mobile devices.

```javascript
const numbers = [1, 2, 3, 4, 5];
const total = numbers.reduce((sum, n) => sum + n, 0);
console.log(`The total is ${total}`);
```

---

## Key Characteristics

| Property | JavaScript |
|---|---|
| Typing | Dynamic (no type declarations needed) |
| Execution | Interpreted + JIT compiled (V8 engine) |
| Memory management | Automatic (garbage collection) |
| Paradigm | Multi-paradigm (OOP via prototypes, functional, event-driven) |
| Concurrency | Single-threaded event loop + async/await |

---

## Why JavaScript Is Everywhere

JavaScript is the **only** programming language that runs natively in every web browser. If you want to make a web page interactive, you use JavaScript. Period.

This gave JavaScript an enormous advantage — every device with a browser can run it, which means every device.

### The Runtime Evolution
```
1995: Browser only (client-side interactivity)
2009: Node.js (server-side JavaScript)
2013: Electron (desktop apps — VS Code, Slack, Discord)
2015: React Native (mobile apps)
Now:   Everywhere
```

---

## Where JavaScript Shines

### Frontend Web Development
The dominant (only) language for browser interactivity:
- **React** — component-based UI library (Meta)
- **Vue** — progressive framework
- **Angular** — full framework (Google)
- **Svelte** — compile-time framework

### Backend Development (Node.js)
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.json({ message: "Hello, World!" });
});

app.listen(3000);
```
Same language on frontend and backend — "full-stack JavaScript."

### Mobile Development
- **React Native** — write JS, get native iOS/Android apps
- **Expo** — simplifies React Native development

---

## How JavaScript Runs

### In the Browser
```
Source JS → [Browser engine (V8, SpiderMonkey, JavaScriptCore)]
         → Parse → Bytecode → Interpret
         → JIT compile hot paths → native code
```

The V8 engine (Chrome, Node.js) uses JIT compilation to make JavaScript surprisingly fast — often competitive with compiled languages for many workloads.

### The Event Loop
JavaScript is **single-threaded** but handles concurrency via the event loop:

```javascript
console.log("1");

setTimeout(() => {
    console.log("3");  // runs after the timeout
}, 0);

console.log("2");
// Output: 1, 2, 3
```

Instead of blocking while waiting, JavaScript registers a callback and continues. When the operation completes, the callback runs. This is ideal for I/O-heavy applications (web servers, APIs).

### async/await
Modern syntax for handling async operations cleanly:
```javascript
async function getUser(id) {
    const response = await fetch(`/api/users/${id}`);
    const user = await response.json();
    return user;
}
```

---

## Strengths and Weaknesses

| Strengths | Weaknesses |
|---|---|
| Runs everywhere (browsers, servers, mobile, desktop) | Dynamic typing can lead to runtime errors |
| Massive ecosystem (npm — 2M+ packages) | Ecosystem churn (new frameworks constantly) |
| Same language frontend and backend | `this` binding is confusing |
| Async model perfect for I/O | Single-threaded — not for CPU-heavy work |
| Fast (with JIT) | No true parallelism (Web Workers help but limited) |

### TypeScript
JavaScript's dynamic typing can cause bugs at scale. **TypeScript** adds static types on top of JavaScript:
```typescript
function greet(name: string): string {
    return `Hello, ${name}`;
}

greet("Alice");  // OK
greet(42);       // Error: Argument of type 'number' is not assignable to 'string'
```
TypeScript compiles to plain JavaScript. It's increasingly the default for large projects.

---

## The Quirks of JavaScript

JavaScript has some famous oddities:
```javascript
typeof null          // "object" (historical bug, kept for compatibility)
0.1 + 0.2 === 0.3    // false (floating point)
[] + []              // "" (empty string)
[] + {}              // "[object Object]"
{} + []              // 0
"5" + 3              // "53" (string concatenation)
"5" - 3              // 2 (subtraction coerces to number)
```

These stem from rapid initial design and backward compatibility constraints. Modern JavaScript (ES6+) added many improvements (let/const, arrow functions, modules, classes, promises).

---

## Who Uses JavaScript

- Essentially every website with any interactivity
- **Netflix** — frontend and Node.js backend
- **Airbnb** — React frontend, Node.js services
- **Microsoft** — VS Code is built in TypeScript/Electron
- **Uber** — web platform
- **Every frontend developer on Earth**

See `python-vs-javascript-vs-java.md` for a comparison.
