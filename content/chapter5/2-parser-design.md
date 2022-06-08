---
date: "2022-06-05"
title: "Designing our parser"
slug: "parsing"
weight: 2
prev_relref: "1-open-parse-file.md"
next_relref: "3-path-vars.md"
---

At this point in our program, `file` is our way of accessing the file passed into 
the function. With the file open successfully, we can start planning how we will 
read and parse it. 

While this is not a course on compiler design, we need to think about how a 
compiler looks at a file. When the program gets to the first character in the 
file, which is a `#` (indicating a heading tag), it needs to know that the rest 
of the line will contain all the text within a heading tag. 

To illustrate, here is the first line of our `test.md`, annotated:

```text
# My favorite author 
^ ^________________^ ^
|         |          |
|         |          +--- end of line; output </h1>
|         |
|         +--- put this inside the <h1></h1> tags
|
+--- start of heading; output <h1>
```

To keep track of what stage the parser is at, we create two kinds of state 
variables called *flags*. The first flag is whether a header tag is open. If a 
header tag is open, that means our HTML file would have a `<h1>` but no 
corresponding `</h1>` yet. 

The second flag we will keep track of is whether a paragraph tag is open. 
Remember: for the purposes of this tutorial, our Markdown compiler can only 
handle first-level headings (`#`) and paragraphs. 

Let's create two mutable boolean variables to keep track of these states:

```rust
let mut ptag: bool = false; // keep track of paragraph tags
let mut htag: bool = false; // keep track of h1 tags
```

Notice that I'm using an underscore (`_`) here to precede their names. In Rust, 
if you instantiate a variable like this but never use the value you use to 
instantiate it, you will get a warning about *unused variable assignments*. Rust 
gets deeply concerned with the fact that you have set a value to a variable that 
is essentially garbage (since the falseness of these flags is just their default 
value going into the for-loop). You can safely remove the underscore, but 
every time you run `cargo build` (or `cargo run` which triggers a build), you 
will get a harmless warning about an unused variable assignment. 

With these two flags we can track what kind of tags we are currently reading for, 
as well as what kind of tag needs to be closed before moving on to the next 
line in the file.

Speaking of lines, we need a way to store the resultant HTML dynamically, so that 
we can add to it before writing it all to a file. What kind of variable type do 
you think we could use for this? 

The way we will do it here is through a vector of strings. I'll call this vector 
`tokens`, to help me remember what's in here. In compiler design, a *token* is 
a keyword, operator, separator, or string literal (or some other kind of lowest 
form of understandable syntax) that the compiler understands. Our vector elements 
are each going to be a `String`, and each string element will be made up of 
either a heading or a paragraph in HTML.

Let's instantiate our `tokens` vector:

```rust
let mut tokens: Vec<String> = Vec::new();
```

We make it mutable because we want to add to it; we use `Vec::new()` to create an 
empty vector object. 

Now we are ready to read the file line by line. Once again, we're going to call 
upon one of Rust's tools for doing just that: `BufReader`, which is, as you can 
derive from the name, a *buffered file reader*. Reading files from the filesystem 
can be a daunting task for programs, especially if these files are gigantic. To 
ensure that a 1gb Markdown file will be read about as fast as a 1mb Markdown 
file, we buffer the input so it reads the file in chunks. Don't worry: all the 
heavy lifting happens behind the scenes. 

Let's go to the top of `main.rs` and add *two* new entries to the block of `use` 
statements:

```rust
use std::io::{BufRead, BufReader};
```

Note that we can combine calls from the same namespace by adding them to a 
comma-separated list between curly brackets. Neat! 

Both `BufReader` and `BufRead` are necessary to do what we want to do. The first 
one lets us buffer a file into memory; the second lets us read those buffers 
line by line.

Back where we left off in the `parse_markdown_file()` function, let's create a 
variable named `reader` that will open our file:

```rust
let reader = BufReader::new(file);
```

There! Not so bad, right? The variable `reader` is now essentially our window 
into a memory-optimized lens through which we can read the file. 

At this point, your `parse_markdown_file()` function should look something like 
this:

```rust
fn parse_markdown_file(_filename: &str) { 
  print_short_banner();
  println!("[ INFO ] Starting parser!");
  
  // Create a path variable from the filename
  let input_filename = Path::new(_filename);

  // Try to open the file
  let file = File::open(&input_filename)
             .expect("[ ERROR ] Failed to open file!");

  let mut ptag: bool = false;
  let mut htag: bool = false;

  // Create a place to store all our tokens
  let mut tokens: Vec<String> = Vec::new();

  // Read the file line-by-line
  let reader = BufReader::new(file);

  // ...
```

With the file now open, we can iterate through it one line at a time and start 
translating Markdown to valid HTML. 