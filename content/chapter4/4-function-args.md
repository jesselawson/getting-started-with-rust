---
date: "2022-06-05"
title: "Passing arguments to functions"
slug: "function-args"
weight: 4
prev_relref: "3-match-blocks.md"
next_relref: "5-chapter4-checkpoint.md"
---

The last thing we need to do in the `main()` function is to pass the second 
argument into the function that parses the markdown file--`parse_markdown_file()`. 
To do this, we need to change the declaration of `parse_markdown_file()` to 
accept a single argument: a string slice that is the filename to parse. 

To add an argument to a function, you declare it the same way you would a 
regular variable, except this time you can omit the `let`. Let's modify the 
function to accept a string slice argument named `_filename`:

```rust
fn parse_markdown_file(_filename: &str) {

}
```

I'm using an underscore (`_`) here in the filename variable to remind me that 
this is coming from a function parameter. Feel free to name it whatever you want. 

Let's also go ahead and put some placeholder text in there to help us see when 
this function is called:

```rust
fn parse_markdown_file(_filename: &str) {
  print_short_banner();
  println!("[ INFO ] Trying to parse {}...", _filename);
}

```

Here I have it outputting the short banner and then the informational message.

Now, if we were to invoke the tool with `cargo run -q afile.md`, it *should* 
tell is `[ INFO ] Trying to parse afile.md...`. We will continue fleshing out 
this function in the next chapter.

Finally, we have to actually pass the filename to the function, back down 
in the match block we had finished earlier. When `args.len()` is `2`, we want to 
pass the second element in the `args` vector to `parse_markdown_file()`. To do 
that, we will simply pass it as a string slice (since the function accepts a 
`&str` as the argument):

```rust
// ...
match args.len() {
  2 => parse_markdown_file(&args[1]),
  //...
}
```

Notice how we access the 2nd element of `args` the same way we might do it in 
other languages. Also note that we are using the reference operator here; we 
don't want to pass `args[1]` in, since that would *move* the value into the 
function--causing `args[1]` to be null. *Remember: in Rust, assignment is a 
move, not a copy!*

Go ahead and build and run your project: `cargo run -q`. Here's what mine says:

```
$ cargo run -q
[ ERROR ] You forgot to specify the markdown file to parse!
tinymd (v0.1.0), A tiny markdown compiler based on Jesse's tutorials.
Written by: Jesse Lawson <drwho@nsa.gov>
Homepage: https://jesselawson.org/rust
Usage: tinymd <somefile>.md
```

Next, let's pass it the name of a fake file to see if we setup our logic correctly:

```
$ cargo run -q test.md
tinymd (v0.1.0), A tiny markdown compiler based on Jesse's tutorials.
[ INFO ] Trying to parse test.md...
```

Neat!