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