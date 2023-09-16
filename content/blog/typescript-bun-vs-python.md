---
title: "TypeScript vs. Python: A Performance Exploration in Bun and Traditional Runtimes"
description: "How typescript with bun compares to python"
dateString: Set 2023
draft: false
tags: ["benchmark", "python", "typescript"]
weight: 102
cover:
    image: "/blog/typescript-bun-vs-python/header.jpg"
---

### Introduction
Bun is swiftly gaining traction in the JavaScript community. Promoted as a runtime with a keen emphasis on speed and crafted in Zig, it brings new possibilities for developers deeply rooted in the JavaScript ecosystem. Historically, I leaned towards TypeScript mainly for front-end development. 

However, Bun's performance promise is making me reconsider its use for server-side tasks. My backend projects predominantly use Python. In this analysis, I aim to draw a performance comparison between TypeScript executed in Bun and Python, offering insights into the potential shifts in the rapidly evolving tech landscape.

### Methodology
To grasp the full spectrum of performance for each language, we selected three distinct algorithms that embody common computational tasks:

- Recursive Fibonacci
- QuickSort
- Matrix Multiplication

These algorithms were implemented in both TypeScript and Python. The TypeScript code was run using Bun and Node, whereas the Python code was executed in the standard CPython interpreter (version 3.11) and PyPy3, a Just-In-Time (JIT) compiled version of Python.

Each algorithm was run multiple times to ensure consistent results. We employed the time command to measure the execution time for each run.

CPU: Intel i5-5200U (4) @ 2.700GHz <br>
Kernel: 6.2.0-32-generic

### Results

![graph](/blog/typescript-bun-vs-python/result.png)

| Environment                  | Time (seconds) |
|------------------------------|----------------|
| Python (CPython 3.11)        | 38.224         |
| TypeScript (Node.js+tsc)     | 14.952         |
| TypeScript (Node.js)         | 8.168          |
| Python (PyPy3)               | 5.563          |
| TypeScript (Bun)             | 1.975          |


### Conclusion
I'm quite impressed with the results from Bun, which managed to outperform Python's PyPy. Of course, choosing a programming language to solve a problem goes beyond just performance, but I'm thrilled to see a high-level language optimized in this way. Creating a project that optimizes both development time and performance is the best of both worlds


[The code](https://github.com/teixeirazeus/bun-vs-python)