---
date: "2022-06-04"
title: "Return a string from a function"
slug: "return-string-from-function"
weight: 3
prev_relref: "1-strings.md"
next_relref: "3-chapter3-checkpoint.md"
---

Recall that there are two types of strings in Rust: there's the vector-esque 
`String`, and the string slice `&str`. We use a `String` when we want a string 
that we can modify like we would an array (pushing and popping characters and/or 
strings to and from it), and `&str` when we only need a *slice* of an existing 
string. 

We can modify `get_version()` to return a string by modifying the return value 
of the function. Recall that the return value of the function right now is a 
`u16`. How do you think we would change it to a `String`?

```rust
fn get_version() -> String {
  //...
}
```

Changing the return value to a `String` is good, but I want to take this 
function a different way. Instead of just getting our version&mdash;which would mean 
we would create separate functions for all of the other manifest values we want 
to retrieve&mdash;let's replace `get_version()` with a function that can 
retrieve more than just one value from the manifest file. 

When the `usage()` function prints the banner, I want it to print something like 
this:

```
$ tinymd
tinymd (v0.1.0), a tiny and mostly useless markdown compiler. 
Written by <Your Name>
Usage: tinymd <somefile.md>
```

You might already see the variables that we need to retrieve in order to 
produce the above output:

```
[name] (v[version]), [description]
Written by [author]
Usage: tinymd <somefile.md>
```

Additionally, whenever the tool is doing it's job, I still want part of the 
banner to be outputted. For example, if I wanted to compile a file called 
`something.md` into `something.html`, maybe the tool works like this:

```
$ tinymd something.md
tinymd (v0.1.0), a tiny and mostly useless markdown compiler.
Compiling something.md...
Done! Your new file is something.html.
```

Imagining how we want our tool to behave is a good way to think about what needs 
to be done to get there. 

Since we're always going to print the first line, let's create a separate 
function to prepare it for us.

To do this, we are going to create a `String` variable and then push the title, 
the version, and the description into it. This means that the variable needs 
to be **mutable**, and brings us to an important concept in Rust: *all variables are immutable by default*.

Rust makes all variables immutable by default as part of a memory strategy that 
guarantees no memory leaks. If you want to modify a variable, you must first 
create a mutable reference to it.
 
To illustrate how Rust treats all variables as immutable unless told otherwise, 
let's try to modify the value of `the_version` after declaring it:

```rust
fn usage() {
    let the_version = "0.1";
    the_version = "0.2";
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
    println!("The Version: {}", the_version);
}

fn main() {
    usage();
}
```

Go ahead and replace all the contents in `main.rs` with what you see above.

Try to build this project using `cargo build`, and look what the Rust compiler
tells you:

```
$ cargo build
   Compiling tinymd v0.1.0
warning: value assigned to `the_version` is never read
  --> src\main.rs:36:9
   |
36 |     let the_version = "0.1";
   |         ^^^^^^^^^^^
   |
   = note: #[warn(unused_assignments)] on by default
   = help: maybe it is overwritten before being read?

error[E0384]: cannot assign twice to immutable variable `the_version`
  --> src\main.rs:37:5
   |
36 |     let the_version = "0.1";
   |         -----------
   |         |
   |         first assignment to `the_version`
   |         help: make this binding mutable: `mut the_version`
37 |     the_version = "0.2";
   |     ^^^^^^^^^^^^^^^^^^^ cannot assign twice to immutable variable

error: aborting due to previous error

For more information about this error, try `rustc --explain E0384`.
error: Could not compile `tinymd`.

To learn more, run the command again with --verbose.
```

You are now witnessing the power of Rust's development toolchain: the integrated 
**borrow-checker**, Rust's smart compiler. My favorite part about this is how 
it will not only tell you when you're doing something wrong, it also *suggests a 
way to fix it*. 

Let's take a closer look at three parts of this output: 

* `warning: value assigned to 'the_version' is never read`. Here, Rust is 
telling us that we assigned a value to `the_version` but then never used it. 
Why would we do that? Rust is trying to show us where we can improve the quality 
of our program, as evident by its message a few lines down: 
`= help: maybe it is overwritten before being read?`

* `error[E0384]: cannot assign twice to immutable variable 'the_version'`. Rust 
is telling us that `the_version` is an immutable variable; it cannot be changed, 
so when we try to assign a new value to its instantiated value, the Rust
compiler aborts. 

* `help: make this binding mutable: 'mut the_version'`. Rust's borrow checker is
showing us how to turn `the_version` into a mutable variable. Only mutable
variables can have their values changed. Otherwise, treat them like a static.

How do you think we should fix this?

(Go to your code editor and try to fix it using the output from the `cargo build`
command as guidance. I'll give you a hint: you have to add a three-letter keyword 
somewhere...)

The answer is to add the `mut` keyword after `let` in the declaration of 
`the_version`:

```rust
fn usage() {
    let mut the_version = "0.1";
    the_version = "0.2";
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
    println!("The Version: {}", the_version);
}
```

Any variable that we want to be able to change after declaration needs to have the 
`mut` keyword, otherwise the variable will be considered immutable (i.e., unchangeable).

With this new knowledge, we can now create a `String` variable that we will use 
to build the first line of the banner.

Go ahead and delete everything in `main.rs`; we're going to start from scratch, 
creating a new way to think about our banner that includes dynamically created 
strings. 

Let's start off with the banner itself. We want to isolate the first line, which 
includes the title, version, and description, so that we can call it from both 
`usage()` and from another function which will be the meat and potatoes of the 
compiler. Let's call that function `parse_markdown_file()`. 

In our empty `main.rs` file, go ahead and create these two empty functions 
along with the same `main()` function as before:

```rust
fn parse_markdown_file() {

}

fn usage() {

}

fn main() {
  usage();
}
```

Now, just above `usage()`, let's create two more functions: one to print just 
the first line of the banner, which I'll call the *title*, and one to print both 
the title and the rest of the banner (e.g., the "written by" and "usage" strings). 
We are going to call them `print_short_banner()` and `print_long_banner()`, 
respectively:

```rust
fn parse_markdown_file() {

}

fn print_short_banner() {

}

fn print_long_banner() {

}

fn usage() {

}

fn main() {
  usage();
}
```

We're almost finished mapping out the main functions of our tool. The last one 
we need--and the one we will spend the rest of this chapter on--goes right at the 
top and will be called `get_title()`. This one will be different because it is 
going to return a `String`:

```rust
fn get_title() -> String {

}
```

Great! Let's go over all these functions so we know how they're all related.

* `parse_markdown_file()` will be called when we are passed a markdown file via 
  the command line. We will leave this empty; Chapter 4 is when we will flesh this 
  out.
* `print_short_banner()` will output the title, version, and description. We're 
  going to build this next.
* `print_long_banner()` will output the short banner plus a "written by"
  attribution and "usage" example. 

Recall that the `env!()` macro, when passed a string key (like `CARGO_PKG_NAME`), 
will retrieve the value of that key from the manifest file and return it. 

Armed with the knowledge we've acquired so far, let's dive into the `get_title()` 
function.

The first thing we'll do is create a local `String` variable to hold all the 
data we want to output. Since this will be a variable that we are going to 
modify by adding strings onto the end of it (like the version and the 
description), what keyword do we need to have when declaring it?

We need the `mut` keyword:

```rust
fn get_title() -> String {
  let mut the_title = String::from(env!("CARGO_PKG_NAME"));
```

Here's something we haven't seen before: a `String` variable can be created *from* 
another string--even if that string is a string slice. Here we are creating 
a mutable variable `the_title` from the string returned by the call to `env!()`, 
which is grabbing the string value associated with the key `CARGO_PKG_NAME`--which 
itself is associated with the manifest file's `name` key. Neat!

What is the value of `the_title` right now? 

It's `tinymd`, which is what we see when we look at the value for the `name` 
key in the manifest file. 

With the beginnings of the title string started, we can now start to imagine what 
this string looks like based on the individual chunks of strings we would need to 
add to it to make it complete:

`[TITLE] (v[VERSION]), [DESCRIPTION]`

Or, as a list of individual strings:

1. `[TITLE]`
2. ` (v`
3. `[VERSION]`
4. `), `
5. `[DESCRIPTION]`

Finally, as a list of the actual strings we will be using:

1. `env!("CARGO_PKG_NAME")`
2. ` (v`
3. `env!("CARGO_PKG_VERSION")`
4. `), `
5. `env!("CARGO_PKG_DESCRIPTION")`

So the title string itself is composed of five individual strings that need to be 
pushed onto `the_title`. 

We can push a string onto another string (i.e., *concatenate* two strings) by 
using the `.push_str()` method of a `String`. Since we already have the title 
in `the_title`, the next string we need to add is ` (v`:

```rust
fn get_title() -> String {
  let mut the_title = String::from(env!("CARGO_PKG_NAME"));
  the_title.push_str(" (v");
```

As you can see, a `String` can be instantiated by building one *from* another 
source--like, in this case, the return value of the `env!()` macro. You'll also 
note that the `String::from`` bears resemblance to method calling from 
other languages. 

To add to the end of the string, Rust gives us the `push_str()` method. At this 
point, the value of `the_title` is `tinymd (v`. 

How do you think we will build the rest of `the_title`?

Try to build the rest of `the_title` yourself, using the `.push_str()` examples 
above, and then open up the Solution below to see if you got it right.

Remember: the goal is for `the_title` to equal something like this:

`tinymd (v0.1.0), A tiny markdown compiler based on Jesse's tutorials.`

<details>
<summary>One Possible Solution</summary>

```rust
fn get_title() -> String {
  let mut the_title = String::from(env!("CARGO_PKG_NAME"));
  the_title.push_str(" (v");
  the_title.push_str(env!("CARGO_PKG_VERSION"));
  the_title.push_str("), ");
  the_title.push_str(env!("CARGO_PKG_DESCRIPTION"));
  return the_title;
}
```
</details>

We now have a function that will get the title string of our program, which is 
what we call the *short banner*. 

We are going to call `get_title()` from the `print_short_banner()` function 
by using the `println!()` macro. Can you guess how we will do that?

<details>
<summary>One Possible Solution</summary>

You can use the string substitution characters `{}` (just like how you 
might use `%s` in C's `printf`), like this:

```rust
fn print_short_banner() {
  println!("{}", get_title());
}
```
</details>

Great! Now, anytime we want to output just the short banner, we have a function 
dedicated to just that. 

Next, let's flesh out the `print_long_banner()` function. 

We know that the first thing the long banner print's is the short banner. How 
might we call the short banner function from inside `print_long_banner()`?

Exactly the way you would think to do it:

```rust
fn print_long_banner() {
  print_short_banner(); 

}
```

The rest of the long banner will contain the other elements from the manifest 
file that we haven't retrieved yet, and we will retrieve them exactly the same 
way we did so for the name, version, and description in the short banner. 

Here's what we want the long banner to look like:

```
$ tinymd
tinymd (v0.1.0), A tiny markdown compiler based on Jesse's tutorials.
Written by: <your name>
Homepage: https://jesselawson.org/rust
Usage: tinymd <somefile>.md
```

If we break this up into four separate strings, it will be easier to plan out 
how you will tackle this function:

* `print_short_banner()`
* `"Written by: "` + `env!("CARGO_PKG_AUTHORS")`
* `"Homepage: "` + `env!("CARGO_PKG_HOMEPAGE")`
* `"Usage: tinymd <somefile>.md"`

Using what you've learned so far and the `get_title()` function as a reference, 
try constructing the rest of the `print_long_banner()` function on your own. 
Here's a hint: you only need the above `env!()` examples and some corresponding 
calls to `println!()`. 

<details>
<summary>Two Possible Solutions</summary>

One way to do this is by creating separate strings for each line of the banner:

```rust
fn print_long_banner() {
  print_short_banner();
  let mut written_by = String::from("Written by: ");
  written_by.push_str(env!("CARGO_PKG_AUTHORS"));
  let mut homepage = String::from("Homepage: ");
  homepage.push_str(env!("CARGO_PKG_HOMEPAGE"));
  let mut usage = String::from("Usage: tinymd <somefile>.md");
  println!("{}", written_by);
  println!("{}", homepage);
  println!("{}", usage);
}
```

Another way is to just pull all this directly into a single call to the 
`println!()` macro:

```rust
fn print_long_banner() {
  print_short_banner();
  println!("Written by: {}\nHomepage: {}\nUsage: tinymd <somefile>.md\n", 
    env!("CARGO_PKG_AUTHORS"), 
    env!("CARGO_PKG_HOMEPAGE")
  );
}
```

</details>

Let's finish out this chapter by building our tool one last time. What command 
can we use to build the project *and* run it *quietly* all at once? 

<details>
<summary>One Solution</summary>

Recall that Cargo's `run` command will automatically trigger a `build` if it 
detects that source files have been changed since the last time the program was 
built. We can also pass the `-q` flag to keep the output quiet--that is, not 
show all the verbose output that comes with a bare `cargo build` command. 

```
$ cargo run -q
```
</details>