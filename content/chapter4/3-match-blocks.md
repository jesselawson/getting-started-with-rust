---
date: "2022-06-05"
title: "The `match` block"
slug: "match"
weight: 3
prev_relref: "2-how-to-parse-cli-args.md"
next_relref: "4-function-args.md"
---

With the arguments collected into `args`, we now want to make sure that there 
are only two of them. Recall that our goal is to accept just one argument: the 
name of a Markdown file to parse. This means that we have two total elements in 
the `args` vector: the name of the program, and the name of the Markdown file. 

We only handle invocation that includes two arguments, so the length of the 
`args` vector should be 2. To check this, we'll use a `match` block to match 
the length of the vector with either 2 (the length we want) or not 2:

```rust
fn main() {

    let args: Vec<String> = std::env::args().collect();

    match args.len() {
      // ...
    }

}
```

Here you can see that we grab the length of the vector by calling `.len()` on it. 
The result is an integer value. To match values in this match block, we declare 
them like this: `left => right`. If `args.len()` equals  `2`, then we want to 
call `parse_markdown_file()`:

```rust
match args.len() {
  2 => parse_markdown_file(),
```

You'll notice the comma at the end; the match rule statements (`left=>right`) are 
comma separated. If we wanted to put one before, and call `usage()` if no file 
was passed, we might do something like this:

```rust 
match args.len() {
  1 => usage(),
  2 => parse_markdown_file(),
  // etc. etc. 
```

In this case, checking for one argument or more than two arguments is redundant; 
instead of checking twice for a number of arguments not equal to `2`, we can use 
the default case for `match`, which is a `_` (underscore):

```rust
match args.len() {
  2 => parse_markdown_file(),
  _ => usage()
}
```

The default match case (`_`) will trigger if no other match case triggers. 

The match cases don't have to be a call to a single function. They can also be 
a block themselves:

```rust
match args.len() {
  2 => parse_markdown_file(),
  _ => { 
    println!("[ ERROR ] Invalid invocation (you done goofed!)");
    usage();
  }
}
```

At this point, our main function can check whether there are two arguments--the 
name of the program (arg 1) and the Markdown file to be parsed (arg 2), and call 
the appropriate function (`parse_markdown_file()`) if there are only two arguments. 