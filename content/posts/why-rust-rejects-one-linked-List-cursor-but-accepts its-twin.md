---
title: "Why Rust Rejects One Linked-List Cursor but Accepts Its Twin"
date: 2026-10-09T19:19:18+08:00
draft: false
tags: ["programming language"]
---

# Why Rust Rejects One Linked-List Cursor but Accepts Its Twin

Two cursor loops over a singly linked list look equivalent, but only one compiles. The difference comes down to how many borrows each version creates.

## The data structure

```rust
#[derive(PartialEq, Eq, Clone, Debug)]
pub struct ListNode {
    pub val: i32,
    pub next: Option<Box<ListNode>>,
}

impl ListNode {
    #[inline]
    fn new(val: i32) -> Self {
        ListNode { next: None, val }
    }
}
```

Both versions remove every node with a given value. They walk the list with a cursor of type `&mut Option<Box<ListNode>>`.

## Rejected: E0506

```rust
let mut cur = &mut head;
loop {
    if let Some(node) = cur {
        if node.val == val {
            *cur = node.next.take();   // error: cannot assign to `*cur` because it is borrowed
        } else {
            cur = &mut node.next;
        }
    } else {
        break;
    }
}
```

## Accepted

```rust
let mut walker = &mut head;
loop {
    match walker {
        None => break,
        Some(node) if node.val == val => {
            *walker = node.next.take();
        }
        Some(node) => {
            walker = &mut node.next;
        }
    }
}
```

## Root cause

In the rejected version there is **one** `node` borrow of `*cur`, shared by both branches.

- The `else` branch stores that borrow back into `cur` (`cur = &mut node.next`).
- `cur` is used again in the next loop iteration, so the borrow must stay alive across the loop.
- Rust's borrow checker (NLL) reasons about lifetimes as sets of program points and isn't fully path-sensitive. It therefore treats the borrow as alive in the `then` branch too, where `*cur = ...` assigns to the borrowed place.

The branches never use `node` at the same time, so the code is sound. The compiler just can't prove it.

The accepted version has one binding, and so one borrow, **per match arm**. Here `Some(node) if node.val == val` is a *match guard*, an `if` condition after the pattern:

- In the guard arm, `node` is only used for `node.next.take()` and is dead before `*walker = ...` runs.
- In the last arm, the borrow flows into `walker`, and assigning to `walker` ends borrows that went through the old `*walker`.

Separate borrows mean no conflict.

There is no way to get around it with current stable release. A more precise analysis called Polonius is under development and it is nightly-only (`-Zpolonius=legacy`) might accept it.
