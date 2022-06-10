---
date: "2022-06-05"
title: "Getting the first character of a line"
slug: "first-character"
weight: 5
prev_relref: "4-line-by-line-parsing.md"
next_relref: "6-compiling-markdown.md"
---

With a line from the Markdown file represented as a string variable called 
`line_contents`, we can now figure out what kind of line this is. There are 
really only two kinds of lines we care about for this naive Markdown compiler: 
first-order headings and paragraphs. 

If a line starts with a hash (`#`) symbol, then it's a first-order heading. If, 
however, it starts with any other alphanumeric value, it's a paragraph. 

You might think that we would do something like `line_contents[0]` to get the 
first character of `line_contents`, but Rust doesn't store strings as a 
sequence of characters in memory. Instead, we have to first convert the variable 
into a sequence of characters, take the first one from that sequence, and then 
convert it into something that we can use. 

To do this, we will create a new variable called `first_char` which will (you 
guessed it) hold the first character of `line_contents`. The variable type will 
be a vector of characters, and we will create it like this:

```rust
let mut first_char: Vec<char> = line_contents.chars().take(1).collect();
```

Let's look at the right-hand one piece at a time:

* `line_contents.chars()` says, "Get the `line_contents` variable and convert 
  it to a sequence of characters." (Rust will create an `Iterator` object of 
  characters)
* `.take(1)` says, "Now take the first element of that iterable object." (Rust 
  will convert this `Iterator` into a `Take<char>` object, a special kind of 
  iterator)
* `.collect()` says, "Now convert everything I have retrieved up to this point 
  into a `Collection`--something that I can subsequently use--of the type 
  matching the left-hand variable (which is `Vec<char>`)

It's a lot of steps, but the more you read them from left to right the more 
they will make sense. Rust is a language that feels like C but behaves like Ruby; 
there is a lot going on behind the scenes! At the end of the day, the most 
important part of the above expression is that `collect()` takes all the work we 
did of pulling out the first character and *collects* up the result in a 
vector for us. 

At this point we have `line_contents`, which holds the entire contents of the 
line, and `first_char`, which has the first character of `line_contents`. Neat!

We're about to start doing different things based on what the first character 
of the line is, but before we do, let's create a new string variable that will 
hold the valid HTML that the current line we are reading (`line_contents`) will 
translate to: 

```rust
let mut output_line = String::new();
```

Just like when we created a `Path` variable by using `Path::new()`, we're using 
`String::new()` to declare a mutable variable named `output_line`.

Our main output variable is `tokens`; that's a vector a strings that will contain 
one string object for every line that comes through from the file. When we are 
done processing `line_contents` based on the value of `first_char` (whether it 
is a `#` or not), we will write valid HTML to `output_line` and then push 
`output_line` into `tokens`. Then, in the last section of this chapter, we will 
iterate through the strings in `tokens` and write them all to an output file. 
