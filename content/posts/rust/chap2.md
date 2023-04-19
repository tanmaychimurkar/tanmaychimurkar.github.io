---
title: "Rust Documentation: Chapter 2"
date: 2023-04-18T10:14:11
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
in the [previous]({{< ref "chap1.md#cargo-" >}} "Rust Chapter 1") chapter, the `Cargo` package
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

`NOTE`: Traits are similar to interfaces in other languages. They define the methods that a type
must implement. We will learn more about traits in a later chapter.

### Breakdown of the above code

The `main` function is the entry point of the program. 

The `println!` macro is used to print the string to the standard output. 
The `println!` macro is similar to the `printf` function in C or the `print`
function in Python. The `println!` macro is a macro because it is prefixed with an exclamation mark. 
We will learn more about macros in a later chapter.

The `let` keyword is used to create a new variable. 

The `mut` keyword is used to make the variable mutable. 

The `String::new` function is used to create a new empty string. The `String` type is a
string type provided by the standard library. The `String` type is a growable, UTF-8 encoded string.

However, we still need a way to read the input from the user. The `read_line` method takes the input
from the user and stores it in the `guess` variable. The `read_line` method takes the input as a
mutable reference. 

The `&` symbol is used to create a reference. The `&mut` symbol is used to create
a mutable reference. 

The `read_line` method returns a `Result` type. The `Result` type is an `enum`
which has two variants: `Ok` and `Err`. The `Ok` variant indicates that the operation was successful.
The `Err` variant indicates that the operation failed. The `expect` method is used to handle the
`Err` variant. The `expect` method takes a string as an argument. If the `Result` type is `Ok`, the
`expect` method returns the value inside the `Ok` variant. If the `Result` type is `Err`, the `expect`
method prints the string passed to it and exits the program. If we don't call the `expect` method,
the program will compile but will throw a warning.

```rust
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
warning: unused `Result` that must be used
  --> src/main.rs:10:5
   |
10 |     io::stdin().read_line(&mut guess);
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: `#[warn(unused_must_use)]` on by default
   = note: this `Result` may be an `Err` variant, which should be handled

warning: `guessing_game` (bin "guessing_game") generated 1 warning
    Finished dev [unoptimized + debuginfo] target(s) in 0.59s
```

The `Rust` compiler is smart enough to detect that the `Result` type returned by the `read_line`
method is not being used. The `Rust` compiler throws a warning to let us know that we are not
handling the `Err` variant of the `Result` type.

Phew! That was a lot of information. Take a look at the code again and make sure we can understand
the code with the new information that we have learned.

#### Building and Running the code

We can run the code using the following command:

```bash
cargo run
```

*`Key takeaway`*: `cargo run` will build and run the code, while `cargo build` will only build an executable 
in the `target/debug` directory. `cargo run` is useful when we are developing the code. `cargo run`
will compile the code and run the executable every time we make a change to the code. `cargo build`
is useful when we are deploying the code. `cargo build` will compile the code and create an executable.

Once run, the code will print the following output:

```bash

$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.59s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
5
You guessed: 5
```

We can see that we can now take an input from the user. However, we still need to generate a random
number with which the comparison for the user input has to be done. This random number generator functionality
can be implemented smoothly by using `cargo`.

#### Generating a random number

With `cargo`, we have the option to get `crates`. `Crates` are packages of Rust code that we can
use in our project. We can get a `crate` by adding the `crate` name and the version number to the
`Cargo.toml` file. The `Cargo` package manager will download the `crate` and add it to the `Cargo.lock`


For our case, we need to add the `rand` crate to the `Cargo.toml` file. The `rand` crate is a
crate that provides random number generation functionality. 
To add the `rand` crate to the `Cargo.toml` file, we need to add the following line to the `Cargo.toml` file:

```toml
[dependencies]
rand = "0.8.5"
```
The documentation of the `rand` crate can be found 
[here](https://docs.rs/rand/0.7.3/rand/ "rand crate documentation").

If you build the project now with `cargo build`, we should see that the `rand` create is being fetched
and added to the `Cargo.lock` file.

```bash

$ cargo build
    Updating crates.io index
 Downloading crates ...
  Downloaded rand v0.8.5
  Downloaded 1 crate (1.1 MB) in 0.75s
   Compiling rand v0.8.5
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.88s
```

However, if you run it again, we will not see the above output. `cargo` will check the `Cargo.lock` file
to see if the dependencies have changed. If the dependencies have not changed, `cargo` will not fetch
the dependencies again. This is useful when we are deploying the code, since we can be sure that the
dependencies will not change when we deploy the code, and we will deploy the code with the same 
dependencies that we were using while developing the code.

