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

## How and when to use Structs

Using tuple can be useful in some scenarios where we want to link values to each other. For example,
if we want to calculate the area of a rectangle, we could use a tuple as follows:

```rust
fn main() {
    let rectangle = (10, 20); // tuple containing width and height
    
    compute_area(rectangle); // pass a tuple struct as an argument  
    
    fn compute_area(dims: (i32, i32)) -> i32 {
        dims.0 * dims.1
    }
}
```

The above implementation uses a tuple struct `Rectangle`, and we pass the width and the height to it
together as a tuple. But what if we wanted to plot the rectangle? We would then need to keep track
of which index represents the `width` and `height`. That can get messy very fast.

### Using `struct` instead

Instead of passing a tuple, we can improve the readiblity of the code above by creating a struct `Rectangle`
and assigning the width and height to it as follows:

```rust
fn main() {
    struct Rectangle {
        width: i32,
        height: i32,
    }

    let rect = Rectangle {
        width: 10,
        height: 20
    };

    compute_area(&rect);

    fn compute_area(rect: &Rectangle) {
        let rect_area = rect.width * rect.height;

        println!("The area of the rectangle is {rect_area}")
    }
}
```

We can see that the implementation that we have now is much easier to understand since the paremeters
`width` and `height` are linked to the struct `Rectangle`.

### Adding more functionality to structs

So far, we have not tried to print a `struct` object in rust. Let's try the following to print a `struct`
object:

```rust
fn main() {
    struct Rectangle {
        width: i32,
        height: i32,
    }
    ;

    let rect = Rectangle {
        width: 10,
        height: 20
    };

    println!("The rectangle is {rect}", rect = rect);
}
```

When run, we should get the following error:

```bash
error[E0277]: `Rectangle` doesn't implement `std::fmt::Display`
  --> src/main.rs:15:60
   |
15 |     println!("The area of the rectangle is {rect}", rect = rect);
   |                                                            ^^^^ `Rectangle` cannot be formatted with the default formatter
   |
   = help: the trait `std::fmt::Display` is not implemented for `Rectangle`
   = note: in format strings you may be able to use `{:?}` (or {:#?} for pretty-print) instead
   = note: this error originates in the macro `$crate::format_args_nl` which comes from the expansion of the macro `println` (in Nightly builds, run with -Z macro-backtrace for more info)
```

Okay, compiler says let's try to print with `{:?}` instead. Let's try that:

```rust
use std::io;

fn main() {
    struct Rectangle {
        width: i32,
        height: i32,
    }


    let rect = Rectangle {
        width: 10,
        height: 20
    };

    println!("The rectangle is {:?}", rect);
}
```

When run, now we get another error:

```bash
error[E0277]: `Rectangle` doesn't implement `Debug`
  --> src/main.rs:15:51
   |
15 |     println!("The area of the rectangle is {:?}", rect);
   |                                                   ^^^^ `Rectangle` cannot be formatted using `{:?}`
   |
   = help: the trait `Debug` is not implemented for `Rectangle`
   = note: add `#[derive(Debug)]` to `Rectangle` or manually `impl Debug for Rectangle`
   = note: this error originates in the macro `$crate::format_args_nl` which comes from the expansion of the macro `println` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider annotating `Rectangle` with `#[derive(Debug)]`
   |
4  |     #[derive(Debug)]
   |
```

It error now says `Rectangle doesn't implement Debug`. 

In rust, `Debug` is a trait that allows us to print the data of a variable such that it is easier for
us to debug. The compiler also gives us help: `add '#[derive(Debug)]' to Rectangle or manually 'impl Debug 
for Rectangle'`

To resolve this issue, we can modify our code as following:

```rust
fn main() {
    #[derive(Debug)] // we added the Debug trait to our struct
    struct Rectangle {
        width: i32,
        height: i32,
    }

    let rect = Rectangle {
        width: 10,
        height: 20,
    };

    println!("The rectangle is {:?}", rect); // this should now work and show the data in the struct
}
```

Run the above code and be in for a surprise!

### Attaching a method to a `struct`

The function to compute area is very specific to rectangles, and will not work for circles or triangles.
In that, we should link the `compute_area` function to the `Rectangle` struct.
This would be attaching a `method` to the struct. Let's see how we can do that:

```rust
fn main() {
    struct Rectangle {
        // create struct Rectangle
        width: i32,
        height: i32,
    }

    let rect = Rectangle { // create an instance of Rectangle
        width: 10,
        height: 20,
    };

    impl Rectangle { // start implementation block
        fn area(&self) -> i32 { // associate method to Rectangle struct via &self
            self.width * self.height
        }
    }

    println!("The area of the rectangle is {}", rect.area());
}
```

We see that using the keyword `impl` and then the name of the struct, we are able to create a method 
for that struct. For the method that we create, we need to first argument to be the `self` keyword,
so that the method knows that it is associated with the struct. We can then use the `self` keyword
to access the values of the struct.

`Important Note`: In rust, instead of using the `self` keyword directly, we use the `&self` keyword
to avoid taking ownership of the struct. The documentation also mentions that it is very rare
that a method takes ownership of the struct, and most of the time we just use the reference of the
struct as input via `&self`.

In rust, we can also set `getters` for attributes by creating methods for them. Getters are not set by
default in rust, unlike in `Python` or other languages. When creating a `getter` method, we usually
set the name of the method equal to the name of the attribute.

### Methods with more Parameters

We can keep multiple parameters in the method, and use them as we would in any other function. We 
can also link multiple methods to a struct. Let's see how we can do that:

```rust