+++
title = 'How to Debug C/C++ Code in VScode'
date = 2024-11-10T19:32:49+08:00
draft = false
tags = ["how to", "programming"]
+++

# Q&A

**Why is my debugger stuck at retrieving local variables?**

If you're on MacOS, take a breath. This is not your problem. There are known bugs with regrad to debugging C/C++ code using VSCode, refer to [this answer](https://stackoverflow.com/questions/76461370/vs-code-c-cmake-project-debugging-info-and-breakpoints-not-working). However, this answer's solution does not work. Here's the solution:

1. Install the **lldb-dap**. The **lldb-dap** binary is a command line tool that implements the [Debug Adapter Protocol](https://microsoft.github.io/debug-adapter-protocol/). It's a substitute for the **lldb-mi** debugger for VSCode.
2. Install a extension named **LLVM DAP** (extension ID: llvm-vs-code-extensions.lldb-dap). 
3. Tell the **LLVM DAP** extension the path to **lldb-dap**.
4. Change the `"type": "cppdbg"` to `"type": "lldb-dap"` in your `.vscode/launch.json` file.

Enjoy it.

**Why won't my breakpoints get hit?**

>Turning on optimization flags makes the compiler attempt to improve the performance and/or code size at the expense of compilation time and possibly the ability to debug the program.

* Check if you have compiled your project with `-g` option. 
* CMake: Check if `CMAKE_BUILD_TYPE` is set to `Debug`.
* Check if you compiled your project with `-O2` or higher-level optimizations. Higher optimization levels may conflict with the `-g` option.