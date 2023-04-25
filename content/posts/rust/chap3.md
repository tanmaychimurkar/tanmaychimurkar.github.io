---
title: "Rust Documentation: Chapter 3"
date: 2023-04-20T15:34:18
description: "This post gives a breakdown of the key takeaways from Chapter 3 of the Rust Documentation"
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

In the [previous chapter]({{< ref "chap2.md" >}} "Rust Chapter 2"), we built a simple guessing game.
Now, we will learn more about variables, data types, mutability, and control flow in Rust

##  Variables and Mutability

In Rust, we define variables with `let` keyword. However, variables by default are not mutable in Rust.
This means that once a variable is defined, it cannot be changed. To make a variable mutable, we use the
`mut` keyword after the `let` keyword. Below is an example of mutable and immutable variables.

```rust 
fn main() {
    let x = 5; // immutable variable
    let mut y = 6; // mutable variable
```
If we try to reassign value to the variable `x` above, the `Rust` compiler will `NOT` like it! Let's try.

```rust
fn main() {
    let x = 5; //immutable variable
    x = 6;
}
```

Try running the above code, and we should get the following:

```bash
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         -
  |         |
  |         first assignment to `x`
  |         help: consider making this binding mutable: `mut x`
3 |     println!("The value of x is {x}");
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable
```

Even if the compiler does not like what we did, it does not hate us! We can see that the issue is 
very nicely printed out for us.

Similarly, we can reassign any other value to the variable `y` from the above example as follows:

```rust
fn main() {
    let mut y = 6;
    y = 7
    println!("The value of y is {y}")
}
```

If you build and run the above code, you should see that the value of `y` is `7`.
