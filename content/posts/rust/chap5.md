---
title: "Rust Documentation: Chapter 5"
date: 2023-05-03T15:42:18
description: "This post gives a breakdown of the key takeaways from Chapter 5 of the Rust Documentation"
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

In the [previous chapter]({{< ref "chap4-slices.md" >}} "Rust Chapter 4 - Slices"), we saw we can use slices as references 
to a contiguous sequence of elements in a collection instead of the whole collection. In this chapter, we will 
deviate from Ownership rules and see how we can use `structs` to create custom data types.

## Structs

In Rust, and in object-oriented programming languages, a `Struct` or structure is a custom data type that lets you
hold multiple values in relation to it. It is like a tuple in the sense that a tuple also holds multiple values, but
a tuple does not have names associated with the values. A `Struct` is similar to a `tuple` in the sense that it also
holds multiple values, but it differs in the sense that it has names associated with the values.

A struct is defined as follows:

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

and is initialized as follows:

```rust
fn main() {
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
}
```

`Note: Syntax`: When initializing a struct, the order of fields does not matter. We need curly brackets containing *key-value*
pairs for each of the field of a struct.

We can extract values from a `struct` using the `.` operator followed by the key.

`Note: Design Choice`: If we want to modify a field of a `struct`, the entire `struct` has to be mutable, rather than a 
single field. 

Structs can also be return values of functions, as shown below:

```rust
struct User { // defining a struct
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}


fn build_user(email: String, username: String) -> User {
    User { // returning an initialized struct
        email: email, // we can use shorthand syntax and just put 'email' instead of 'email: email'
        username: username, // can also use shorthand syntax here
        active: true,
        sign_in_count: 1,
    }
}
```

### Creating a new struct by reusing values from an old struct

The `struct update` syntax helps us reuse most of the values from a struct to create a new struct. This is useful when
only have to modify a few values in the new `struct`.

Suppose we have a `user1` object from the `User` struct as follows:

```rust
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
};
```

Let's say we want to create another user that only has a different email, but the rest of the values are
the same as that of `user1`. This can be done as follows:

```rust

let user2 = User {
    email: String::from("user2email@example.com"),
    active: user1.active, // we can just get the value from user1 object
    username: user1.username, // we can just get the value from user1 object
    sign_in_count: user1.sign_in_count, // we can just get the value from user1 object
};
```

An even better shorthand for this is as follows:

```rust
let user2 = User{
    email: String::from("user2email@example.com"),
    ..user1 // take values for the rest of the fields as defined in 'user1'
}
```

### Ownership of Struct Data

In the above example, when we create `user2`, we will no longer have access to `user1` object. This is
because of the `=` operator. When we use the `=` operator, the value is moved from the old variable to the new
field, and the field `username` of `user1`, which is a `String`, will transfer ownership to `user2`.

On the other hand, if both `email` and `username` fields are changed for `user2`, then `user1` will still be
valid, as the ownership of the `String` values will not be transferred to `user2`, and `active` and `sign_in_count`
are both stored on `Stack` as their sizes are known at compile time. Boom!

### Tuple Structs

We can also define structs that look like tuples, but are different types. These are called `tuple structs`. They
have no key names associated with the values, but they are different types. For example:

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);
```

are two different tuples of type `Color` and `Point` respectively, even though they have the same number of values
and the same types of values. A function that takes `Color` type as an argument cannot take a `Point` type
as an argument, even though they look the same.
