---
title: "Rust Documentation: Chapter 2"
date: 2023-04-17T11:41:15
description: "This post gives a breakdown of the key takeaways from Chapter 2 of the Rust Documentation"
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

## Programming a Guessing Game 🎲

In the [previous]({{< ref "chap1.md" >}} "Rust Chapter 1"), we learned about the basics of Rust.
In this chapter, we will learn about the basics of Rust by building a simple guessing game.

### Setting up the project

We will use the `Cargo` package manager to create a new project called `guessing_game`.
We can create a new project using the following command:

```bash
cargo new guessing_game
```

This creates a new project called `guessing_game` in the current directory.
The `Cargo` package manager creates a new directory with the following structure:

```bash
guessing_game
├── Cargo.toml
└── src
    └── main.rs
```

The `Cargo.toml` file contains the metadata of the project. The `src` directory contains the source 
code of the project. The `main.rs` file contains the main function of the project. As we saw 
in the [previous]({{< ref "chap1.md" >}} "Rust Chapter 1") chapter, the `Cargo` package
manager uses the `main.rs` file as the entry point of the project. The `Cargo` package manager uses
the `Cargo.toml` file to manage the dependencies of the project.

### Writing the code

Let's write the code for the guessing game. We will start by adding the following code to the
`main.rs` file:

```rust
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

The `use` keyword is used to import the `io` module from the standard library. The `io` module
contains the `stdin` function which is used to read the input from the user. The `stdin` function
returns an instance of the `Stdin` type. The `Stdin` type implements the `Read` trait. The `Read`
trait defines the `read_line` method which is used to read a line from the `Stdin` type.
