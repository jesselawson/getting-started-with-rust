---
date: "2022-06-03"
title: "Creating a string variable in Rust"
slug: "create-string"
weight: 2
prev_relref: "_index.md"
next_relref: "2-return-a-string.md"
---

In most languages, the concept of strings have been abstracted away to just 
text between quotes `"like this"`. Rust requires that we become very familiar 
with how a string is understood by a computer, which is as a collection of 
bytes. In Rust's case, these bytes are guaranteed to be valid UTF-8. 

Every language has a way to implement a collection of bytes in one of two ways: 
*heap-allocated* strings, which are dynamically created at runtime and during 
the application's lifecycle, and *stack-allocated* strings, which are 
allocated at compile-time.

Rust calls these "strings" and "string slices"; `String`, which is allocated 
on the heap, and `&str` (called a *string slice*), which may be stack or heap 
allocated depending on what it points to. 

Think of a `String` like a vector. It can grow and shrink, you can push and pop 
elements into and out of it,  and it is automatically freed when it goes out 
of scope. Further, a `String` has its own buffer in memory; it is said to be 
the *owner* of the memory where the bytes are stored. 

Since stack-allocated variables must have a known size at compile time, 
only `String` variables retain ownership over their addresses in memory when 
they're changed. 

A `&str`, on the other hand, is a *string slice*. It does not own any buffers 
in memory, but rather, it *borrows* whatever is at an address from a different 
owner. You can think of a `&str` as pointer or **borrowed reference** to a 
string owned by either adifferent variable or the application itself. For this 
reason, string slices are always immutable. 

## Should I use a String or a &str?

A `String` can be mutable; a `&str` is always immutable.

For example, in our markdown compiler, we will be using `String`s to hold the 
value of each block of HTML code; if we were going to write 
`<h1>Hello, world!</h1>`, we would `push()` the first tag, then the inner 
content, then the closing tag. Using a string slice wouldn't make sense here 
since the value of the `String` will change as we add ("push") data 
into it. 

A `&str`, on the other hand, is a window into another string, whether that 
string is a string literal (in which case the string slice would be static and 
stack-allocated) or a `String` (in which case the string slice would be 
heap-allocated).

## Using strings

Let's see how both of these string types can be used effectively by modifying 
our `the_version` variable to be a `&str`&mdash;and while we're at it, let's change 
the version of our markdown compiler to be a more traditional early prototype 
version (`0.1`) instead of `1000`:

```rust
fn usage() {
    let the_version: &str = "0.1";
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
    println!("The Version: {}", the_version);
}
```

Here we have turned `the_version` into a string slice (`&str`). Rust is going to 
see the `"0.1"` and know to compile that value as a static string in the program, 
and then has `the_version` borrow that value. 

The reason we say it *borrows* the value there is because technically it's not 
the *owner* of the buffer where `"0.1"` lives&mdash;the application is. 
(Anytime a string literal like `"0.1"` is created at compile time, it is 
created as a *static string slice*, which means it exists in the binary 
code that makes up the executable and cannot be changed.)

The Rust compiler is smart enough to infer `the_version`'s type based on 
the provided value (`"0.1"`), so we can rewrite the declaration to omit the type:

```rust {linenostart=2}
let the_version = "0.1";
```

Here we omit the `: &str` part of the declaration because Rust will infer that, 
since `"0.1"` is a string literal and all string literals become static string \
slices, `the_version` needs to be a `&str`.

When Rust goes to compile our program, the string `"0.1"` is compiled into the 
program as a string literal (essentially a static string) and thus gets 
instantiated in stack memory, and `the_version` (which is allocated dynamically 
at runtime in heap memory) *borrows the value* at the address in stack memory 
where Rust stored it.

Our program will build and run just fine as long as we comment out our old 
`get_version()` function. Ensure your `main.rs` looks like this now:

{{<codecaption lang="rust" title="main.rs">}}
// Comment this out for now; we will come back to it soon
/*fn get_version() -> u16 {
    1000
}*/

fn usage() {
    let the_version = "0.1";
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
    println!("The Version: {}", the_version);
}

fn main() {
    usage();
}
{{</codecaption>}}

Go ahead and build and run it:

```
$ cargo run -q
tinymd, a markdown compiler written by <YOUR NAME>
The Version: 0.1
```

Though it runs fine, we haven't improved anything by baking in a string version 
instead of an integer version. What we want to do instead is to pull the actual 
version of the application from the project's manifest file (`Cargo.toml`). Rust 
gives us a macro to do just that: `env!()`, which returns the value associated 
with a particular key.

Let's take a look inside `Cargo.toml` right now, to see what we are working with:

{{<codecaption lang="toml" title="Cargo.toml">}}
[package]
name = "tinymd"
version = "0.1.0"
authors = ["Jesse Lawson <jesselawson@protonmail.com>"]
edition = "2018"
{{</codecaption>}}

Many other languages use manifest files like Rust's `Cargo.toml`, such as Node 
(`package.json`) and Ruby (`Gemfile`). The information here is fairly 
straight-forward. These variables in here are sometimes called *environment* 
variables. Rust will provide the key values from the manifest file as environment 
variables for us during compilation.

Our goal is to pull out variables from this manifest file and stick them into our 
`usage()` function. In doing so, we will then be able to display the string values
we see in the manifest file as parts of the banner of our application. Neat!

To do this, we will use the `env!()` macro I mentioned earlier. Remember that 
macros in Rust are, as far as we are concerned, basically the same as 
functions--except a macro has an exclamation point after its name to denote 
that it is a macro. The `env!()` macro takes one argument: a string 
key corresponding to the variable we want. 

The following are the string keys we are going to be using:

* `CARGO_PKG_VERSION` - The full version of your package.
* `CARGO_PKG_AUTHORS` - Colon separated list of authors from the manifest of 
your package.
* `CARGO_PKG_NAME` - The name of your package.
* `CARGO_PKG_DESCRIPTION` - The description from the manifest of your package.
* `CARGO_PKG_HOMEPAGE` - The home page from the manifest of your package.

As you may have guessed, each of these string keys corresponds to a key from 
the manifest file. For example, the `version` key in the manifest file is retrieved 
by passing `CARGO_PKG_VERSION` to `env!()`. 

You'll notice that there are some fields in the list of string keys above that 
are not in the manifest file. The reason they aren't there is because they 
are not part of the default scaffolding. Let's go ahead and add them; you are free 
to set these to whatever you would like.

Go to the `Cargo.toml` file and add entries for description and homepage, then
modify the name, authors, and version as you see fit:

{{<codecaption lang="toml" title="Cargo.toml">}}
[package]
name = "tinymd"
version = "0.1.0"
authors = ["Jesse Lawson <drwho@nsa.gov>"]
edition = "2018"
description = "A tiny markdown compiler based on Jesse's tutorials."
homepage = "https://jesselawson.org/rust"
{{</codecaption>}}

*Note: The `edition` field lets you target a specific edition of Rust. Don't 
change this; use the value that Cargo put in there for now.*

Looking good. Next, we're going to create a function that gets us one of the 
environment variables. Let's do the version first. Knowing that `env!()` takes a 
single string key as an argument and returns the environment variable from the 
manifest file, how do you think we would do that?

One way is to just set the value of `the_version` to be the result of a call to 
`env!()`:

```rust
// ...
let the_version = env!("CARGO_PKG_VERSION");
println!("Version: {}", the_version);
// ...
```

However, a smarter way to generate a banner is through a single function call. In 
other words, anytime we would need to print out the tool's banner, we should be 
able to do it with a single function call: `usage()`. To do that, we would need 
to move the version variable into the `usage()` function. While we're at it, 
let's go ahead and encapsulate the work of getting the version out into its 
own function--and replacing `get_version()` with something a little more helpful.