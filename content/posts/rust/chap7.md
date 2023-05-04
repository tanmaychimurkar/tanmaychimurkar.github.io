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

In the [previous chapter]({{< ref "chap6.md" >}} "Rust Chapter 4 - Slices"), we saw we can use slices as references 
to a contiguous sequence of elements in a collection instead of the whole collection. In this chapter, we will 
deviate from Ownership rules and see how we can use `structs` to create custom data types.
