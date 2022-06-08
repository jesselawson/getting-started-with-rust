---
date: "2022-05-29"
title: "Return a value from a function"
slug: "return-value-from-function"
weight: 3
prev_relref: "1-function.md"
next_relref: "3-integer-variable.md"
---

In this section, we're going to create a function called `get_version()` that
will return some arbitrary version number of our tool. 

We saw earlier that a function with no arguments and no return value is written 
like this:

```rust
fn get_version() {  
}
```

When we want a function to return a value, we append `->` and the return value's 
type to the declaration, like this:

```rust
fn get_version() -> u16 {  
}
```



Let's say our version number is `1000`, and we want to return
that from a function and then print it out. Recall that the range of an unsigned
integer is 0 to 2<sup>X</sup>-1, meaning we would need to store the number 1000 
in at least a 16-bit unsigned integer (which has a range of 0 to 65,535). We 
denote that in Rust with the keyword `u16`.

To tell a function to *return* a `u16`, we write it like this:

```rust
fn get_version() -> u16 {
}
```

This declares a function named `get_version` that takes no arguments and returns
a `u16`. You can see that we specify return values by using the `->` set of 
symbols.  

To return the value `1000`, we can either use the `return` keyword (which does
exactly what it sounds like) or we can do what Ruby does and just write the
number. 

The following functions do the same exact thing:

```rust
fn get_version() -> u16 {
  1000
}

fn get_version() -> u16 {
  return 1000;
}
```

In both examples above, the functions will evaluate to the number 1000. This is 
because Rust is considered an expression-based language (like Ruby!). In an 
expression-based language, everything is an expression--and expressions evaluate
to values. That means that a block of code, being an expression, evaluates to
a value. Since a function is a block of code, functions evaluate to a value,
too. 

Notice that we only need the semi-colon when we use the `return` keyword; 
the number 1000 by itself is the value that the block evaluates to, while the 
`return 1000;` is a statement--and statements end with a semicolon.

The accepted way in the Rust community (and in most expression-based languages) 
is to only use the `return` keyword for early returns--that is, for when the 
last statement in a particular block may not be the only value that the block
can evaluate to. For example, if you wanted to print some output based on the
version number, you would do something like this:

```rust
fn get_version() -> u16 {
    let version = 1; // For the sake of example
    
    if version < 2 {
        return 1;
    } 

    2
}
```

Obviously this is a pointless block of code, but you can see that the block will
evaluate to `1`, and in the satisifed *if* expression, we use the `return` 
keyword to indicate an early return. It's an *early* return because the block 
would normally evaluate to `2`, but in the case of the early *if* check, it 
could return a value earlier than when `2` would be returned.

Let's add the new `get_version()` function to our program by calling it from
within the `println!()` macro in `usage()`:

{{<codecaption lang="rust" title="main.rs">}}
fn get_version() -> u16 {
    1000
}

fn usage() {
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
    println!("Version {}", get_version());
}

fn main() {
    usage();
}
{{</codecaption>}}

Here you can see how we pass arguments to `println!()` like we would a typical
`printf` function in another language. Since Rust provides this macro for us, it
can discern what kind of variable you are trying to print so you don't have to
worry about specifying the type. For example, in C's `printf` function you 
would use `%s` to denote where you want the character array to be printed; Rust
only requires you to use `{}`, regardless of the variable's type. (We will be 
printing strings very soon, so sit tight!)

Go ahead and compile, build, and run this with the following command:

```
$ cargo run -q
tinymd, a markdown compiler written by <YOUR NAME>
Version 1000
```

The version integer is being printed, and the `println!` macro can infer
that it's an integer based on the return type specified by `get_version()`. 

We're *almost* done with this chapter. Now that we're confident with Rust's 
function syntax, let's learn how to create and assign a value to a simple 
variable.