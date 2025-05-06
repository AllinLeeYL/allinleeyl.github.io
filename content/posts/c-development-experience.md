---
title: 'C/C++ Development Experience'
date: 2024-10-20T18:12:15+08:00
draft: false
tags: ["programming"]
---

This blog collects commonly seen C/C++ errors that occur during the compilation/linking process and gives some advice on them.

## undefined reference to `xxx`

Your compiler will complain about this when the symbol `xxx` cannot be found among the linked libraries, especially when you are compiling a project using multiple components from different developers. Usually, you can solve this problem by:

* Checking if you forget to link some .c/.cpp files to your final target, if the symbol is defined in your own code.
* Checking if you forget to add the `-llibxxx` argument if you are using some functions provided by third-party libraries. For example, `-lm` should be given when using mathematics functions provided by `libm.a`.
* Checking if there is a mismatch of versions between your header file (.h/.hpp) and the libraries (.o/.a/.dylib/.dll). Most developers will discard support for some features of older versions after some version. This will lead to some symbols in the header file not being defined in an older/newer version of libraries. You are suggested to check out the changelog of the libraries you're using.
