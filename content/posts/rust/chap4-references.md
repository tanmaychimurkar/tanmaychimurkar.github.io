---
title: "Rust Documentation: Chapter 4 - References"
date: 2023-05-01T08:23:15
description: "This post gives a breakdown of the key takeaways from Chapter 4 - References of the Rust Documentation"
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

In the [previous chapter]({{< ref "chap4.md" >}} "Rust Chapter 4"), we saw how ownership works in Rust.
The main drawback we saw was that for functions, the return value changes ownership and the original variable is 
no longer usable. In this chapter, we will look at a continuation of ownership, i.e. references.

## References

Instead of passing variables to functions, we can pass `references` to functions. `reference` is like a pointer
we can follow to get to the data. Unlike a pointer, a `reference` is guaranteed to point to valid data for the
life of that reference.

In `rust`, references can be used as follows:

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len); // here s is valid as we only pass its reference
}

fn calculate_length(s: &String) -> usize { // we need to pass the type of the reference
    s.len()
}
```

In the above function, passing the ampersand `String` instead of `String` is called `referencing` and it allows us to
refer to a value without taking ownership of it. Since the value `s` in the function is only referenced and not owned,
it will not be dropped when the function ends, and would thus still be valid.

This action of creating a reference and passing it to a function that accepts a reference is called `borrowing`, since
we are only borrowing a value and not taking `ownership` of it! Neat!!!!

### Modifying borrowed values

What were to happen if we try to modify a value we have borrowed? For example, in the function below, we try to
modify the `reference` that is passed to the function:

```rust
fn main() {
    let s = String::from("hello");
    
    update_string(&s);
    
    
    fn update_string(s: &String) {
        s.push_str(", world!");
    }
}
```

If we try to run the above code, we will get the following error:

```bash
$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0596]: cannot borrow `*some_string` as mutable, as it is behind a `&` reference
 --> src/main.rs:8:5
  |
7 | fn change(some_string: &String) {
  |                        ------- help: consider changing this to be a mutable reference: `&mut String`
8 |     some_string.push_str(", world");
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `some_string` is a `&` reference, so the data it refers to cannot be borrowed as mutable

For more information about this error, try `rustc --explain E0596`.
error: could not compile `ownership` due to previous error
```

The error is pretty self-explanatory. We cannot modify a value that we have borrowed. This is because when we pass a
reference to a function, we are only allowed to read the value, not modify it. This is called `immutable` borrowing.
This is also the same behaviour that we have seen for variables that are `immutable` by default in [Chapter 3]({{< ref "chap3.md#variables-and-mutability" >}} "Rust Chapter 3").

### Mutable References

Just as we had mutable variables, we can also have mutable references. We can create a mutable reference by using
`&mut` instead of `&`:

```rust
fn main() {
    let mut s = String::from("hello");
    
    update_string(&mut s);
    
    fn update_string(s: &mut String) {
        s.push_str(", world!");
    }
}
```

In the above implementation, we pass the `&mut s` to the function as reference instead of `&s`. 
Next, we also pass the `&mut String` to the function instead of `&String`, implying that we are passing a mutable
reference to the function.

`Major restriction`: We can only have one mutable reference to a particular piece of data in a particular scope.
In short, there can only be a single `mutable reference` to a particular variable that we can use.

For example, the following code will not compile:

```rust
fn main() {
    let mut s = String::from("hello");
    
    let r1 = &mut s;
    let r2 = &mut s;
    
    println!("{}, {}", r1, r2);
}
```

If we run the above code, it will raise the following error:

```bash
$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0499]: cannot borrow `s` as mutable more than once at a time
 --> src/main.rs:5:14
  |
4 |     let r1 = &mut s;
  |              ------ first mutable borrow occurs here
5 |     let r2 = &mut s;
  |              ^^^^^^ second mutable borrow occurs here
6 |
7 |     println!("{}, {}", r1, r2);
  |                        -- first borrow later used here

For more information about this error, try `rustc --explain E0499`.
error: could not compile `ownership` due to previous error
```

Since we are trying to create two references `r1` and `r2` to the same variable `s`, the compiler will throw an error.
The error is because we are borrowing the same variable a second time before the scope of the first variable
is not finished. 

If we were to change the above code to the following, it will run without this error:

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &mut s;
    println!("The value of r1 is {r1}");

    let r2 = &mut s;
    println!("The value of r1 is {r2}");
}
```

In this case, we are first ending the scope of `r1` when we call print, and we are then creating a second
reference `r2` to the variable `s`. This is allowed by the compiler.

`Resticting multiple mutable references`: The main advantage for restricting multiple mutable references 
is that the compiler does not have data races at compile time. A data race is similar to a race condition and happens when these three behaviors occur:

- Two or more pointers access the same data at the same time.
- At least one of the pointers is being used to write to the data.
- There’s no mechanism being used to synchronize access to the data.

We can also use `{}` to scope a mutable reference such that there is no conflict between two mutable references:

```rust
fn main() {
    let mut s = String::from("Hello");

    {
        let r1 = &mut s;
        println!("The value of r1 is {}", r1);
    } // scope of mutable reference is finished, and a new variable with mutable reference can be created
    
    let r2 = &mut s;
    println!("The value of r2 is {}", r2);
}
```

The above code will also compile, since the scope of the first mutable reference `r1` is finished before we create
the second mutable reference `r2` by using `{}`.

### Scope matters for mutable and immutable references!

We can have multiple `immutable` references from a variable, since by nature, immutable references cannot 
change any value, and can be looked at as a `read-only` reference. However, we cannot put an `mutable` reference
before the `scope` of the `immutable` reference is finished, since the `mutable` reference can change the value
of the variable, and thus, the `immutable` reference will not be valid anymore.

This can be seen clearly from the below example:

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // no problem, since immutable reference used
    let r2 = &s; // no problem, since immutable reference used
    let r3 = &mut s; // BIG PROBLEM, mutable reference used before immutable references are finished

    println!("{}, {}, and {}", r1, r2, r3); // will not compile
}
```

Running the above code will raise the following error:

```bash
$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
 --> src/main.rs:6:14
  |
4 |     let r1 = &s; // no problem
  |              -- immutable borrow occurs here
5 |     let r2 = &s; // no problem
6 |     let r3 = &mut s; // BIG PROBLEM
  |              ^^^^^^ mutable borrow occurs here
7 |
8 |     println!("{}, {}, and {}", r1, r2, r3);
  |                                -- immutable borrow later used here

For more information about this error, try `rustc --explain E0502`.
error: could not compile `ownership` due to previous error
```

As we can see, the compiler will not allow us to create a mutable reference `r3` before the immutable references
`r1` and `r2` are finished. This is because the mutable reference `r3` can change the value of the variable `s`,
and thus, the immutable references `r1` and `r2` will not be valid anymore.

However, the below code is valid and will compile:

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // no problem, read-only
    let r2 = &s; // no problem, read-only
    println!("{} and {}", r1, r2);
    // variables r1 and r2 will not be used after this point

    let r3 = &mut s; // no problem even if modified
    println!("{}", r3);
}
```

### Dangling references

For languages with pointers, it is possible that a reference can point to a location that is given to another
variable, or free some other variables memory by mistake. In `rust`, the compiler never lets us create `dangling
references` to a variable. If we have a `reference` to some data, the compiler will make sure that the data
does not go out of scope before the reference is out of scope. 

Let's look at the below example to see how `rust` prevents us creating a dangling reference.

```rust
fn main() {
    let some_reference = dangle(); // is a reference to a return value of a function
    
    fn dangle() -> &String {
        let s = String::from("hello"); // s comes into scope at this line
        
        &s // create a reference to s and return it
    } // the function finishes and s goes out of scope after this line, so the reference has nothing to
      // point to
}
```

Running the above, we get the following error:

```bash
error[E0106]: missing lifetime specifier
 --> src/main.rs:6:20
  |
6 |     fn dangle() -> &String {
  |                    ^ expected named lifetime parameter
  |
  = help: this function's return type contains a borrowed value, but there is no value for it to be borrowed from
help: consider using the `'static` lifetime
  |
6 |     fn dangle() -> &'static String {
  |                     +++++++
```

The error says that `this functions's return type contains a borrowed value, but there is no value for it to 
be borrowed from`. This is because the variable `s` goes out of scope after the function `dangle` finishes, and
thus, the reference `&s` has nothing to point to.

Instead of returning a reference in the above function, we need to return `s` directly, and take ownership of
the variable `s` in the function `dangle`:

We can see how a plan comes together in `rust` with ownership and scope. 

In the [next](chap4-slices.md) chapter, we will look at a different kind of reference, called `slice`.