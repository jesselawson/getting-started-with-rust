---
date: "2022-06-05"
title: "Line-by-line parsing"
slug: "line-by-line-parsing"
weight: 4
prev_relref: "3-path-vars.md"
next_relref: "5-first-character.md"
---

Now it's time to actually do something with every file line that we read. To do 
that, we want to loop through the lines in the file that we are given with the 
`reader` variable. This `reader` variable, being assigned the value of the 
`BufReader::new(file)` call, now gives us a `.lines()` method that we can iterate 
over with a simple for-loop. 

In Rust, a for-loop's syntax is *"for x in y"*, like this:

```rust
for line in reader.lines() {
```

Perfect: now each *line* buffered into `reader` can be parsed. 

We're not quite ready to read the line yet, though. Remember when we did that 
condensed and verbose example of `File::open()` because it returned a `Result` 
object? Well, `line` here is also a `Result` object! So how do we get the value 
of the line--the actual contents--out? 

You might think we need to use a `match` block, like the verbose example for 
`File::open()`. While you can do it this way, there's really nothing we care to 
do if the line is empty (i.e., if the `Result` object produces an `Err()`). Think 
about it: if the file was opened correctly, and the contents were buffered 
correctly, what could cause the `line` variable within this for-loop to be an 
error? 

Well, it might be the end of the file! So instead of doing error checking on 
this line every time, we'll just do a Rust trick called *unwrapping*. When you 
*unwrap* an Result object, you are telling Rust that you 1) expect the value 
to be available, and 2) don't care if the value is garbage. 

If you wanted to do it the verbose way, you might write:

```rust
let line_contents = match line {
      Ok(contents) => contents,
      Err(e) => panic!("Garbage: {}", e.description())
};
```

Notice how we would have to match each of the `Result` elements (`Ok()` and 
`Err()`). We don't really need all this because, frankly, it's a bit overkill 
for just reading the contents of a line in a toy markdown compiler. So instead 
of manually unwrapping the `Result` object, we can just use Rust's `.unwrap()` method:

```rust
// Verbose way:
/*let line_contents = match line {
      Ok(contents) => contents,
      Err(e) => panic!("Garbage: {}", e.description())
};*/

// Condensed way:
let line_contents = line.unwrap();
```

Let's look at what we have so far for the for-loop, complete with some comments:

```rust
// Loop through the reader lines
for line in reader.lines() {
  // For each line, unwrap it
  let line_contents = line.unwrap();
```

You're doing great! Get a drink of water, stretch your back and legs, and come 
back when you're ready. 