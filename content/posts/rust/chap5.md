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