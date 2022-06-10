---
date: "2022-06-05"
title: "Making a `Path` variable"
slug: "path-vars"
weight: 3
prev_relref: "2-parser-design.md"
next_relref: "4-line-by-line-parsing.md"
---

Rust goes to great lengths to make sure that your program is cross-platform. One 
of the ways it does this is by providing the `Path` module, which helps format 
strings and string slices into OS-specific path types. The official Rust docs 
recommend to use a `Path` variable instead of a string slice anytime you're 
working with filenames. This is one of several special tools from Rust that 
comes from Rust's `std` library--which, for the purposes of this tutorial, you 
can think of as like a namespace. 

In Rust, we can tell our program that we want to use `Path` objects by going to 
the top of `main.rs` and adding:

```rust
use std::path::Path;
```

Now we can create a `Path` object from the argument passed to the 
`parse_markdown_file()` function:

```rust
fn parse_markdown_file(_filename: &str) {
  print_short_banner();
  println!("[ INFO ] Starting parser!");
  
  // Create a path variable from the filename
  let input_filename = Path::new(_filename);
```

The call to `Path::new()` creates a new `Path` object for us. Now, `input_filename` 
is a `Path` that we can try to open. Again, don't use a `String` object to 
hold a filename, since the `Path` object is specificially designed to play well 
with Rust's other tools for opening, reading, and writing to files. 

With our path now an official Rust `Path` variable, let's pull in another tool 
to be able to open the file that the path points to: `File`.

Just like when we created the `Path` variable, we will first need to declare that 
we want to `use` it by adding the following to the top of `main.rs`:

```rust
use std::path::Path;
use std::fs::File;
```

Notice that `File` comes from `std::fs`, whereas `Path` comes from `std::path`. 

Back to our parsing function, after declaring the `input_filename` path variable, 
let's create a new `File` variable with it:

```rust

  // Create a path variable from the filename
  let input_filename = Path::new(_filename);

  // Try to open the file
  let file = File::open(&input_filename)
             .expect("[ ERROR ] Failed to open file!");
```

Interesting! Now we have a semantic way to open a file using the `File::open()` 
function, to which we pass a reference to `input_filename`, and then to which 
we chain the `.expect()` function. You will encounter the `.expect()` function 
a lot in your Rust development; it's used to remove the verbosity around Rust's 
`Result` type. 

In a nutshell, many functions in Rust do not just return a value, they return 
a `Result`. A `Result` in Rust has two parts: `Ok()` and `Err()`. When you call 
a function that returns a `Result` type, you generally need to check whether 
the function was successful (and thus returned an `Ok()`) or not successful (and 
thus returned an `Err()`). It's akin to exception handling in other languages, 
except here it's baked into the function itself. 

What the `.expect()` does is tell Rust to unwrap the return value and pass along 
the `Ok()`&mdash;except upon failure, in which case the program quits and emits 
a string (`"Couldn't open file"`). 

So when we write something like this:
```rust
let file = File::open(&input_filename).expect("Couldn't open file");
```

We're basically writing a less verbose version of this:
```rust
use std::error::Error;
// ...
let file = match File::open(&input_filename) {
  Err(err) => panic!("Couldn't open file: {}", err.description()),
  Ok(value) => value,
};
```

{{% deepdive title="Why the `panic!()` macro instead of `println!()`?" %}}

The `panic!()` macro respects the return type inferred from `File::open()`, which 
is type `std::fs::File`. If you try to use `println!()` here instead, you will 
get an error that says something like `match arms have incompatible types`. This 
is because `println!()` doesn't know what to do with a `std::fs::File` type, but 
`panic!()` doesn't care what kind of variable you are dealing with. 

As you can see, the verbose way requires a lot more of a deep dive into Rust 
than this tutorial is designed for. For now, the condensed way is good enough 
to build confidence in using Rust; once confidence is there, you will have 
plenty of room to confuse yourself with Rust's esoteric way of doing things--
and ultimately become a stronger developer in all languages because of how Rust 
forces you to think!

{{% /deepdive %}}

There are a lot of new things in the verbose example above, but since you have 
some experience interpreting `match` blocks in Rust already, try to see what's 
going on. Notice, for example, how we can assign a value to a variable based on the 
result of a match block.

