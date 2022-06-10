---
date: "2022-05-28"
title: "5: Reading & writing files"
slos:
  chapter-5:
    number: "5"
    objectives:
    - Open a file without errors
    - Read a file line-by-line without errors
    - Describe how a Markdown compiler works
    - Write to a file without errors
prev_relref: "chapter4/5-chapter4-checkpoint.md"
next_relref: "1-open-parse-file.md"
---

Our Markdown compiler is now at a point where it can accept command-line input 
and reliably displays some information based on the input provided.

In this chapter, we will configure our tool to read and parse a Markdown file,
translate it into HTML, then write that HTML to a new file.

{{<objectives chapter="5">}}

By the end of this chapter, we will have a working (and naive) Markdown compiler 
written in Rust.
