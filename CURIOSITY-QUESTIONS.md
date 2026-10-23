# Programming Curiosity Challenge

> The purpose is to make you start asking:
> **"But how does this actually work?"**

Don't search for the answers immediately. Pick questions, discuss them with your team, make your own assumptions, and then find the real answer.

Each question below links to the topic file that explores it. Read the question first, think about it, then follow the link.

---

## The 7-Question Framework

Whenever you learn something, ask 7 questions:

**WHY? → WHAT? → HOW? → WHERE? → WHEN? → WHO? → WHAT IF?**

For example:

> "I learned MongoDB."

Don't stop there.

- **WHAT** is MongoDB?
- **WHY** do we use it?
- **HOW** does it store data?
- **WHERE** is the data physically stored?
- **WHEN** should I choose MongoDB?
- **WHO** uses it in the industry?
- **WHAT IF** MongoDB goes down?

A good programmer doesn't just learn answers. A good programmer develops better questions.

---

## 🌍 1. The Programming World

1. What exactly is programming? → [why-programming.md](01-programming-basics/why-programming.md)
2. Why was programming invented? → [why-programming.md](01-programming-basics/why-programming.md)
3. Why can't humans directly communicate with a computer? → [programming-languages.md](01-programming-basics/programming-languages.md)
4. How does a computer understand if, for, while and function? → [how-code-runs.md](01-programming-basics/how-code-runs.md)
5. Who decides what a programming language can do? → [programming-languages.md](01-programming-basics/programming-languages.md)
6. Why are there hundreds of programming languages? → [programming-languages.md](01-programming-basics/programming-languages.md)
7. Why do we need Python, Java, JavaScript, C, C++, Go, Rust, etc.? → [programming-languages.md](01-programming-basics/programming-languages.md)
8. What makes one programming language different from another? → [programming-languages.md](01-programming-basics/programming-languages.md)
9. Why does a programming language need a compiler or interpreter? → [compiler.md](03-how-programs-run/compiler.md) · [interpreter.md](03-how-programs-run/interpreter.md)
10. What actually happens when you run a program? → [how-code-runs.md](01-programming-basics/how-code-runs.md)

---

## 💻 2. Your First Line of Code

11. What happens internally when you write `x = 10`? → [variables.md](02-variables-and-memory/variables.md)
12. Where is 10 stored? → [memory.md](02-variables-and-memory/memory.md)
13. Where is x stored? → [variables.md](02-variables-and-memory/variables.md) · [stack-and-heap.md](02-variables-and-memory/stack-and-heap.md)
14. Is x actually the value 10? → [variables.md](02-variables-and-memory/variables.md)
15. What happens when you write `x = 10` then `x = 20`? → [variables.md](02-variables-and-memory/variables.md)
16. What happens to 10? → [garbage-collection.md](02-variables-and-memory/garbage-collection.md)
17. What happens when you write `a = b = c = 100`? → [variables.md](02-variables-and-memory/variables.md) · [stack-and-heap.md](02-variables-and-memory/stack-and-heap.md)
18. Are a, b and c three copies of 100? → [stack-and-heap.md](02-variables-and-memory/stack-and-heap.md)
19. What happens to variables when a function finishes? → [stack-and-heap.md](02-variables-and-memory/stack-and-heap.md)
20. Who cleans unused memory? → [garbage-collection.md](02-variables-and-memory/garbage-collection.md)

---

## 🧠 3. Memory — The Hidden World

21. What is RAM actually doing while your program runs? → [memory.md](02-variables-and-memory/memory.md)
22. What is the difference between RAM and storage? → [memory.md](02-variables-and-memory/memory.md)
23. Where does a Python variable live? → [stack-and-heap.md](02-variables-and-memory/stack-and-heap.md)
24. What is a memory address? → [memory.md](02-variables-and-memory/memory.md)
25. Why can't we normally see memory addresses in Python? → [memory.md](02-variables-and-memory/memory.md)
26. What is a stack? → [stack-and-heap.md](02-variables-and-memory/stack-and-heap.md)
27. What is a heap? → [stack-and-heap.md](02-variables-and-memory/stack-and-heap.md)
28. What is garbage collection? → [garbage-collection.md](02-variables-and-memory/garbage-collection.md)
29. Why doesn't unused memory remain forever? → [garbage-collection.md](02-variables-and-memory/garbage-collection.md)
30. What happens when your program consumes too much memory? → [memory.md](02-variables-and-memory/memory.md)

---

## ⚙️ 4. When You Run a Program

Suppose you execute: `python app.py`

31. Who receives this command? → [process.md](03-how-programs-run/process.md)
32. How does the operating system know where Python is? → [process.md](03-how-programs-run/process.md)
33. What is a process? → [process.md](03-how-programs-run/process.md)
34. What is a thread? → [threads.md](03-how-programs-run/threads.md)
35. Where does the Python interpreter run? → [interpreter.md](03-how-programs-run/interpreter.md) · [process.md](03-how-programs-run/process.md)
36. How does Python read your .py file? → [interpreter.md](03-how-programs-run/interpreter.md) · [compiler.md](03-how-programs-run/compiler.md)
37. Does Python directly understand your source code? → [interpreter.md](03-how-programs-run/interpreter.md)
38. What is bytecode? → [interpreter.md](03-how-programs-run/interpreter.md) · [compiler.md](03-how-programs-run/compiler.md)
39. What is the Python Virtual Machine? → [interpreter.md](03-how-programs-run/interpreter.md)
40. What happens when the program crashes? → [process.md](03-how-programs-run/process.md)

---

## 🌐 5. What Actually Happens on the Internet?

41. What happens when you type google.com into a browser? → [dns.md](04-internet/dns.md) · [http.md](04-internet/http.md)
42. How does your computer find Google's server? → [dns.md](04-internet/dns.md) · [ip.md](04-internet/ip.md)
43. What is DNS? → [dns.md](04-internet/dns.md)
44. What is an IP address? → [ip.md](04-internet/ip.md)
45. Why do we need ports? → [ip.md](04-internet/ip.md) · [http.md](04-internet/http.md) · [server.md](08-cloud-and-deployment/server.md)
46. What is HTTP? → [http.md](04-internet/http.md)
47. Why was HTTPS created? → [https.md](04-internet/https.md)
48. What happens when you click a button on a website? → [http.md](04-internet/http.md) · [request-response.md](05-apis/request-response.md)
49. How does your request travel thousands of kilometres? → [ip.md](04-internet/ip.md)
50. How does the server know where to send the response? → [ip.md](04-internet/ip.md) · [http.md](04-internet/http.md)

---

## 🔌 6. APIs — The Invisible Communication

51. What exactly is an API? → [what-is-api.md](05-apis/what-is-api.md)
52. Why can't two applications simply access each other's variables? → [what-is-api.md](05-apis/what-is-api.md) · [process.md](03-how-programs-run/process.md)
53. What happens when a React application calls a Node/Python API? → [request-response.md](05-apis/request-response.md) · [rest-api.md](05-apis/rest-api.md)
54. What is a request? → [request-response.md](05-apis/request-response.md)
55. What is a response? → [request-response.md](05-apis/request-response.md)
56. Why do APIs commonly use JSON? → [request-response.md](05-apis/request-response.md)
57. What are HTTP methods? → [http.md](04-internet/http.md) · [rest-api.md](05-apis/rest-api.md)
58. Why do we have GET, POST, PUT, PATCH and DELETE? → [rest-api.md](05-apis/rest-api.md)
59. What happens if an API takes 10 seconds to respond? → [threads.md](03-how-programs-run/threads.md) · [scaling.md](08-cloud-and-deployment/scaling.md)
60. What happens if the API server is completely down? → [server.md](08-cloud-and-deployment/server.md) · [scaling.md](08-cloud-and-deployment/scaling.md)

---

## 🗄️ 7. Databases

61. Why can't we simply store everything in Python variables? → [sql.md](06-databases/sql.md) · [memory.md](02-variables-and-memory/memory.md)
62. Why do applications need databases? → [sql.md](06-databases/sql.md)
63. What happens when you click "Save"? → [transactions.md](06-databases/transactions.md)
64. Where does the data physically go? → [sql.md](06-databases/sql.md) · [server.md](08-cloud-and-deployment/server.md)
65. What is a table? → [sql.md](06-databases/sql.md)
66. What is a row? → [sql.md](06-databases/sql.md)
67. What is a column? → [sql.md](06-databases/sql.md)
68. Why do we need primary keys? → [sql.md](06-databases/sql.md) · [indexes.md](06-databases/indexes.md)
69. Why do we need indexes? → [indexes.md](06-databases/indexes.md)
70. What is the difference between SQL and NoSQL? → [sql.md](06-databases/sql.md) · [nosql.md](06-databases/nosql.md)

---

## 🔐 8. Security Curiosity

71. How does a website know who you are after login? → [authentication.md](07-security/authentication.md)
72. Where is your password stored? → [hashing.md](07-security/hashing.md) · [authentication.md](07-security/authentication.md)
73. Why shouldn't passwords be stored as plain text? → [hashing.md](07-security/hashing.md)
74. What is encryption? → [encryption.md](07-security/encryption.md)
75. What is hashing? → [hashing.md](07-security/hashing.md)
76. What happens if someone changes an API request? → [encryption.md](07-security/encryption.md) · [https.md](04-internet/https.md)
77. What is CORS? → [authorization.md](07-security/authorization.md) · [what-is-api.md](05-apis/what-is-api.md)
78. How does a server know whether you are authorized? → [authorization.md](07-security/authorization.md)
79. What is a token? → [authentication.md](07-security/authentication.md)
80. How can one careless line of code expose millions of users? → [authorization.md](07-security/authorization.md) · [authentication.md](07-security/authentication.md)

---

## 🐍 9. Language Comparison

81. Why is Python popular for AI? → [python.md](09-language-comparisons/python.md)
82. Why is JavaScript used in browsers? → [javascript.md](09-language-comparisons/javascript.md)
83. Why is Node.js able to run JavaScript on servers? → [javascript.md](09-language-comparisons/javascript.md)
84. Why is Java still heavily used in enterprise applications? → [java.md](09-language-comparisons/java.md)
85. Why is C still important? → [programming-languages.md](01-programming-basics/programming-languages.md)
86. Why is Rust becoming popular? → [garbage-collection.md](02-variables-and-memory/garbage-collection.md) · [python-vs-javascript-vs-java.md](09-language-comparisons/python-vs-javascript-vs-java.md)
87. Is Python actually slower than Java? → [python-vs-javascript-vs-java.md](09-language-comparisons/python-vs-javascript-vs-java.md)
88. Does "faster language" always mean "better language"? → [python-vs-javascript-vs-java.md](09-language-comparisons/python-vs-javascript-vs-java.md)
89. Can Python call code written in C? → [python.md](09-language-comparisons/python.md)
90. Can different programming languages communicate with each other? → [what-is-api.md](05-apis/what-is-api.md) · [python-vs-javascript-vs-java.md](09-language-comparisons/python-vs-javascript-vs-java.md)

---

## ☁️ 10. Real-World Software

91. Where does your application actually run after deployment? → [server.md](08-cloud-and-deployment/server.md)
92. What happens when 1 user uses your application? → [server.md](08-cloud-and-deployment/server.md)
93. What changes when 1 million users use it? → [scaling.md](08-cloud-and-deployment/scaling.md)
94. Why can't we simply buy one huge computer? → [scaling.md](08-cloud-and-deployment/scaling.md)
95. What is a server? → [server.md](08-cloud-and-deployment/server.md)
96. What is cloud computing? → [cloud.md](08-cloud-and-deployment/cloud.md)
97. What is AWS/Azure actually providing? → [cloud.md](08-cloud-and-deployment/cloud.md)
98. What happens when a server crashes? → [server.md](08-cloud-and-deployment/server.md) · [scaling.md](08-cloud-and-deployment/scaling.md)
99. How does Netflix/YouTube/Instagram handle millions of users? → [scaling.md](08-cloud-and-deployment/scaling.md) · [cloud.md](08-cloud-and-deployment/cloud.md)
100. If you had to build WhatsApp from scratch, what would you need? → [scaling.md](08-cloud-and-deployment/scaling.md) · [what-is-api.md](05-apis/what-is-api.md) · [authentication.md](07-security/authentication.md)

---

## 🔥 The 10 Rabbit Hole Questions

These are the deep-dive questions that connect multiple concepts. Each one traces a path through the entire stack.

1. **When I type `x = 10`, where exactly does 10 go?**
   Trace the path: Variable → Object → Memory → RAM → Process → OS
   → [variables.md](02-variables-and-memory/variables.md) → [memory.md](02-variables-and-memory/memory.md) → [stack-and-heap.md](02-variables-and-memory/stack-and-heap.md) → [process.md](03-how-programs-run/process.md)
   → Full narrative: [what-happens-when-you-run-code.md](10-rabbit-holes/what-happens-when-you-run-code.md)

2. **When I type `python app.py`, what happens between pressing Enter and seeing the output?**
   → [what-happens-when-you-run-code.md](10-rabbit-holes/what-happens-when-you-run-code.md)

3. **When I enter google.com, how does my computer find another computer somewhere in the world?**
   → [what-happens-when-you-open-google.md](10-rabbit-holes/what-happens-when-you-open-google.md)

4. **When I click Login, how does my password travel from my phone to the server and come back as "Login successful"?**
   → [how-a-login-works.md](10-rabbit-holes/how-a-login-works.md)

5. **When I upload a photo to Instagram, where does that photo actually go?**
   → [server.md](08-cloud-and-deployment/server.md) · [cloud.md](08-cloud-and-deployment/cloud.md) · [https.md](04-internet/https.md)

6. **How can millions of people use the same application at the same time?**
   → [scaling.md](08-cloud-and-deployment/scaling.md) · [cloud.md](08-cloud-and-deployment/cloud.md) · [threads.md](03-how-programs-run/threads.md)

7. **What happens if two users try to modify the same database record at exactly the same time?**
   → [transactions.md](06-databases/transactions.md)

8. **How does a program know that another program has crashed?**
   → [process.md](03-how-programs-run/process.md) · [threads.md](03-how-programs-run/threads.md)

9. **If Python, JavaScript and Java are completely different languages, how can they communicate with each other?**
   → [what-is-api.md](05-apis/what-is-api.md) · [request-response.md](05-apis/request-response.md) · [python-vs-javascript-vs-java.md](09-language-comparisons/python-vs-javascript-vs-java.md)

10. **If the Internet disappeared tomorrow, which parts of your application would still work?**
    → [server.md](08-cloud-and-deployment/server.md) · [cloud.md](08-cloud-and-deployment/cloud.md) · [sql.md](06-databases/sql.md)

---

## How to Use This Repository

1. **Pick a question** that makes you curious
2. **Don't look at the answer yet** — think about it, discuss it, write down your assumptions
3. **Follow the link** to the topic file
4. **Read and compare** — was your assumption right? What surprised you?
5. **Ask the 7 follow-up questions**: WHY? WHAT? HOW? WHERE? WHEN? WHO? WHAT IF?
6. **Go deeper** — follow cross-references to related topics
7. **Repeat** — curiosity is a loop, not a destination
