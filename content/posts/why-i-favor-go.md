---
title: "Why Go Became My Favorite Language for PhD Research"
date: 2026-09-08T19:19:18+08:00
draft: false
tags: ["programming language"]
---

I need something dramatically faster than Python, while minimizing the amount of my PhD that I spend engineering software.

As a PhD researcher, I have gradually settled on Go as my favorite general-purpose programming language. I use it for prototyping small research tools, validating ideas, building utilities, and even writing script-like programs that glue different parts of my workflow together.

The appeal is simple: I can stay inside one ecosystem and use essentially one language for almost everything.

## The sweet spot

For me, Go hits a particularly useful sweet spot between **ease of development and performance**.

It is statically typed and compiled to native code, so its performance is dramatically better than Python for CPU-heavy workloads. It provides a relatively large standard library and ecosystem of tools that make it easy to get started. At the same time, it avoids many of the engineering costs that come with C++ and Rust. 

A comparison of the quantity of packages available until September, 2026.

```
Go      2,289,335  ████████████████████████████████████████
Python    933,811  ████████████████
Rust      337,789  ██████
C++         1,948  ▏
```

Compared to Python, Go is orders of magnitude faster for CPU-heavy workloads. Compared with C++, I rarely need to think about complicated build systems, package-management conventions, memory ownership, or subtle lifetime bugs. Compared with Rust, Go asks much less of me while I am experimenting. I do not have to constantly reason about ownership, borrowing, lifetimes, or which abstraction satisfies the borrow checker. 

My optimization target is **research productivity**.

## My requirements are slightly unusual

My PhD sits somewhere between computer architecture and software.

A lot of my code is not production software that will be maintained for ten years. Instead, I frequently write and modify small programs to test an idea, generate workloads, process experimental results, automate tools, or explore some strange idea simply because I am curious about it.

The code changes frequently. Some tools may only exist for one experiment. That changes what I want from a programming language. I do not want to spend a significant fraction of my research time solving software-engineering problems that are unrelated to my actual research question. I want to spend as little time as possible on infrastructure while still getting enough performance to run serious experiments. Unfortunately, interpreted languages can become frustrating when experiments contain large CPU-bound loops, which happens often enough in my work that performance matters.

## Why not Python?

Python initially sounds like the obvious choice. It has an enormous ecosystem, excellent libraries, and probably the lowest barrier to quickly turning an idea into working code. For many research tasks, Python is still difficult to beat.

But its runtime performance can become painful. If a program spends most of its time calling optimized NumPy, PyTorch, or native libraries, Python's overhead may not matter very much. But when the algorithm itself contains large amounts of custom CPU-heavy logic, Python can become the bottleneck. Sometimes, I need to run a Python script for hours to get the results I need. When the bottleneck is the language itself, this becomes a significant issue.

Then I face an unpleasant choice: accept the runtime cost (which is not acceptable in my case), rewrite the critical part in another language, or introduce bindings between Python and C/C++/Rust. At that point, the supposedly simple Python project starts becoming a multi-language software project. That is exactly what I am trying to avoid.

## Why not C++?

I already know C++, and its performance is obviously excellent.

The problem is everything surrounding the language. Dependency management, CMake, compiler configurations, platform differences, library discovery, build-system conventions, and integration problems can easily consume an unreasonable amount of time. Sometimes it feels as if the time spent on fighting with the build system is almost as much as the time spent on writing the actual code. For a large production system, that complexity may be justified. For a 2,000-line research tool that might be discarded next month, I would rather not deal with it.

## Why not Rust?

Rust solves many of the problems that frustrate me about C++. Cargo is excellent. Dependency management is excellent. The tooling is coherent. The language eliminates entire categories of memory-safety bugs while still producing high-performance native binaries. I like many of Rust's ideas.

But Rust also has a much steeper learning cost. Ownership, borrowing, lifetimes, traits, and the type system are powerful because they force the programmer to encode important correctness properties explicitly. That is an excellent trade-off when correctness, safety, and long-term maintainability dominate the engineering requirements. 

But power comes at a cost. One may spend a big portion of time just to implement a simple algorithm in Rust and wasting much time waiting for the compilation. If feels not smooth when developing some toy projects in Rust. During research prototyping, I often want to write something, modify it twenty minutes later, throw half of it away, and try something completely different. In that environment, Go's simplicity becomes a feature.

## Research productivity is the metric that matters

The fastest program is not necessarily the best research tool. For a researcher, the relevant quantity is closer to:

> **How much scientific novelty can I explore within a fixed amount of time?**

Suppose I have an experiment that requires long execution time where development time and execution time look roughly like this:

```
█ = Engineering time ░ = Waiting for execution
Python   20h eng + 80h wait = 100h   ████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
Go       30h eng + 10h wait =  40h   ████████████░░░░
Rust     55h eng +  8h wait =  63h   ██████████████████████░░░
```

These numbers are obviously illustrative rather than benchmarks. Python wins on initial implementation time but loses badly when the program is logically complex and needs to run repeatedly. Rust produces the fastest implementation, but I may spend far more time engineering it. Go does not win either category individually. It wins the **total**. And that is exactly why I like it.

## Go is boring, but that's where its power lies

Go does not try to give me every possible abstraction. Its type system is comparatively simple. Its garbage collector means I give up some control over memory management. It does not provide the same zero-cost abstraction philosophy as Rust or C++. But in return, I get:

* fast compilation;
* native-code performance;
* simple memory management;
* straightforward concurrency;
* Python-like scripting by `go run script.go`;
* and a language small enough that I rarely need to stop my research to study the language itself.

For production systems with hard real-time constraints, extreme performance requirements, kernels, embedded systems, or highly optimized libraries, I would probably choose something else. But that is not most of my PhD. For research software, the goal is rarely to spend the least time to implement a fast-enough executable. The key is to move from **idea → implementation → experiment → result** as quickly as possible.

For my work, Go is currently the best compromise I have found.
