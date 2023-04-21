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

In the [previous chapter]({{< ref "chap1.md" >}} "Rust Chapter 1"), we learned about the basics of Rust.
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

### Guessing game code

The full code for the guessing game, which is in the `main.rs` file, looks as follows:

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=101);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: i32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("This is not expected. Please enter an integer");
                continue;
            },
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win! You are the guessing game champion!");
                break;
            },
        }
    }
}
```

Much of the code looks like mumbo jumbo for now, but we will break it down in pieces.


### Breakdown of the above code

The `use` keyword is used to import the `io` module from the standard library. The `io` module
contains the `stdin` function which is used to read the input from the user. The `stdin` function
returns an instance of the `Stdin` type. The `Stdin` type implements the `Read` trait. The `Read`
trait defines the `read_line` method which is used to read a line from the `Stdin` type.

`NOTE`: Traits are similar to interfaces in other languages. They define the methods that a type
must implement. We will learn more about traits in a later chapter.

*Points to remember:*

- The `main` function is the entry point of the program. 

- The `println!` macro is used to print the string to the standard output. 
The `println!` macro is similar to the `printf` function in C or the `print`
function in Python. The `println!` macro is a macro because it is prefixed with an exclamation mark. 
We will learn more about macros in a later chapter.

- The `let` keyword is used to create a new variable. 

- The `mut` keyword is used to make the variable mutable. 

- The `String::new` function is used to create a new empty string. The `String` type is a
string type provided by the standard library. The `String` type is a growable, UTF-8 encoded string.

- However, we still need a way to read the input from the user. The `read_line` method takes the input
from the user and stores it in the `guess` variable. The `read_line` method takes the input as a
mutable reference. 

- The `&` symbol is used to create a reference. The `&mut` symbol is used to create
a mutable reference. 

- The `read_line` method returns a `Result` type. The `Result` type is an `enum`
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

*`Key takeaways`*: 
- `cargo run` will build and run the code, while `cargo build` will only build an executable 
in the `target/debug` directory. 
- `cargo run` is useful when we are developing the code. `cargo run`
will compile the code and run the executable every time we make a change to the code. 
- `cargo build` is useful when we are deploying the code. `cargo build` will compile the code and 
create an executable.

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

#### Generating Random Number for our game

Now that we have the `rand` crate, we can use it to generate a random number. We can use the `gen_range`
method of the `rand` crate to generate a random number. The `gen_range` method takes two arguments:
the lower bound and the upper bound. The `gen_range` method will generate a random number between
the lower bound and the upper bound.

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

If you run the code now, you will see that the code will generate a random number between 1 and 101, and
will print it to the console. 

```bash

$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 42
Please input your guess.
5
You guessed: 5
```

Now we see that a random number is being generated. However, we still need to compare the user input
with the random number. We will do this in the next section.

#### Comparing the user input with the random number

We can compare the user input with the random number using the `cmp` method. 

*Points to remeber*:

1) The `cmp` method takes a reference to the value that we want to compare with. 
2) The `cmp` method returns an `Ordering` type. 
3) The `Ordering` type is an `enum` that can have three values: `Less`, `Greater`, and `Equal`. The
`cmp` method compares the value that we are calling the method on with the value that we pass as an
argument to the `cmp` method. 
4) If the value that we are calling the method on is less than the value
that we pass as an argument to the `cmp` method, the `cmp` method will return the `Less` variant of
the `Ordering` type. If the value that we are calling the method on is greater than the value that
we pass as an argument to the `cmp` method, the `cmp` method will return the `Greater` variant of
the `Ordering` type. If the value that we are calling the method on is equal to the value that we
pass as an argument to the `cmp` method, the `cmp` method will return the `Equal` variant of the
`Ordering` type. 
5) We can use the `match` expression to handle the `Ordering` type returned by the `cmp` method.
6) The `match` expression is similar to the `switch` statement in other languages. 
7) The `match` expression takes a value as an argument and compares the value with the patterns
that we specify in the `match` expression. If the value matches the pattern, the code that is 
associated with the pattern will be executed. If the value does not match any of the 
patterns, the `match` expression will throw an error.

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win! You are the guessing game champion!"),
    }
}
```

If you run the code now, you will see that the code will not compile. 

*Points to remember*:
The reason for this is that we are trying to compare a `String` type with an `i32` type. We can 
fix this by converting the `String` type to an `i32` type. We can do this by using the `trim` method to remove the newline character
from the `String` type, and then using the `parse` method to convert the `String` type to an `i32`
type.

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    let guess: i32 = guess.trim().parse().expect("Please type a number!");

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win! You are the guessing game champion!"),
    }
}
```

If you run the code now, you will see that the code `will` compile and run. 

However, we can still type an alphabet and get away with it. We can fix the input to be only numbers
by using the `Result` type returned by the `parse` method. The `Result` is of type `enum` and can have
two values: `Ok` and `Err`. 

*Points to remember*:

1) The `Ok` variant of the `Result` type means that the operation was successful 
2) The `Err` variant of the `Result` type means that the operation failed. 
3) The `parse` method will return the `Ok` variant of the `Result` type if the
conversion was successful, and will return the `Err` variant of the `Result` type if the conversion
failed.
4) We can use the `match` expression to handle the `Result` type returned by the `parse` method.
If the `parse` method returns the `Ok` variant of the `Result` type, we will assign the value that
is inside the `Ok` variant to the `guess` variable. If the `parse` method returns the `Err` variant
of the `Result` type, we will print the error message that is inside the `Err` variant to the console.

With these points, we can change the code as follows:

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    let guess: i32 = match guess.trim().parse() {
        Ok(num) => num,
        Err(_) => {
            println!("This is not expected. Please enter an integer");
            continue;
        },
    };

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win! You are the guessing game champion!"),
    }
}
```

If you run the code now, you will see that the code will compile and run. However, if you enter a
non-numeric value, the code will not panic. Instead, the code will print the error message that we
specified in the `Err` variant of the `Result` type. We handled the panic using the `Err` variant of
the `Result` type.

#### Looping until correct number is guessed

We can use `loop` to keep the program running until the correct number is guessed. This can be done as
follows:

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=101);

    println!("The secret number is: {}", secret_number);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: i32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("This is not expected. Please enter an integer");
                continue;
            },
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win! You are the guessing game champion!");
                break;
            },
        }
    }
}
```

*Points to remember*:

We just use `loop` keyword to create an infinite loop. We can use `break` keyword to break out of
the loop. We can also use `continue` keyword to skip the rest of the loop and start the next
iteration of the loop. 

#### Removing the secret number message

We just have to remove the secret number generation part from the code. Once we remove the secret
number generation part from the code, we will not be able to print the secret number to the console.

The final script should then look like this:

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=101);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: i32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("This is not expected. Please enter an integer");
                continue;
            },
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win! You are the guessing game champion!");
                break;
            },
        }
    }
}
```
