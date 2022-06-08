---
date: "2022-05-28"
title: "3: Strings & memory"
slos:
  chapter-3:
    number: "3"
    objectives:
    - Create a string variable without errors
    - Return a string variable from a function without errors
    - Concatenate two strings without errors
    - Print a string to the command line without errors
prev_relref: "chapter2/4-chapter2-checkpoint.md"
next_relref: "1-strings.md"
---

{{<objectives chapter="3">}}

Our goal in this chapter is to develop confidence about a few of the basic string 
operations in Rust.

Our tiny markdown compiler is only just being born. We've got a project setup, 
and if we run the program we are able to output a *banner*--a block of helpful 
text that usually says what the program is, who wrote it, and how to use it. We 
have wrapped up this banner in the `usage()` function. By the end of the last 
chapter, we were outputting the version of our program. 

In this chapter, we will improve the banner by pulling data from our project's 
manifest file (`Cargo.toml`) instead of hard-coding things like the version 
number into `main.rs`. To do this, we need to understand how Rust deals with 
strings and, perhaps most importantly, what makes Rust wholly different from 
many languages you might already be used to: *ownership*. 
