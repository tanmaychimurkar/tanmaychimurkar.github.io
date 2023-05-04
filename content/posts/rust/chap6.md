---
title: "Rust Documentation: Chapter 4 - Slices"
date: 2023-05-03T10:12:10
description: "This post gives a breakdown of the key takeaways from Chapter 4 - Slices the Rust Documentation"
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

In the [previous chapter]({{< ref "chap5.md" >}} "Rust Chapter 4 - References"), we saw how references work in Rust
when we do not directly want to transfer ownership. In this chapter, we will see how we can use `slices` to reference
a contiguous sequence of elements in a collection instead of the whole collection.

## Slices

Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection. 
A slice is a kind of reference, so it does not have ownership.

### Why are slices useful?

Let's take the example from the Rust documentation. We need to write a function that takes a string, and
returns the first word separated by a space or the whole word if space is not found. The definition of the function
would take in a string `reference` as we do not want to take ownership, but what should be the return type?

It could be the whole string, but that would mean that we are taking ownership of the string. We could return an
index of the location where we see a space or the index of the last character depending on whether we
could find a space. Let's try the second approach and create a function.

```rust
fn get_first_word(s: &String) -> usize {
    let bytes = s.as_bytes(); // convert string to array of bytes

    for (i, &item) in bytes.iter().enumerate() { // iterate over the array of bytes by index
        if item == b' ' { // match the byte with the space character
            return i; // return index of the space character, function ends if found
        }
    }

    s.len() // return the length, i.e., index of the last character of string, function ends
}
```

Even though our implementation is correct and would work, we still have one problem. The index we derive from the
function is only valid as long as the input word itself does not change. What if the string passed as reference 
to `s` changes in value or is dropped altogether? Then the index we return would no longer be valid and would
point to a different value. This can be seen in the following example.

```rust
fn main() {
    let mut some_string = String::from("Hey there!"); // create a variable
    
    let first_word = get_first_word(&some_string); // returns 3, index of space character
    
    some_string.clear(); // string is cleared, but first_word variable still contains the index
}
```

We can see from the above example that even if the input attribute to the function is cleared, 
the value that the function holds is not cleared, but becomes invalid in comparison to the changed
string. This is where slices come in.

### String Slices

String slices are references to parts of string that we can create. It can be created as follows:

```rust
let s = String::from("Hello World!");

let first_slice = &s[0..5]; // only takes the first 5 characters
let second_slice = &s[6..11]; // takes the last 5 characters, the word 'World'
```

Internally, a slice stores the start index and the length of the slice to store defined by the last
index. 

Using the above, let's try to rewrite our initial function of getting the first word of a string. 
String slice references are represented as `&str` in Rust.

```rust
fn get_first_word(s: &String) -> &str {
    let bytes = s.as_bytes(); // convert string to array of bytes
    
    for (i, &item) in bytes.iter().enumerate() { // iterate over the array of bytes by index
        if item == b' ' { // match the byte with the space character
            return &s[0..i]; // return the slice of the string from 0 to index of space character
        }
    }
    
    &s[..];
}
```

Even though much of the logic of the code remains the same to our initial implementation of the `get_first_word`
function, we now return a string slice which will remain valid if the input string changes. Moreover, the
compiler will make sure that the string slice returned is valid and does not point to an invalid index.

If we now try to drop the string after the function call, we will get an error as the string slice returned 
as follows:

```rust
fn main() {
    let s = String::from("Hello World!");
    
    let first_word = get_first_word(&s);
    
    s.clear(); // compiler will throw an error as the string slice returned is invalid
}
```

If we run the above code, we will get the following error:

```bash
$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
  --> src/main.rs:18:5
   |
16 |     let word = first_word(&s);
   |                           -- immutable borrow occurs here
17 |
18 |     s.clear(); // error!
   |     ^^^^^^^^^ mutable borrow occurs here
19 |
20 |     println!("the first word is: {}", word);
   |                                       ---- immutable borrow later used here

For more information about this error, try `rustc --explain E0502`.
error: could not compile `ownership` due to previous error
```

This is coherent with the `borrowing` references we saw in [Chapter 4 - References](chap5.md), where we saw
that if we have an immutable reference to something, we cannot also take a mutable reference.
The `s.clear()` call will take a mutable reference to `s` to clear the string out, but this is not allowed
in Rust. Neat!

### String Literals are Slices

When we create a string literal, we are actually creating a string slice. For example, `"Hello World!"` is
a string literal, and its type is `&str`. This is because string literals are stored in the binary of the
program, and are therefore immutable. This is why we cannot add to a string literal, but we can add to a
`String` type.

### Other slice types

We can also create slices on `array` types in rust just as we created them for `string` types. For example,
we can create a slice on an array of integers as follows:

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3]; // slice of array from index 1 to 3

&assert_eq!(slice, &[2, 3]);
```

### Summary

All the concepts we have seen in Chapter 4, 5 and 6 are very important to understand the concept of ownership,
borrowing, and memory safety in rust. We will see more of these concepts in the next chapter, where we will
see how to use these concepts to create a program that is memory safe and does not have any memory leaks.

Let us now look at the [next](chap7.md) chapter on `Struct` in Rust!