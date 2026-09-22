---
date: '2026-09-22T21:29:17+02:00'
draft: false
title: 'Modern Cli Tools'
---

In this post, I will introduce some of my favorite modern command-line tools. These tools have substantially improved my quality of life when working in the terminal and made the command-line experience much more pleasant.

## [`mise`](https://mise.jdx.dev/) — an alternative to `Makefile` and Bash scripts

The name `mise` comes from "*mise en place*", a French phrase that roughly means "everything in its place." I think `mise` is surprisingly revolutionary while also making you wonder why something like it did not exist earlier. So far, the feature I use most frequently is its **task runner**. For example, given a `mise.toml` file like this:

```toml
[tasks.build]
run = "cd orchestrator && go build -o ../orchestrate ./cmd/orchestrate"

[tasks.test]
depends = ["build"]
run = "./orchestrate test"
```

I can simply run:

```bash
mise run test
```

instead of repeatedly typing the full command and all its arguments. You may also notice that tasks can be chained together.

This is especially useful for projects where I frequently run the same build, test, simulation, or experiment commands. Instead of maintaining a collection of Bash scripts or remembering long command lines, I can define everything in `mise.toml`. 

Though this can also be done by Makefile, but you have to define awkawrd `.PHONY` targets and it works, I will say, in a wired way. I have also considered other tools, such as [task](https://taskfile.dev/) and [just](https://github.com/casey/just), but I found that none of them is as powerful as `mise`.

What I have described here is only a small part of what `mise` can do. Among other things, it can serve as:

1. A task runner;
2. A tool/version manager;
3. An environment-variable manager;
4. A unified way to install and manage development tools.

For me, the task-running feature alone already makes it worth using.

## [`ripgrep`](https://github.com/BurntSushi/ripgrep) — an alternative to `grep`

Imagine that you want to search for the string `orchestrator` recursively in the current directory.

With `grep`, you might write:

```bash
grep --recursive "orchestrator" .
```

With `ripgrep`, you can simply write:

```bash
rg orchestrator
```

That is already more convenient, but `ripgrep` also provides cleaner, better-organized, and colorized output.

It also ignores hidden files, binary files, and files excluded by `.gitignore` by default, which is usually exactly what I want when searching through a source-code repository. Once I started using `rg`, I rarely went back to `grep` for interactive use.

## [`bat`](https://github.com/sharkdp/bat) — an alternative to `cat`

`bat` is, quite simply, a nicer `cat`.

For example, `bat main.rs`, displays the file with syntax highlighting, line numbers, and a more readable layout. For quickly inspecting source files in the terminal, I find it much more pleasant than plain `cat`.

## [`zellij`](https://zellij.dev/) — an alternative to `tmux`

The biggest advantage of `zellij` over `tmux`, at least for me, is that it does not immediately force you to memorize a large collection of keyboard shortcuts. And most importantly, **scrolling works without extra settings**!

`zellij` provides an on-screen interface showing the available modes and key bindings, so you can usually figure out what to press without consulting documentation. This makes it much friendlier to new or occasional users. `tmux` is extremely powerful, especially once you have customized it and memorized its shortcuts, but `zellij` gives you a much nicer experience out of the box. For someone who just wants to SSH into a remote machine, create a few panes, run long-running jobs, detach, and come back later, `zellij` feels much more approachable.

