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
