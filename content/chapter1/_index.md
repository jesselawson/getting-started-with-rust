---
date: "2022-05-28"
title: "1: A new project"
prev_relref: "about/_index.md"
next_relref: "chapter1/1-structure.md"
slos:
  chapter-1:
    number: "1"
    objectives:
    - Create a new Rust project on the command line without errors 
    - Compile and build a simple "Hello, World" Rust project without errors
---

{{<objectives chapter="1">}}

In _Getting Started with Rust_, we will create a Markdown compiler called 
**TinyMD**, which will take a markdown file as input, convert the markdown to
HTML, then write the HTML to a new file. 

To start, let's create a new project with `cargo`, the package manager and 
project building tool that comes from [installing Rust](https://rustup.rs). 

We will scaffold this and all projects in Rust using the `cargo` tool, passing 
the `new` argument and specifying what we want:

```
$ cargo new tinymd --bin
```

The command `cargo new` builds a new project, and the `--bin` flag tells Cargo 
that you want this project built as an executable (called `tinymd`) instead of 
a library.

After running the command, you will get a confirmation that the project was 
scaffolded correctly. Go ahead and open the project's root folder, `tinymd`.