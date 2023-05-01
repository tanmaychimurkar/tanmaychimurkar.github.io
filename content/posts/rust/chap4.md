---
title: "Rust Documentation: Chapter 4"
date: 2023-04-29T11:23:17
description: "This post gives a breakdown of the key takeaways from Chapter 4 of the Rust Documentation"
tags: ["Rust", "bare metal", "embedded"]
categories: ["Rust"]
author: "Tanmay"
showToc: true
TocOpen: true
cover:
    image: "https://rustacean.net/assets/rustacean-orig-noshadow.png"
    alt: Rust Crab
    caption: "The Rust crab, Ferris, with its arms folded"
ShowBreadCrumbs: true
---

In the [previous chapter]({{< ref "chap3.md" >}} "Rust Chapter 3"), we saw how we can use data types, functions,
and control flow in `rust`. In this chapter, we will look at the most unique feature of `rust` - ownership.

## Ownership

Ownership enables Rust to make memory safety guarantees without needing a garbage collector, so it’s important 
to understand how ownership works. In this chapter, we’ll talk about ownership as well as several related 
features: borrowing, slices, and how Rust lays data out in memory.

### Stack and Heap

In `stack`, it is easier to store objects that have a known size at compile time. The stack stores values in the
`LIFO` order. In `heap`, it is easier to store objects that have an unknown size at compile time or a size that
might change. The heap stores values in any order, wherein the compiler looks for a place to store the data.

Pushing to the stack is faster than allocating on the heap because the allocator never has to search for a 
place to store new data; that location is always at the top of the stack. Comparatively, allocating space 
on the heap requires more work because the allocator must first find a big enough space to hold the data 
and then perform bookkeeping to prepare for the next allocation.

When we call a function, all the parameters of the function are pushed onto the `stack`. When the function is
over, the parameters are popped off the stack. This is why the size of all the parameters of a function must
be known at compile time. Usually, all vairables that have a known data type, i.e., `i32`, `u32`, `f32`, etc.
are stored on the stack.

In `heap`, we usually keep a `pointer` to the location in memory where the data is stored. 

## Ownership Rules

In `rust`, there are a few rules that the compiler checks to ensure memory safety. These are:

- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

### Memory and Allocation

In `rust`, when we take a `String` type, we might not always know how much memory we need to allocate for it, since
it is possible to later change the value that the `String` type holds. We can see this in an example below:

```rust
    let mut s = String::from("hello");

    s.push_str(", world!"); // push_str() appends a literal to a String

    println!("{}", s); // This will print `hello, world!`
```

We can see that we can add string literals to `rust` strings. However, this type of operation is not possible
for string literals. This main difference comes because of the way `rust` handles memory and allocation for 
the `String` type.

With the String type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents. This means:

- The memory must be requested from the memory allocator at runtime.
- We need a way of returning this memory to the allocator when we’re done with our String.

The `String::from` function does all the memory allocation for us. However, once we have requested memory, we need to
keep track of when that memory needs to be freed up. 

In `rust`, the memory is automatically returned once the variable that owns it goes out of scope. This can be 
seen in the example below:

```rust
    {
        let s = String::from("hello"); // s is valid from this point forward

        // do stuff with s
    }                                  // this scope is now over, and s is no longer valid
```

When a variable goes out of scope, the `drop` function is called that frees up the memory. 

### Variable and Data Interaction with `Move`

In `rust`, we can assign a `String` to a different variable using let as follows:

```rust
    let s1 = String::from("hello");
    let s2 = s1;
```

When we do this, we expect `s2` to also contain the contents of the `String` that `s1` contains. However, this is not
the case. 

A `String` is made up of three parts: a pointer to the memory that holds the contents of the string, a length, 
and a capacity. This group of data is stored on the stack. The memory that the pointer refers to is stored on the heap. 

This can be seen in the below image:

![Memory Allocation](https://doc.rust-lang.org/book/img/trpl04-01.svg)

When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the pointer, the length, and 
the capacity that are on the stack. We do not copy the data on the heap that the pointer refers to.

In short, this is what happens

![Making a copy of the data](https://doc.rust-lang.org/book/img/trpl04-02.svg)

We can see from the above image that there is not a new `copy` of the data that is created when we assign
`s1` to `s2`. In short, `rust` does not create a deep copy when we assign one variable to another.
