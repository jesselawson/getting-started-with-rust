---
date: "2022-05-29"
title: "Create a function in Rust"
slug: "create-function"
weight: 2
prev_relref: "_index.md"
next_relref: "2-function-with-return-value.md"
---

The first thing we are going to write in Rust is a function.

A function is defined with the `fn` keyword, like this:

```rust
fn main() {
    println!("Hello, world!");
}
```

Above our `main()` function, let's create a new function called `usage()`. 
Inside it, we'll use the same  used to write `"Hello, world!"` to output the name
and a short description of our tool:

```rust
fn usage() {
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
}

fn main() {
    println!("Hello, world!");
}
```

Go ahead and replace `<YOUR NAME>` with your name if you haven't already done
so. 

Compile and run your project with `cargo run -q`, which will always automatically 
detect changes in your source files and rebuild your project if needed:

```
$ cargo run -q
Hello, world!
```

{{< expandable label="What was -q for again?" level="4" class="test">}}
The `-q` flag in `cargo run -q` invokes "quiet" mode.

Here's without:

```
 $ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
      Running `target\debug\tinymd.exe`
  Hello, world!
```

Here's with: 

```
$ cargo run
Hello, world!
```

{{< /expandable >}}

{{< expandable label="What if I see a `warning: function is never used` message?" level="4" >}}
If you run the code above, you'll likely see a warning that looks like this:

```
warning: function is never used: `usage`
 --> src/main.rs:1:4
  |
1 | fn usage() {
  |    ^^^^^
  |
  = note: `#[warn(dead_code)]` on by default
```

If it wasn't already apparent, you can *ignore* this warning because it is just 
Rust telling us we have a function that we've declared but not used. This warning 
will go away once we actually call the function.
{{< /expandable >}}

If you don't really care to look at the accompanying information (`Finished dev`
and `Running` lines), you can pass the `-q` flag to Cargo, which tells it to be
quiet:

```
$ cargo run -q
Hello, world!
```

To save space, that's how we'll be compiling, building, and running the code as
we make changes to it&mdash;so don't panic when you see the `-q` flag.

Now that we have a function to print the banner, let's replace the `println!` command 
in the main function with a call to `usage()`: 

```rust 
fn usage() {
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
}

fn main() {
    usage();
}
```

Finally, let's compile, build, and run, all in one command:

```
$ cargo run -q
tinymd, a markdown compiler written by <YOUR NAME>
```

Neat! 

But an argumentless and returnless function is about as useful as an 
inflatable dart board. 

To improve our `usage()` function, let's have it return a simple value that we 
can write to the console window. 
