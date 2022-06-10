---
date: "2022-05-28"
title: "The structure of new Rust projects"
slug: "structure"
weight: 1
prev_relref: "chapter1/_index.md"
next_relref: "chapter1/2-chapter1-checkpoint.md"
---

Inside a newly created Rust project, you'll find three things:

* A `src` folder, where your Rust code (Rust files end in `.rs`) lives
* A `.gitignore` file, because version control thinking is built-in
* A `Cargo.toml` file, which is the manifest file. This is the project 
configuration and dependencies script. This would be like the `Gemfile` in Ruby, 
or `package.json` in Node.

Anytime we create a new project like this, Rust sticks in some default code for
us. Open up `src/main.rs` and let's see what we can intuit about Rust's syntax:

```rust
fn main() {
    println!("Hello, world!");
}
```

There's so much we can infer from this tiny block of code:
- Functions are declared with `fn`; 
- the `printf` equivalent is `println!`, which is a **macro** and not a 
function; 
- this program does nothing more than print `"Hello, world!"` to the 
command line. 

{{< expandable label="🤔 Functions vs Macros in Rust" level="4" >}}
A macro in Rust encapsulates code and presents it in a developer-friendly way. 
There are only a few that we will be using throughout these tutorials, and all 
of them are provided by Rust. 

Macros make it easy to provide batteries-included tools to help developers 
write more fluid and legible code. For now, think of macros like super 
functions.

_I personally don't like that new Rust programmers have to deal with macros
from the start, but this is just one of the ways Rust really is a unique 
programming experience._

{{< /expandable >}}

Before we get ahead of ourselves, let's build this project. Open up your 
terminal (if you're using VS Code, you can use the integrated terminal by typing
 `Ctrl`+`` ` ``), and then type `cargo build -q`: 

```
$ cargo build -q
```

The `build` command compiles your project and builds your executable. (This will be 
the last time I mention that the `-q` flag tells cargo to "be quiet" and not emit 
anything it doesn't need to). 

The executable is a "development" version. The main difference 
between a development and release version is that in a development version 
the code is not optimized and there are debug symbols (to help us track 
down issues) present that we would normally omit from a release version.

When Cargo is finished building your project, you'll have a new folder 
named `target` in your project directory. Inside that folder is where 
you'll find your executable. 

We have covered two Cargo commands so far: 

* `cargo new`, to create a new Rust project, and
* `cargo build`, to build a Rust project.

The third Cargo command we will often use is `cargo run`, which is how we 
will run the executable that our project builds. 

Let's try running our current project that we just built:

```
$ cargo run -q
Hello, world!
```

Note that we don't need to pass the name of the project; Rust will infer that
for us, by executing the appropriate `.exe` file based on the release target.

If you see "Hello, world!" in your terminal, then congratulations: you have 
built your first Rust program!


