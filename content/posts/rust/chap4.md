---
title: "Rust Documentation: Chapter 4"
date: 2023-04-29T11:23:17
description: "This post gives a breakdown of the key takeaways from Chapter 4 of the Rust Documentation"
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

In the [previous chapter]({{< ref "chap3.md" >}} "Rust Chapter 3"), we saw how we can use data types, functions,
and control flow in `rust`. In this chapter, we will look at the most unique feature of `rust` - ownership.

## Ownership

Ownership enables Rust to make memory safety guarantees without needing a garbage collector, so it’s important 
to understand how ownership works. In this chapter, we’ll talk about ownership as well as several related 
features: borrowing, slices, and how Rust lays data out in memory.