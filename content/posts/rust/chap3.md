---
title: "Rust Documentation: Chapter 3"
date: 2023-04-26T15:34:18
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

Even if the compiler does not like what we did, it does not hate us!(unlike the `C` compiler, wha whaaaat!!!!) We can see that the issue is 
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

### Constants

In rust, constants are always immutable by default. We define constants using the `const` keyword, 
instead of the `let` keyword. Trying to make a constant defined with `const` mutable, as seen in below
code, would result in the following error:

```rust
fn main() {
    const mut MAX_POINTS: u32 = 100_000;
    MAX_POINTS = 100_001;
}
```

```bash
error: const globals cannot be mutable
 --> src/main.rs:4:11
  |
4 |     const mut MAX_POINTS: u32 = 100_000;
  |     ----- ^^^ cannot be mutable
  |     |
  |     help: you might want to declare a static instead: `static`
  ```

So, in conclusion, we cannot make constants mutable in Rust.

### Shadowing

We can shadow a variable by using the same variable name as the previous variable. This is different from
mutability. Let's see an example:

```rust

fn main() {
    let x = 5;
    let x = x + 1;
    let x = x * 2;
    println!("The value of x is {x}");
}
```

Also, shadowing is different that using `mut` variables, since for mut variables, we can ignore the 
`let` keyword, but for shadowing, we have to use the `let` keyword. Also, if we plan to change the 
datatype of the variable we are shadowing, we need to explicitly mention the new datatype.


*Key Points*

- Variables are immutable by default in Rust
- If we want immutable variables, we use the `mut` keyword after the `let` keyword
- By strict nature, `const` variables, i.e., constants, are immutable in Rust
- Shadowing is different that using a `mut` variable, since if we shadow a variable again, we need to use the
  -`let` keyword again.
- If we change the datatype of the variable we are shadowing, we need to explicitly mention the new datatype.


##  Data Types

In rust, every value has a `data type`. Data types are the typical data types that we see in other programming
languages, and they are as follows: 

- Scalar Types
  - Integers
  - Floating-Point Numbers
  - Booleans
  - Characters

### Integer Types

Integer types as signed and unsigned, i.e., can range from negative to positive values, and only positive values, 
respectively. They are represented as either `i32` for signed integers, or `u32` for unsigned integers.
Full reference can be found [here](https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-types`).

### Floating-Point Types

There are only two floating point types in Rust, `f32` and `f64`. The default type is `f64`, since it is faster
than `f32` on modern CPUs.

`Important Note`: When performing mathematical operations, we need to be careful about the data types we are using.
By default, mathematical operations in `rust` do not convert floating point numbers to return floats. For example,
an operation `2/3` would return `0`, since both `2` and `3` are integers. To get the correct result, we need to
explicitly convert one of the numbers to a float, as follows: `2.0/3` or `2/3.0` or `2.0/3.0`.

### Boolean Type

Boolean types in Rust are represented as `bool`, and can have two values, `true` or `false`.

### Character Type

Most primitive alphabetic type. Characters are always specified using single quotes, as follows: `let c = 'z'`.

## Compound Types

These data types are used to combine multiple values into a one type. There are only two compound types in Rust:
`tuples` and `arrays`.

### Tuples

Can have multiple data types clubbed into one tuple. Type annotation is optional when creating a tuple, but useful.

To get the values out from a tuple in `rust`, we can either unzip the tuple, or use `.` operator with the index. 

### Arrays

Arrays must have all elements from the same data type. Arrays in `rust` have a fixed size, so we cannot append
to arrays, instead we append to `vectors`, which we will see in a later chapter. 

Also, arrays are good when we want to store the data on `stack` rather than `heap`, since they always have
a fixed size.

In `rust`, arrays should be defined as follows:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Elements from array in `rust` are accessed as they are in `Python`, via the `[index]` operator.
If we try to access an invalid index, the compiler `panics` and we usually get a helpful error message.

## Functions

In `rust`, functions are defined with `fn` keyword, followed by the name of the function, and then the
parameters. The return type of the function is specified after the `->` operator. If the function does
not return anything, we can omit the return type. 

In `rust`, the function signatures `must` have a data type. This is because the compiler needs to know
the size of the return type at compile time. This can be seen in this example:

```rust
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

### Expressions and Statements

In `rust`, expressions and statements are different.

- Statements are instructions that perform some action and do not return a value.
- Expressions evaluate to a resultant value.

`let y = 6;` is a statement, as it is only assigning a value to the variable `y`.

```rust
{
    let x = 3;
    x + 1
} // This is an expression, since it returns the value 'x + 1'
```

We can return early from a function using the `return` keyword in `rust`. However, even without the `return`
keyword, the last expression in a function is returned implicitly. If we have a return type, its type is to 
given in the function signature as follows:

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}
```

## Control Flow

Most common constructs are the `if`, `while`, and `for` loops. They are all expressions, as they typically
return a value at the end of their execution.

### If Expressions

Only requirement is that the condition that is being checked must be a `bool` type. So, we cannot have a conditional
block like this in `rust`:

```rust
fn main() {
    let number = 3;

    if number {
        println!("number was three");
    }
}
```

The above code block would raise an `error` in `rust`, as the condition `if number` is not a `bool` type.

### Using `if` in `let` statements

In rust, we can use `if` in `let` statements as follows:

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

This is a valid assignment (statement) in rust, as we assign a value based on a condition. However, we need to
make sure that the return value of the conditional is of the same data type, otherwise the compiler will throw
an error. For example, this is not a valid expression using `if` blocks:

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { "six" };

    println!("The value of number is: {number}");
}
```

The above would throw an error, as one type is of integer and another is of string type.

### Looping your code

In rust, we can make loops using the `loop`, `while`, and `for` keywords.

#### `loop` keyword

Executes forever, until user stops the program manually. We can also use `break` keyword to break out of the loop.

We can also assign `loop labels` in rust to know which loop is being executed. When breaking out of a loop,
we can then use the `break` keyword with the `loop label` to stop execution. It is mandatory to have the 
loop labels starting with the `'` character to be able to use them in `break` statements.

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

#### `while` keyword

The `while` keyword works just like the `if` keyword, wherein the code is executed until it reaches a `false` condition.

#### `for` keyword

The `for` keyword is used to iterate over a collection of items. For example, we can iterate over a range of numbers.

`Important Note`: In `for` conditions, the range over which we an element to loop over is specified with the syntax
`(start..end)`, where `start` is inclusive and `end` is exclusive. For example, `for n in (1..4)` will iterate over
the numbers `1, 2, 3`.

## Conclusion

In this chapter, we learned about the basic data types in `rust`, and how to use them. We also learned about the
`if` expressions, and how to use them in `let` statements. We also learned about the `loop`, `while`, and `for`
keywords, and how to use them to iterate over a collection of items.