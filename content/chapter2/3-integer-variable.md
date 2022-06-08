---
date: "2022-05-29"
title: "Create an integer variable"
slug: "integer-variable"
weight: 4
prev_relref: "2-function-with-return-value.md"
next_relref: "4-chapter2-checkpoint.md"
---

The first kind of variable we will learn about is the integer. All variables in 
Rust are declared by putting their type *after* their name. For example, if we 
want to create an integer variable to hold the version of our application, we 
would declare a variable `version` like this:

```rust
let version: u16;  // I'm using u16 for the sake of example only. This could be
                   // a u8, too. It just depends on your variable size.
```

Recall that `u16` is short for *unsigned 16-bit integer*.

Variables in Rust are declared with the `let` keyword, and then we use a colon 
(`:`) to describe the variable's type. All variables need to be declared like
this, unless the variables type can be inferred by, say, the return value of a 
function. Let's practice using both ways to declare a variable.

To store our arbitrary application version in a variable, let's declare it 
within the scope of the `usage()` function. Then, instead of having `println!` 
use the function `get_version()` to print the version, we'll have it use our 
local variable:

```rust
fn get_version() -> u16 {
    1000
}

fn usage() {
    let the_version: u16;  // Declare our variable
    the_version = get_version();  // Assign a value to our variable
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
    println!("Version {}", the_version);  // Print the value assigned
}

fn main() {
    usage();
}
```

As you can see, substituting the function with the variable in `println!` is
fairly straightforward. Additionally, look at how we declared `the_version`,
then passed the value of the function to it. Rust infers the type of variable we
want based on the return value of the function we use to assign it a value.

We can improve this by letting Rust infer `the_version`'s type:

{{<codecaption lang="rust" title="main.rs">}}
fn get_version() -> u16 {
    1000
}

fn usage() {
    let the_version = get_version();
    // ...
    println!("Version {}", the_version);
}

fn main() {
    usage();
}
{{< /codecaption >}}

Neat!

