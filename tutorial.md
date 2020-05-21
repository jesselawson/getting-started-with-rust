---
title: "Getting Started with Rust by Building a Tiny Markdown Compiler"
summary: Dive head-first into Rust from scratch, learning the basics by building
  a markdown compiler.
slug: rust-tutorial-markdown-compiler
author: Jesse Lawson
type: post
tutorial: true
hackernewslink: https://news.ycombinator.com/item?id=23206660
series: rust
date: '2019-10-21'
categories:
- Tutorials
- Rust
draft: false
lastmod: '2019-10-22'
slos:
  chapter-1:
    number: "1"
    objectives:
    - Create a new Rust project on the command line without errors 
    - Compile and build a simple "Hello, World" Rust project without errors
  chapter-2:
    number: "2"
    objectives:
    - Create a function without errors
    - Create an integer variable without errors
    - Print an integer variable to the command line without errors
  chapter-3:
    number: "3"
    objectives:
    - Create a string variable without errors
    - Return a string variable from a function without errors
    - Concatenate two strings without errors
    - Print a string to the command line without errors
  chapter-4:
    number: "4"
    objectives:
    - Describe how a compiler works in general
    - Create a vector without errors
    - Read and parse command-line arguments without errors
    - Implement a match block without errors
    - Pass an argument to a function without errors
  chapter-5:
    number: "5"
    objectives:
    - Open a file without errors
    - Read a file line-by-line without errors
    - Describe how a Markdown compiler works
    - Write to a file without errors
  chapter-6:
    number: "6"
    objectives:
    - Build a release version of a project in Rust
---

# Welcome

My name is Jesse, and this is an introductory Rust tutorial for developers who 
like learning by doing.

The purpose of this tutorial is to develop intuition about 
toolbuilding in Rust--specifically, to learn how to think and build in Rust. 

Our goal is to produce a very basic command line compiler that will turn a 
basic Markdown document containing headings and paragraphs into an html file.

To do this, we will start from scratch by building a simple "Hello, 
World!" executable. Then, over the course of six chapters, iterate and expand 
until finally we can compile a very simple Markdown file into valid HTML.

{{% deepdive title="Why THIS tutorial?" %}}

Rust requires a different kind of mindset than most developers are 
typically used to. A lot of the initial pains and frustrations with learning 
Rust can be mitigated by focusing on developing confidence first. That's what 
this tutorial is: a confidence builder. 

When I first picked up Rust, I could only get a few weeks into it before my 
frustration would get the best of me and I would have to abandon it for a while. 
Eventually I would go back to Rust, have a few more "a-ha" moments, then get 
frustrated again and take a break. This cycle would continue for about four more 
times until finally I sat down with a pen and paper and wrote down exactly what it 
was that kept simultaneously bothering me about and attracting me to Rust. 

What I arrived at was something that will likely not be a surprise to you: Rust 
was frustrating to learn because it required a different mental model when approaching 
programming problems. I couldn't do what I did when going from C to C++, or C++ to PHP, 
or C++ to Java, or any of the other thirty or so languages I am familiar with, which 
was to just fit whatever syntactical differences the new language had into my 
existing framework for what a language should do and how it should feel. Rust 
required, for the most part, a clean slate. 

As a tutorial author, I wanted to write an introductory tutorial on Rust to get 
developers in other languages up and running as fast as possible--but I didn't 
want to be like every other Rust tutorial and just dive into the nitty gritty 
parts of it that, although foundational, were off-putting to anyone trying to 
learn a new language as unique as Rust.

And it wasn't just me with this concern. In a survey of 6,000 Rust users, 
nearly [25%](https://www.infoworld.com/article/3324488/rust-language-is-too-hard-to-learn-and-use-says-user-survey.html) said the language 
was too hard or confusing to learn, with the two largest areas that all users said 
were very difficult being *lifetimes* and the *ownership/borrowing system*. If 
I was going to write a tutorial that would get people excited about Rust, I knew 
I would have to focus on building confidence *before* tackling those complicated 
topics. In my mind, once someone's confidence in the language was high enough, 
tackling things like lifetimes and ownership would be a natural progression in 
their study. 

{{% /deepdive %}}

Take a moment to review the learning objectives (below), then move on to the 
first chapter. 

## Learning Objectives

{{% deepdive title="All Learning Objectives" %}}

**[Chapter 1]({{<relref "#chapter-1">}})**

{{% showobjectives chapter="1" %}}

**[Chapter 2]({{<relref "#chapter-2">}})**

{{% showobjectives chapter="2" %}}

**[Chapter 3]({{<relref "#chapter-3">}})**

{{<showobjectives chapter="3">}}

**[Chapter 4]({{<relref "#chapter-4">}})**

{{<showobjectives chapter="4">}}

**[Chapter 5]({{<relref "#chapter-5">}})**

{{<showobjectives chapter="5">}}

**[Chapter 6]({{<relref "#chapter-6">}})**

{{<showobjectives chapter="6">}}

{{% /deepdive %}}

## Prerequisites

* Have Rust installed on your workstation. You can complete 
[this tutorial]({{< ref "/rust/1-install-rust-tutorial.md" >}}) if you 
don't have Rust installed yet. 

## Audience

You will excel in this tutorial if you:

* have Rust installed on your workstation,
* are familiar with the basics of Git-based version control,
* have little to no experience with Rust, and
* are confident in at least one other language.

{{% pretty-spacer %}}

# Prologue

*Optional chapter. Dive in or skip as needed.*

{{% deepdive title="Why Rust?" %}}

![](/images/tinymd2.png)

Rust's main attractor to me is that I can have all the control of a lower-level 
language like C with some of the more expressive elements and memory safety of a
 higher-level language. It's almost like a whole new way of thinking about 
 software engineering problems. In fact, the way Rust handles variable 
 assignment, passing arguments into functions, and returning values out from 
 functions, is a somewhat unique way of thinking about the tools we're building.

![](/images/tinymd3.png)

Well, just like how Common Core came in and revamped the way we teach Math, 
there are people who are very (and sometimes toxically) critical of how Rust 
forces us to think differently about things. But every language has pros and 
cons, and it's really up to you to figure out which one makes the most sense 
given what you're building and how you like to mentally model your problems. 

I recommend that you don't fall down these "Is Rust Better Than" or "Rust Sucks 
Because" rabbit holes. We're all engineers, and engineers use tools. We should 
welcome new ways to think about the problems we've been solving for years and 
years, because eventually we're going to find better ways of solving them. 

![](/images/tinymd4.png)

Form your own opinions. Tools are supposed to be fun engaging, not talking 
points for mudslinging. 

{{% /deepdive %}}

{{% deepdive title="How is this different from other introductory Rust tutorials?" %}}

My tutorial series is designed differently than anything else I've seen about 
Rust. Being an autodidact, I wrote these tutorials in a way that would have 
helped give me the foundational confidence that I never got reading traditional 
books and popular introductory tutorials online. Rust requires a different 
mindset, yes, but I don't think we as Rust teachers should be frontloading all 
of Rust's esotericities into tutorials meant to gently introduce someone into 
the language.

The official Rust book is great. The O'Reilly book is great. There are a lot of 
great tutorials, too. I personally like to learn a new language and it's 
requisite thinking paradigms by **diving in and building tools**. Rust is, after 
all, a systems language, and toolbuilding is a great way to dive into practical 
application. On top of that, having a parallel objective (building a tiny 
markdown compiler) that is easy to understand provides some foundational 
confidence in what we're learning and applying. 

It's a unique way to learn Rust, that's for sure, and it's the way I wish I 
could have learned it. 

That being said, this course *is not* meant to get you up and running with Rust 
per se; the problem that I have seen with newcomers to Rust is that there is 
just too much to learn too soon, and there's not enough payoff in the beginning 
after "Hello, world!" to justify sticking around. In my humble opinion, this is 
because most Rust tutorials aren't teaching people how fun Rust is for building 
tools, and instead focusing too much on how idiosyncratic the unique features 
of Rust are. Those tutorials are still important, but I think other tutorials--
ones that focus on building something, even if it's not that complicated--are 
important, too, and serve to compliment the still young (as of 2019) ecosystem 
of Rust tutorials online.

{{% /deepdive %}}

{{% deepdive title="What is Markdown and what does a Markdown compiler do?" %}}

Markdown is a minimalistic language for writing HTML content very quickly. It 
gives us the ability to write text-based documents that are legible and small, 
while compiling into vaid HTML. It makes drafting, updating, and publishing very 
easy, and is a common tool used among programmers in all lines of work. 

A Markdown compiler's job is to convert "markdown" content into valid HTML. 

For example, I could have the following article written in Markdown:

```markdown
# This is a title

This is a paragraph of text. How neat! In this tutorial, we will learn how to 
turn this into a paragraph block by putting `<p>` tags around it. 
```

And a markdown compiler will convert it into the following HTML:

```html
<h1>This is a title</h1>
<p>This is a paragraph of text. How neat! In this tutorial, we will learn how to 
turn this into a paragraph block by putting <code><p></code> tags around it.</p>
```

The compiler we are building in this course will convert heading tags, paragraph 
blocks, and code snippets from Markdown into valid HTML.

{{% /deepdive %}}

{{% pretty-spacer %}}

# Chapter 1

Our goal for this chapter is to develop confidence in setting up a new Rust
project.

If you haven't already [installed Rust](
    {{< ref "/rust/1-install-rust-tutorial" >}}
), you should do that now. 

{{<objectives chapter="1">}}

We will start by creating a folder to keep track of all our tutorial projects. 
In my tutorials, you will often see `C:\RustTutorials` as the root folder of all 
our projects. Go ahead and name yours whatever you want.

For this tutorial, we are creating a Markdown compiler called **TinyMD**--which 
is also the name of my rejected Netflix show about a sentient spider that goes 
to medical school. You don't have to make a new directory; we're going to do that 
with Rust's `Cargo` toolchain, the package manager and project building tool 
that comes along with the Rust compiler when you 
[installed Rust]({{< ref "/rust/1-install-rust-tutorial.md" >}}). 

We will scafford this and all projects in Rust using the `cargo` tool, passing 
the `new` argument and specifying what we want:

```shell
$ cargo new tinymd --bin
```

The command `cargo new` builds a new project, and the `--bin` flag tells Cargo 
that this project will result in an executable (called `tinymd`) instead of 
being a library. I am going to cover libraries and the package system in a 
different tutorial, but for now you can think of it like NPM, Rubygems, or any 
other package system.

Once you've ran the `cargo new` command, you will get a confirmation that the 
project was scaffolded correctly:

![](/images/tinymd1.png)

With our new Rust project started, let's go ahead and open the project's root 
folder. 

Use your favorite editor to open up the new folder named `tinymd`. 
If you're using VS Code like me, you can usually type:

`code tinymd`

and VS Code will automatically launch with the new folder:

![](/images/tinymd5.png)

Rust projects, at the bare minimum, consist of:

* A `src` folder, where your Rust code (Rust files end in `.rs`) lives
* A `.gitignore` file, because version control thinking is built-in
* A `Cargo.toml` file, which is the manifest file. This is the project 
configuration and dependencies script. This would be like the `Gemfile` in Ruby, 
or `package.json` in Node.

Anytime we create a new project like this, Rust sticks in some default code for
us. Here you can see that it's nothing more than a simple "Hello, world!" 
program. 

There are several things you can intuit about Rust's syntax by looking at the
prepackaged dummy code in the `main.rs` file:

{{<codecaption lang="rust" title="main.rs">}}
fn main() {
    println!("Hello, world!");
}
{{</codecaption>}}

As you might have guessed: this looks a lot like C; functions are declared with 
`fn`; the `printf` equivalent is `println!`, which is a **macro** and not a 
function; this program does nothing more than print `"Hello, world!"` to the 
command line. 

{{% deepdive title="Functions vs Macros in Rust" %}}

A macro in Rust encapsulates code and presents it in a developer-friendly way. 
There are only a few that we will be using throughout these tutorials, and all 
of them are delivered through Rust. Macros make it easy to provide 
batteries-included tools to help developers write more fluid and legible code, 
and you can think of them as like super functions for now.

I personally don't like that new Rust programmers have to deal with macros
from the start, but this is just one of the ways Rust really is a unique 
programming experience. 

{{% /deepdive %}}

Before we get ahead of ourselves, let's build this project. Open up your 
terminal (if you're using VS Code, you can use the integrated terminal by typing
 `Ctrl`+`` ` ``), and then type `cargo build`: 

{{<codecaption title="Terminal">}}
$ cargo build
   Compiling tinymd v0.1.0 (C:\RustTutorials\tinymd)
    Finished dev [unoptimized + debuginfo] target(s) in 1.19s
$ 
{{</codecaption>}}

You can see that the `build` command first compiles the project before 
building it. The other stuff there--`dev [unoptimized + 
debuginfo]`--is telling us that we have compiled and built this project for dev
purposes only. The code is not optimized and there are debug symbols present
that we would normally not keep in a release version. The `dev` target is the
default target for the `cargo build` command, and is equivalent to a Debug
build (in a paradigm where you have either Debug or Release builds).

The default `cargo build` is perfectly fine for learning Rust, and will be the 
way we build all our Rust projects. It takes care of dependency management 
checking and interfaces the compiler (`rustc`) for us so that we can have a
one stop shop for working with our code. 

At this point, you should see a new directory called `target` in your project's
root directory. It was created by using the `cargo build` command's default 
target, which is Debug. Here you will find your project's executables.

We have covered two Cargo commands so far: 

* `cargo new`, to create a new Rust project, and
* `cargo build`, to build a Rust project

The third Cargo command we will often use is `cargo run`, which is how we 
will run the executable that our project builds. 

Let's try running our current project:

{{<codecaption title="Terminal">}}
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target\debug\tinymd.exe`
  Hello, world!
$ 
{{</codecaption>}}

Note that we don't need to pass the name of the project; Rust will infer that
for us, by executing the appropriate `.exe` file based on the release target.

If you see "Hello, world!" in your terminal, then congratulations: you have 
built your first Rust program!

In the next section, we'll dive into the Rust language and start customizing 
our markdown compiler to make it feel like an actual program.

{{<checkpoint 
        chapter="chapter-1"
        check="first"
        quote="The best way out is always through." 
        author="Robert Frost">}}

{{<pretty-spacer>}}

# Chapter 2

Our goal for this chapter is to develop confidence about basic functions 
and variables in Rust.

{{<objectives chapter="2">}}

All of our project's source files live in the `src` directory, and for the
purposes of clarity, we're only going to use the `main.rs` file for our 
markdown compiler.

The Hello World example code provided gives us all we need to know to start
customizing our project. By the end of this chapter, executing our code without
arguments will output a banner--a short block of text that includes the 
program's name, the author, a brief description, and usage examples. 

## Creating a function in Rust

The first thing we are going to write in Rust is a function.

As you can see from the `main()` function, basic functions with no return values
or arguments are fairly simple to write. Let's go ahead and write a new function
called `usage()` right at the top of our file. Inside it, we'll use the same
code that we can see being used to write `"Hello, world!"` to output the name
and a short description of our tool:

{{<codecaption lang="rust" title="main.rs">}}
fn usage() {
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
}

fn main() {
    println!("Hello, world!");
}
{{</codecaption>}}

Go ahead and replace `<YOUR NAME>` with your name if you haven't already done
so. 

To compile and run this, you can just run `cargo run`, which, if it detects any
changes in your source files, will recompile and rebuild your project behind
the scenes:

```shell
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target\debug\tinymd.exe`
Hello, world!
$
```

If you don't really care to look at the accompanying information (`Finished dev`
and `Running` lines), you can pass the `-q` flag to Cargo, which tells it to be
quiet:

```shell
$ cargo run -q
Hello, world!
```

To save space, that's how I'll be compiling, building, and running the code as
we make changes to it--so don't panic when you see the `-q` flag.

Let's go ahead and replace the Hello World line in the main function body with
a call to `usage()`: 

{{<codecaption lang="rust" title="main.rs">}}
fn usage() {
    println!("tinymd, a markdown compiler written by <YOUR NAME>");
}

fn main() {
    usage();
}
{{</codecaption>}}

Go ahead and compile, build, and run:

```shell
$ cargo run -q
tinymd, a markdown compiler
```

Neat! But an argumentless and returnless function is about as useful as an 
inflatable dart board. To improve our `usage()` function, let's have it return
a simple value that we can write to the console window. 

## Creating a function with a return value in Rust

In this section, we're going to create a function called `get_version()` that
will return some arbitrary version number of our tool. 

We saw earlier that a function with no arguments and no return value is written 
like this:

```rust
fn get_version() {  
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

`cargo run`

You should see the following output:

```bash
$ cargo run
   Compiling tinymd v0.1.0 (C:\RustTutorials\tinymd)
    Finished dev [unoptimized + debuginfo] target(s) in 0.63s
     Running `target\debug\tinymd.exe`
tinymd, a markdown compiler written by <YOUR NAME>
Version 1000
```

The version integer (1000) is being printed, and the `println!` macro can infer
that it's an integer based on the return type specified by `get_version()`. 

We're *almost* done with this chapter. Now that we're confident with Rust's 
function syntax, let's learn how to create and assign a value to a simple 
variable.

## Creating an integer variable in Rust

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

{{<codecaption lang="rust" title="main.rs">}}
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
{{</codecaption>}}

As you can see, substituting the function with the variable in `println!` is
fairly straightforward. Additionally, look at how we declared `the_version`,
then passed the value of the function to it. Rust infers the type of variable we
want based on the return value of the function we use to assign it a value.

We can improve this by letting Rust infer `the_version`'s type:

```rust
fn usage() {
    let the_version = get_version();
    // ...
    println!("Version {}", the_version);
}
```

Neat!

{{<checkpoint 
        chapter="chapter-2"
        check="second"
        quote="Success is stumbling from failure to failure with no loss of enthusiasm." 
        author="Winston Churchill"
        gist="https://gist.github.com/jesselawson/76d2f3e279060809bb943601b4ae8c7e">}}

{{<pretty-spacer>}}

# Chapter 3

Our goal in this chapter is to develop confidence about a few of the basic string 
operations in Rust.

{{<objectives chapter="3">}}

Our tiny markdown compiler is only just being born. We've got a project setup, 
and if we run the program we are able to output a *banner*--a block of helpful 
text that usually says what the program is, who wrote it, and how to use it. We 
have wrapped up this banner in the `usage()` function. By the end of the last 
chapter, we were outputting the version of our program. 

In this chapter, we will improve the banner by pulling data from our project's 
manifest file (`Cargo.toml`) instead of hard-coding things like the version 
number into `main.rs`. To do this, we need to understand how Rust deals with 
strings and, perhaps most importantly, what makes Rust wholly different from 
many languages you might already be used to: *ownership*. 

## Creating a string variable in Rust

Most people think of strings as the content between a pair of quotes (`"`) and, 
well, nothing really else beyond that. Rust forces us to become very familiar 
with how a string is understood by a computer--as a collection of bytes that, in 
Rust's case, are guaranteed to be valid UTF-8. That being said, every language 
has a way to implement a collection of bytes in one of two ways: *heap-allocated* 
strings, which are dynamically created at runtime and during the application's 
lifecycle, and *stack-allocated* strings, which are allocated at compile-time. 
Rust is going to make us get real familiar with both. 

In Rust, there are two types of string variables: `String`, which is allocated 
on the heap, and `&str` (called a *string slice*), which may be stack or heap 
allocated depending on what it points to. Since stack-allocated variables must 
have a known size at compile time, only `String` variables retain ownership 
over their addresses in memory when they're changed. 

A `String` is a akin to a vector. It can grow, shrink, `push()`, `pop()`, 
and it is automatically freed when the variable goes out of scope. Further, a 
`String` has its own buffer in memory; it is said to be the *owner* of the 
memory where the bytes are stored.

A `&str`, on the other hand, is a *string slice*. It does not own any buffers 
in memory, but rather, it *borrows* whatever is at an address from a different 
owner. You can think of a `&str` as pointer or **borrowed reference** to a 
string owned by a different variable or the application itself. For this reason, 
string slices are immutable. 

So which one would we use, and when?

Think of a `String` as a dynamic vector of characters that we would use when we 
want to build a string dynamically. For example, in our markdown compiler, we 
will be using `String`s to hold the value of each block of HTML code; if we were 
going to write `<h1>Hello, world!</h1>`, we would `push()` the first tag, then 
the inner content, then the closing tag. Using a string slice wouldn't make 
sense here since the value of the `String` will change as we add ("push") data 
into it. 

Think of a `&str` as a window into another string, whether that string is a 
string literal (in which case the string slice would be static and 
stack-allocated) or a `String` (in which case the string slice would be 
heap-allocated).

Let's see how both of these string types can be used effectively by modifying 
our `the_version` variable to be a `&str`--and while we're at it, let's change 
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
and then has `the_version` borrow that value. The reason we say it *borrows* the 
value there is because technically it's not the *owner* of the buffer where `"0.1"` 
lives--the application is. (Anytime a string literal like `"0.1"` is created at 
compile time, it is created as a *static string slice*--it exists in the binary 
code that makes up the executable and cannot be changed.)

Since `the_version` is going to borrow the value of a string literal, how might 
we let Rust infer `the_version`'s type? 

We can rewrite the declaration to omit the type, since Rust knows `"0.1"` is a 
string literal and all string literals become static string slices:

```rust
let the_version = "0.1";
```

Here we omit the `: &str` part of the declaration because Rust will infer that, 
since `"0.1"` is a string literal, `the_version` needs to be a `&str`. 

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

`$ cargo run -q`

Remember that the `run` command will also trigger a `build` if it detects that 
the files have changed (which they have), and the `-q` flag tells cargo to just 
do it as quietly as possible.

Your output should look like this:

```shell
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
functions--except a macro has an exclamation point after it's name to denote 
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

## Returning a string variable in Rust

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

Changing the return value to a `String` is easy enough, but I want to take this 
function a different way. Instead of just getting our version--which would mean 
we would create separate functions for all of the other manifest values we want 
to retrieve--let's replace `get_version()` with a function that returns more 
than just one value from the manifest file. 

When the `usage()` function prints the banner, I want it to print something like 
this:

```shell
$ tinymd
tinymd (v0.1.0), a tiny and mostly useless markdown compiler. 
Written by <Your Name>
Usage: tinymd <somefile.md>
```

You might already see the variables that we need to retrieve in order to 
produce the above output:

```shell
[title] (v[version]), [description]
Written by [author]
Usage: tinymd <somefile.md>
```

Additionally, whenever the tool is doing it's job, I still want part of the 
banner to be outputted. For example, if I wanted to compile a file called 
`something.md` into `something.html`, maybe the tool works like this:

```shell
$ tinymd something.md
tinymd (v0.1.0), a tiny and mostly useless markdown compiler.
Compiling something.md...
Done! Your new file is something.html.
$
```

Imagining how we want our tool to behave is a good way to think about what needs 
to be done to get there. In the above example, you can see that the first line 
of the banner will be printed regardless of what we are doing. So it makes sense 
to create a separate function that can generate that for us. 

To do this, we are going to create a `String` variable and then push the title, 
the version, and then the description to it. This means that the variable needs 
to be mutable (i.e., able to be changed), and brings us to an important concept 
in Rust: *all variables are immutable by default*.

Rust makes all variables immutable by default as part of a memory strategy that 
guarantees no memory leaks. It's one of the nuances that beginners tend to get 
hung up on in Rust, but once you master it you will see just how clever this 
way of thinking about writing programs is. 
 
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

```shell
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
**borrow-checker**, Rust's smart compiler, which sees what you are doing, tells 
you what you're doing wrong, and *then suggests a way to fix it*. Though there 
is a lot of output here, I want you to focus your attention on three parts 
specifically:

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

If you're using a modern command window, the output's colors help direct our
attention to where we need to focus:

![](/images/tinymd6.png)

The red text there helps us see what the problem is. 

How do you think we should fix this?

(Go to your code editor and try to fix it using the output from the `cargo build`
command as guidance. I'll give you a hint: you have to add a three-letter keyword 
somewhere...)

The answer is, of course, to add the `mut` keyword after `let` in the declaration
of `the_version`:

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

{{<codecaption lang="rust" title="main.rs">}}
fn parse_markdown_file() {

}

fn usage() {

}

fn main() {
  usage();
}
{{</codecaption>}}

Now, just above `usage()`, let's create two more functions: one to print just 
the first line of the banner, which I'll call the *title*, and one to print both 
the title and the rest of the banner (e.g., the "written by" and "usage" strings). 
We are going to call them `print_short_banner()` and `print_long_banner()`, 
respectively:

{{<codecaption lang="rust" title="main.rs">}}
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
{{</codecaption>}}

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
will retrive the value of that key from the manifest file and return it. 

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
itself is associated with the manifest file's `title` key. Neat!

What is the value of `the_title` right now? 

It's `tinymd`, which is what we see when we look at the value for the `title` 
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
note that the `String::from`` bears resemblance to method calling calling from 
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

```shell
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

```shell
$ cargo run -q
```
</details>

With the banner complete, we are now ready to start building functionality into 
our tool. Armed with new knowledge and confidence about creating strings, adding 
characters to strings, and printing strings, we're now ready to begin parsing 
command line arguments so that we can really dig into the heart of this tool. 

{{<checkpoint 
        chapter="chapter-3"
        check="third"
        quote="It's not that I'm so smart, it's just that I stay with problems longer." 
        author="Albert Einstein"
        gist="https://gist.github.com/jesselawson/9e1579576398bdc183caf5fb16c4ad5e">}}

{{<pretty-spacer>}}

# Chapter 4

Our goal in this chapter is to develop confidence with parsing and interpreting
command-line arguments, describing how a markdown compiler works, and implementing 
control blocks to make it possible. We are also going to introduce vectors to get 
a head start on the next chapter. 

{{<objectives chapter="4">}}

## How a Markdown compiler works

Now is a good time to start thinking about how we want to interact with our 
tiny markdown compiler. At its very core, a command-line markdown compiler should 
take in the name of a markdown file and then turn it into valid HTML file. Our 
tool is going to be very *naive*: it will accept only one argument (the name 
of the markdown file), it will expect that file to end in `.md`, and it will 
only convert first-order headings (`#` for the `<h1>` tag) and paragraphs. 

By the end of this tutorial, though, you will have all the knowledge necessary 
to expand on this tool and make it less naive. I will leave that to you as a 
challenge for once you're finished. 

Until then, let's start thinking about how we want to interact with our tool, and 
some of our expectations when we use it. 

Calling our tool should be as simple as:

```shell
$ tinymd somefile.md
```

When we call the tool from the command line, we will intitiate the tool's 
*lifecycle*. An application's lifecycle is the set of steps it goes through from 
the start of execution to the end of execution. 

Our compiler's lifecycle will look something like this:

```text
Given a call to tinymd...
  When I pass a markdown file as an argument, it should:
    1. open the file
    2. parse the file line by line into a buffer
    3. export the buffer to a new html file
  When I pass anything else OR no argument at all, it should:
    1. show the banner
```

As I mentioned before, this tool isn't going to be very smart. We will expect 
only two possible outcomes from invoking it: either we will print the banner, 
because we have not passed a single argument that is a valid markdown file, or 
we will parse a valid markdown file, because we passed a valid markdown file as 
the sole argument. 

Knowing all the ways our tool will be invoked helps to guide how we will develop 
the main logic that will call the appropriate functions based on the given 
arguments (or lack thereof).

The following table illustrates all the possible ways someone could invoke the 
`tinymd` tool, and what we need to plan for in terms of execution: 

| Command           | Outcome                                    |
|:------------------|:-------------------------------------------|
| `tinymd`          | `usage()` (which calls `print_long_banner()`) |
| `tinymd abc`      | Pass `parse_markdown_file()` the file named `abc` |
| `tinymd test.md`  | Pass `parse_markdown_file()` the file named `test.md` |
| `tinymd one two`  | `usage()` (because it only accepts ONE argument) |

Essentially, any command that does not have a single valid markdown file as its 
sole argument will just call `usage()`, which outputs the banner. 

Now that we know all the ways our took can be invoked, we are ready to think 
about how we want our tool to parse a Markdown file. 

To define a tool that parses Markdown, we need to know what a Markdown file looks 
like. For the sake of this tutorial, we are only concerned with two types of 
Markdown syntax: *headings* and *paragraphs*. A heading in Markdown is denoted 
with a `#`. Paragraphs are plain text with no special characters at the start of 
the line. 

For example, let's say we have a markdown file called `favorite_writers.md` with 
the following contents:

```markdown
# My Favorite Writer

My favorite writer is Jesse!
```

Aww, thanks! 

In the table below, I have manually translated each line into its HTML equivalent: 

| Markdown                | HTML Equivalent                |
|:------------------------|:-------------------------------|
| `# My Favorite Writer`  | `<h1>My Favorite Writer</h1> ` |
| `My favorite writer is Jesse!`  | `<p>My favorite writer is Jesse!<p> ` |

This type of translation is exactly what the compiler is going to do. Neat!

Now we know how our tool will be invoked, and when it is, how we want the 
Markdown inside to be translated into HTML. Next, we'll start building the 
command-line argument parsing logic so that we can pass a Markdown file to our
tool. 

## How to parse command line arguments in Rust

The idea behind parsing command line arguments is fairly straight forward. Given 
an argument, perform some operation based on what that argument is. In the case 
of our Markdown compiler, we really only care about one argument--and we hope 
that it's the name of a Markdown file. 

The way Rust parses arguments is to collect them all up and store them in an 
iterable object. This is also how most other languages do it, too; once the 
collection happens, you will generally want to do a `for` loop or some kind of 
`switch` statement that will parse the arguments first by the number of arguments 
(since the number of arguments can trigger certain operations, if we wanted) and 
then by the value of those arguments. 

Rust has a special kind of `switch` statement called `match`, which does exactly 
what it sounds like: it *matches* values with associated logic. The way we use 
these to parse arguments is by collecting the arguments into an iterable object, 
then looping through them to match their values with some associated operations. 

Rust has a concept called *collections*, which you can probably guess are akin 
to vectors or lists. A collection in Rust is something that can be iterated; 
anything that can be looped through and acted on is generally considered a 
collection. When values (like command-line arguments) can be stuffed into a 
single object and iterated, we say that they are *collected*. 

To do this in Rust, it's actually very straightforward: we will create a variable 
to hold our arguments, which will be a vector of `String`s, pull out our 
arguments, then collect them into a single, iterable object. 

A vector in Rust is a type denoted by the keyword `Vec`, followed by `<` and `>`, 
with the variable type enclosed in the brackets. 

Since we want to create a `String` vector--where each argument given will be a 
different element of the vector--we will declare it as a type `Vec<String>`.

Let's create a variable called `args` that is a `String` vector, and assign 
it the value of our collected arguments:

```rust
// Collect all arguments in a vector
    let args: Vec<String> = std::env::args().collect();
```

You might recognize this chaining-style syntax from other languages. Rust has 
a lot of features behind the scenes that make it feel like a higher-level 
language, while at the same time requiring a closer-to-the-metal mindset. Here, 
the expression `std::env::args()` exposes an evironment variable of type 
`std::env::Args`, and then *collects* the arguments into a vector--to be used 
as the return value. 

The `.collect()` part of this takes the arguments and converts them into a 
`std::iter::Iterator<std::string::String>`--which, as you can see, is a generic 
`Iterator` type (from which a `Vec` is derived) of type `std::string::String`. If 
you guessed that `Vec<std::string::String>` and `Vec<String>` are the same thing, 
then you guessed right!

The value we give `args` is a collection of all the arguments. To get this, 
Rust gives us a standard library of tools called `std`. From `std`, we are 
given the `env`, which holds specific environment variables. To get those 
variables, we use `env`'s member function `args()` to pull them out--then 
chain the function `collect()` to it--which *collects* the arguments into an 
iterable object. 

At this point, `args` is an iterable vector of strings, where each element is 
an argument passed from the command line. If you've ever dealt with command-line 
tools before, you will know that the first element in the list is always going 
to be the name of the program (i.e., `tinymd`). Rust arrays start at zero, so 
the value at `&args[0]` is going to be `tinymd`. 

Notice how we use the reference operator `&` to grab the value at `args[0]`. 
Rust has a very unique way of treating variable assignment and ownership. To 
explain, consider the following two pseudo statements:

```text
1) left = right;
2) left = &right;
```

In most other languages, the first statement means that the value of `left` is 
identical now to the value of `right`. In other words, if we try to access 
the value of `left` or `right` later on, both will be the same value. In Rust, 
however, *the right-hand expression is **moved** to the left,*, meaning that 
only the left-hand expression contains the value now. 

If you want to keep the value in the right-hand expression while also creating 
a reference to it, you need to use the reference (`&`) operator. This seems like 
a daunting new idea at first, but the more you think about it, the more it makes 
sense. In fact, this one idea in Rust has helped me to be a better developer in 
every other language, since now I am purposefully thinking about what kind of 
mutability I want each variable I create to have. 

So if we wanted to access the second element in the `args` vector (which is the 
name of the Markdown file we are supposed to be passing), we would use `&args[1]` 
and not `args[1]`, since the former will provide a reference to the data and the 
latter will trigger a *move* of that value--which we do not want. 

Let's go ahead and collect up the arguments in our program. Go to your `main()` 
function and delete everything inside. Next, let's add our `args` declaration: 

```rust
fn main() {

    let args: Vec<String> = std::env::args().collect();

}
```

Great! Our `main()` function is now going to collect up the arguments and, in 
the next section, we'll see how it <em>match</em>es the correct number of 
arguments with the appropriate operations. If the program has two arguments, 
that means we have the program name (element 0) and the Markdown file name 
(element 1). Any other number of arguments should be rejected by the program. 
In the next section, we will build this logic out. 

## How to write a basic `match` block in Rust

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

## How to pass an argument to a function in Rust

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

```shell
$ cargo run -q
[ ERROR ] You forgot to specify the markdown file to parse!
tinymd (v0.1.0), A tiny markdown compiler based on Jesse's tutorials.
Written by: Jesse Lawson <drwho@nsa.gov>
Homepage: https://jesselawson.org/rust
Usage: tinymd <somefile>.md
```

Next, let's pass it the name of a fake file to see if we setup our logic correctly:

```shell
$ cargo run -q test.md
tinymd (v0.1.0), A tiny markdown compiler based on Jesse's tutorials.
[ INFO ] Trying to parse test.md...
```

Neat!

In this chapter, we developed confidence in our ability to describe how a Markdown 
compiler works, and learned how to instantiate a vector by reading and parsing 
command-line arguments in Rust. We also got a little familiar with `match`, and 
passing an argument to a function. In the next chapter, we are going to implement 
our Markdown compiler logic and open a file, read it line-by-line, translate it 
into HTML, and write the HTML to a new file. It's going to be fun!

{{<checkpoint 
        chapter="chapter-4"
        check="fourth"
        quote="If you can't fly, then run, if you can’t run then walk, if you can’t walk then crawl, but whatever you do you have to keep moving forward." 
        author="Martin Luther King, Jr."
        gist="https://gist.github.com/jesselawson/6fe6cc4f42c9ab127804e8dc95e8d237">}}

{{<pretty-spacer>}}

# Chapter 5

If you've followed along up until this point, you've hopefully grown more 
confident about starting, building, and iterating a Rust project. Our Markdown 
compiler is now at a point where it can accept command-line input, and reliably 
displays a banner based on the input provided. 

In this chapter, we will configure our tool to actually parse a Markdown file then 
output the valid translated HTML into a new file. We will do this by developing 
confidence in opening and parsing a file, using control statements, 
iterating over a vector, and writing to a file in Rust.

By the end of this chapter, we will have a working (and naive) Markdown compiler 
written in Rust, ready to be packaged up for release in the next chapter. 

Let's now dive in and start working on the parsing logic. 

## How to open and parse a file in Rust

The primary function that will contain the logic of our Markdown compiler is 
`parse_markdown_file()`, which takes a single argument: a string slice (`&str`) 
that corresponds to the Markdown file we want to create. 

Let's go ahead and create that dummy file right now. Copy all of the below text 
into a new file called `test.md`, and save it in the root of your Rust project. 
The root of your Rust project also contains the manifest file (`Cargo.toml`), so 
just make sure `test.md` and `Cargo.toml` are in the same directory. 

*If you would rather just download `test.md` directly, you can right-click and 
save a copy via [this link](https://gist.githubusercontent.com/jesselawson/007ea78b162c280c66fc96372a156895/raw/f155405be37f5924ac0d49f50d796ad038ea1735/test.md)*

{{<codecaption title="test.md">}}
# My favorite author

This is a report about my favorite writer. His name is Jesse Lawson.

# Jesse's favorite food

Jesse really likes enchiladas and any kind of sushi. 

# Jesse's favorite drink

Jesse likes to drink coffee in the mornings and iced tea throughout the day. Sometimes, he even drinks water. 

# Jesse's favorite hobbies

Jesse likes to write about computer programming and game design, and when he is not hunched over a computer, you can find him out on a run and listening to a podcast or the serenity of mother nature. 
 
 
{{</codecaption>}}

As you can see, one of my most endearing qualities is my humble modesty.

With `test.md` in the root of our project folder, we now have an actual file to 
open and read from.

## Making a `Path` variable in Rust

Rust goes to great lengths to make sure that your program is cross-platform. One 
of the ways it does this is by providing the `Path` module, which helps format 
strings and string slices into OS-specific path types. The official Rust docs 
recommend to use a `Path` variable instead of a string slice anytime you're 
working with filenames. This is one of several special tools from Rust that 
comes from Rust's `std` library--which, for the purposes of this tutorial, you 
can think of as like a namespace. 

In Rust, we can tell our program that we want to use `Path` objects by going to 
the top of `main.rs` and adding:

```rust
use std::path::Path;
```

Now we can create a `Path` object from the argument passed to the 
`parse_markdown_file()` function:

```rust
fn parse_markdown_file(_filename: &str) {
  print_short_banner();
  println!("[ INFO ] Starting parser!");
  
  // Create a path variable from the filename
  let input_filename = Path::new(_filename);
```

The call to `Path::new()` creates a new `Path` object for us. Now, `input_filename` 
is a `Path` that we can try to open. Again, don't use a `String` object to 
hold a filename, since the `Path` object is specificially designed to play well 
with Rust's other tools for opening, reading, and writing to files. 

With our path now an official Rust `Path` variable, let's pull in another tool 
to be able to open the file that the path points to: `File`.

Just like when we created the `Path` variable, we will first need to declare that 
we want to `use` it by adding the following to the top of `main.rs`:

```rust
use std::path::Path;
use std::fs::File;
```

Notice that `File` comes from `std::fs`, whereas `Path` comes from `std::path`. 

Back to our parsing function, after declaring the `input_filename` path variable, 
let's create a new `File` variable with it:

```rust

  // Create a path variable from the filename
  let input_filename = Path::new(_filename);

  // Try to open the file
  let file = File::open(&input_filename)
             .expect("[ ERROR ] Failed to open file!");
```

Interesting! Now we have a semantic way to open a file using the `File::open()` 
function, to which we pass a reference to `input_filename`, and then to which 
we chain the `.expect()` function. You will encounter the `.expect()` function 
a lot in your Rust development; it's used to remove the verbosity around Rust's 
`Result` type. 

In a nutshell, many functions in Rust do not just return a value, they return 
a `Result`. A `Result` in Rust has two parts: `Ok()` and `Err()`. When you call 
a function that returns a `Result` type, you generally need to check whether 
the function was successful (and thus returned an `Ok()`) or not successful (and 
thus returned an `Err()`). It's akin to exception handling in other languages, 
except here it's baked into the function itself. 

What the `.expect()` does is tell Rust to unwrap the return value and pass along 
the `Ok()`--except upon failure, in which case we output the string argument to 
except as a kind of error message. 

So when we write something like this:
```rust
let file = File::open(&input_filename).expect("Couldn't open file");
```

We're basically writing a less verbose (and, as you can see, less diagnostically 
helpful) version of this:
```rust
use std::error::Error;
// ...
let file = match File::open(&input_filename) {
  Err(err) => panic!("Couldn't open file: {}", err.description()),
  Ok(value) => value,
};
```

{{% deepdive title="Why the `panic!()` macro instead of `println!()`?" %}}

The `panic!()` macro respects the return type inferred from `File::open()`, which 
is type `std::fs::File`. If you try to use `println!()` here instead, you will 
get an error that says something like `match arms have incompatible types`. This 
is because `println!()` doesn't know what to do with a `std::fs::File` type, but 
`panic!()` doesn't care what kind of variable you are dealing with. 

As you can see, the verbose way requires a lot more of a deep dive into Rust 
than this tutorial is designed for. For now, the condensed way is good enough 
to build confidence in using Rust; once confidence is there, you will have 
plenty of room to confuse yourself with Rust's esoteric way of doing things--
and ultimately become a stronger developer in all languages because of how Rust 
forces you to think!

{{% /deepdive %}}

There are a lot of new things in the verbose example above, but since you have 
some experience interpreting `match` blocks in Rust already, try to see what's 
going on. Also notice how we can assign a value to a variable based on the 
result of a match block. Neat!

For this tutorial, though, we'll be using the condensed way. You will have plenty 
of time in your future Rust development journeys to unpack and deep dive into 
the `Result` object. 

At this point in our program, `file` is our way of accessing the file passed into 
the function. With the file open successfully, we can start planning how we will 
read and parse it. 

While this is not a course on compiler design, we need to think about how a 
compiler looks at a file. When the program gets to the first character in the 
file, which is a `#` (indicating a heading tag), it needs to know that the rest 
of the line will contain all the text within a heading tag. 

To illustrate, here is the first line of our `test.md`, annotated:

```text
# My favorite author 
^ ^________________^ ^
|         |          |
|         |          +--- end of line; output </h1>
|         |
|         +--- put this inside the <h1></h1> tags
|
+--- start of heading; output <h1>
```

To keep track of what stage the parser is at, we create two kinds of state 
variables called *flags*. The first flag is whether a header tag is open. If a 
header tag is open, that means our HTML file would have a `<h1>` but no 
corresponding `</h1>` yet. 

The second flag we will keep track of is whether a paragraph tag is open. 
Remember: for the purposes of this tutorial, our Markdown compiler can only 
handle first-level headings (`#`) and paragraphs. 

Let's create two mutable boolean variables to keep track of these states:

```rust
let mut _ptag: bool = false; // keep track of paragraph tags
let mut _htag: bool = false; // keep track of h1 tags
```

Notice that I'm using an underscore (`_`) here to precede their names. In Rust, 
if you instantiate a variable like this but never use the value you use to 
instantiate it, you will get a warning about *unused variable assignments*. Rust 
gets deeply concerned with the fact that you have set a value to a variable that 
is essentially garbage (since the falseness of these flags is just their default 
value going into the for-loop). You can safely remove the underscore, but 
every time you run `cargo build` (or `cargo run` which triggers a build), you 
will get a harmless warning about an unused variable assignment. 

With these two flags we can track what kind of tags we are currently reading for, 
as well as what kind of tag needs to be closed before moving on to the next 
line in the file.

Speaking of lines, we need a way to store the resultant HTML dynamically, so that 
we can add to it before writing it all to a file. What kind of variable type do 
you think we could use for this? 

The way we will do it here is through a vector of strings. I'll call this vector 
`tokens`, to help me remember what's in here. In compiler design, a *token* is 
a keyword, operator, separator, or string literal (or some other kind of lowest 
form of understandable syntax) that the compiler understands. Our vector elements 
are each going to be a `String`, and each string element will be made up of 
either a heading or a paragraph in HTML.

Let's instantiate our `tokens` vector:

```rust
let mut tokens: Vec<String> = Vec::new();
```

We make it mutable because we want to add to it; we use `Vec::new()` to create an 
empty vector object. 

Now we are ready to read the file line by line. Once again, we're going to call 
upon one of Rust's tools for doing just that: `BufReader`, which is, as you can 
derive from the name, a *buffered file reader*. Reading files from the filesystem 
can be a daunting task for programs, especially if these files are gigantic. To 
ensure that a 1gb Markdown file will be read about as fast as a 1mb Markdown 
file, we buffer the input so it reads the file in chunks. Don't worry: all the 
heavy lifting happens behind the scenes. 

Let's go to the top of `main.rs` and add *two* new entries to the block of `use` 
statements:

```rust
use std::io::{BufRead, BufReader};
```

Note that we can combine calls from the same namespace by adding them to a 
comma-separated list between curly brackets. Neat! 

Both `BufReader` and `BufRead` are necessary to do what we want to do. The first 
one lets us buffer a file into memory; the second lets us read those buffers 
line by line.

Back where we left off in the `parse_markdown_file()` function, let's create a 
variable named `reader` that will open our file:

```rust
let reader = BufReader::new(file);
```

There! Not so bad, right? The variable `reader` is now essentially our window 
into a memory-optimized lens through which we can read the file. 

At this point, your `parse_markdown_file()` function should look something like 
this:

```rust
fn parse_markdown_file(_filename: &str) { 
  print_short_banner();
  println!("[ INFO ] Starting parser!");
  
  // Create a path variable from the filename
  let input_filename = Path::new(_filename);

  // Try to open the file
  let file = File::open(&input_filename)
             .expect("[ ERROR ] Failed to open file!");

  let mut _ptag: bool = false;
  let mut _htag: bool = false;

  // Create a place to store all our tokens
  let mut tokens: Vec<String> = Vec::new();

  // Read the file line-by-line
  let reader = BufReader::new(file);

  // ...
```

With the file now open, we can iterate through it one line at a time and start 
translating Markdown to valid HTML. 

## How to read a file line-by-line in Rust

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

## Getting the first character of a line in Rust

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

Let's now write some HTML based on what the first character of the line is. 

Remember that `first_char` is a vector that only has one element. To see it, 
we will use the `pop()` method that Rust provides to vectors, which will not 
return the first character of the line, but rather, an `Option` object. 

Just like `Result`, `Option` is made up of two pieces--but they're called `Some()` 
and `None()` instead of `Result`'s `Ok()` and `Err()`. 

When you pop and element from a vector in Rust, you will either get *some* 
value or *none*. We really only care if we are getting *some* character that 
looks like `#`--or, put another way, `Some('#')`.

The `match` block has the same kind of syntax that you may recall from before, 
this time with the default case (`_`) and `Some("#")`:

```rust

match first_char.pop() {
  Some('#') => {},// The first character is #
  _ => {} // The first character is not # 
}
```

Let's work on the first-order heading matches first. 

What are the things that need to happen when we arrive at a line that starts 
with the `#` character? Don't think about the lines in sequence; think about the 
problem and try to come up with what would need to happen algorithmically, 
regardless of which line we are on. 

What do we know about the `#` character? Well, we know it corresponds to a `<h1>` 
tag--an opening tag for a first-order heading. We also know that these should 
not be nested; in no case should a `<h1>` tag be followed by a `<p>` tag, nor 
should a `<p>` tag be followed by a `<h1>` tag. So the first thing we should do 
is check whether either of these tags are open. 

Remember those boolean flag variables we created earlier? They're finally 
coming into play! 

We're going to do the `Some('#')` match block first, and then the default case, 
denoted by an underscore character (`_`). 

We'll first check if the `_ptag` is set, since we don't want to start a new 
heading tag without first closing an open `<p>` tag. 

If it's set, we'll unset it (by marking it `false`), then send a closing `</p>` 
tag and a newline character to the `output_line` string--which is the string we 
are creating inside this loop iteration that will be pushed to `tokens` when we 
are done processing `line_contents`:

```rust
Some('#') => {
  if _ptag {
    _ptag = false;
    output_line.push_str("</p>\n");
  }

```

Other than the syntax for the if block, there's nothing new here. We are setting 
`_ptag` to false and then using `.push_str()` to append a string literal onto 
the end of `output_line`.

Next, we are going to perform the same check for `_htag`:

```rust
Some('#') => {
  if _ptag {
    _ptag = false;
    output_line.push_str("</p>\n");
  }

  if _htag {
    _htag = false;
    output_line.push_str("</h1>\n");
  }
```

At this point, we have accounted for the two kinds of tags that our compiler 
knows about; we checked for open paragraph and first-order heading tags, and if 
we found an open one, we closed it properly. The next thing to do is to set the 
`_htag` flag to true, then push a new heading tag to `output_line`.

How do you think we will do that?

{{% deepdive title="Three Possible Solutions" %}}

One way to do this is exactly the way we did the checks above:

```rust
_htag = true;
output_line.push_str("<h1>");
```

Another way is to add some newline characters, depending on how you want your 
resultant HTML file to be organized:

```rust
_htag = true;
output_line.push_str("\n\n<h1>");
```

This is the part of your compiler where you can add this kind of sugar. For 
example, if you wanted all your headings to be part of a certain class, you 
could do something like this:

```rust
_htag = true;
output_line.push_str("\n\n<h1 class=\"report-title\">");
```

If you're feeling confident, go ahead and make this compiler your own by 
adding in some of these fun customizations!

{{% /deepdive %}}

The easiest way is to just use the `.push_str()` method from way back in 
Chapter 3:

```rust
_htag = true;
output_line.push_str("\n\n<h1>");
```

At this point, we are almost done processing this line! The last step of this 
iteration is to actually push the contents of `line_contents` minus the starting 
character `#` and the space next to it onto `output_line`. 

We want `line_contents` minus the first two characters. Here's why:

```text
# Sometext
^^
||
|+--- This space is unnecessary for <h1>Sometext</h1>
|
+--- This character converts to <h1>
```

To get all of `line_contents` except the first two characters, we can use a 
special string slice generation method that will feel very familiar to Python 
developers: `&line_contents[2..]`. The `[2..]` says *"Take a string slice of 
`line_contents` starting at the element in index 2 (so, the third character) and 
go all the way until the end of the string."*

To push this to `output_line`, we just pass it by reference to `.push_str()`:

```rust
  _htag = true;
  output_line.push_str("\n\n<h1>");
  output_line.push_str(&line_contents[2..]); // Get all but the first two characters
}, // end of the Some('#') => { ... } block
```

The `Some('#')` match block is complete, but we have one more case to account for 
before closing it completely: the default case. 

For this, the only check we care about is whether there is no `_ptag`. If we read 
a line that doesn't start with a `#`, then what are our priorities? Well, to start 
a paragraph tag if one isn't already open! So we'll check if `_ptag` is false, 
and if it is, we'll set it to true and then push `<p>` to `output_line`. When 
we're done, we will push *all* of `line_contents` to `output_line`. 

With those parameters, see if you can finish the match block's default case by 
yourself, then open the solution below to see how you did.

{{% deepdive title="One Solution" %}}  

```rust
 _ => {
    
    if !_ptag {                      // If _ptag is false,
      _ptag = true;                  // set it to true, then 
      output_line.push_str("<p>");  // push a <p> to the output line.
    } 

    output_line.push_str(&line_contents); // Push the whole line to the output line.
    
  }
};
```

{{% /deepdive %}}

At this point, the match block is finished! 

Here's about what it should look like so far:

```rust {linenos=true}
match first_char.pop() {
    Some('#') => {
      if _ptag {
        _ptag = false;
        output_line.push_str("</p>\n"); // adding \n for instructional clarity
      } 
      if _htag {
        _htag = false;
        output_line.push_str("</h1>\n"); // close it if we're already open
      }

      _htag = true;
      output_line.push_str("<h1>");
      output_line.push_str(&line_contents[2..]); // Get all but the first two characters
    },

    _ => {
      
      if !_ptag {
        _ptag = true;
        output_line.push_str("<p>");
      } 

      output_line.push_str(&line_contents);
      
    }
};
```

We're almost finished with this section. Before we can push `output_line` into 
the `tokens` vector (which we will ultimately be writing to the output file), 
we have three more checks we have to do.

At this point in the program, we have completed checking whether the line is a 
heading or a paragraph, and we have both opened the appropriate tag and pulled 
in the contents of the line appropriately. The final three checks we need to do 
are to 1) check if there's still an open paragraph tag, 2) check if there's still 
an open heading tag, and 3) check if the line is empty, since we don't really 
care to write an empty set of tags to the output file. 

Right after the closing bracket of the match block, we'll do our first check: if 
the paragraph tag is open, close it and push a closing HTML tag.

```rust
  if _ptag {
    _ptag = false;
    output_line.push_str("</p>\n");
  }
```

Next, we'll do the same for the heading tag:

```rust    
  if _htag {
    _htag = false;
    output_line.push_str("</h1>\n");      
  }
```

Finally, we'll avoid pushing blank lines by making sure `output_line` is not 
equal to two empty paragraph tags:

```rust
  if output_line != "<p></p>\n" {
    tokens.push(output_line);
  }

} // end of "for line in reader.lines()" block
```

Notice that after the final check above, we are closing out the for loop. Finally!

At this point, our compiler successfully reads and parses paragraph and first-order 
headings from Markdown to HTML. In the next section, we will derive the name of 
our output file based on the name of the input file, then write the contents of 
`tokens` (which, remember, holds all of the `output_line` iterations from the 
for-loop) to our output file. 

Let's add a quick loop that will iterate over `tokens` and then 
print out the value of each element--which will give us the resultant HTML that 
will be written to the output file in the next section. 

Try to write a for-loop that iterates over `tokens` and calls `println!()` on 
each element, then check your work against the solution below.

{{% deepdive title="One Solution" %}}

```rust
for t in &tokens {
  println!("{}", t);
}
```

You can also download all the code up to this point, including the above 
for-loop that just prints `tokens` straight to the console, from [this gist link](https://gist.github.com/jesselawson/f2833e1aa02ed9320792291bbd2a1457).

{{% /deepdive %}}

You may be wondering about that open `reader` that we never closed. In other 
languages it's often necessary to *close* file pointers. However, Rust will do 
this automagically when `reader` falls out of scope. Thanks, Rust!

We're now ready to put the finishing touches on the `parse_markdown_file()` 
function--writing our results to a file. 

## How to write to a file in Rust

Remember when we had to include those Rust libraries when we wanted to open and 
read a file into a buffer? We will have to do the same thing for creating and 
writing to a file. Fortunately, it's only one addition.

At the top of `main.rs`, add a `use` block for the `Write` library:

```rust
use std::io::Write;
```

Now let's head back to the bottom of the `parse_markdown_file()` function, and 
think about all we need to do. 

By this point, the `tokens` vector has a bunch of string elements that need to 
be written to a file. Before we can do that, we need to know *which* file we 
are going to be writing to. Most tools have a way of specifying the output file, 
but since ours is a naive one, we will just have it automatically derive the 
output file name from whatever we passed as the input file. 

Recall that the argument variable for the filename is `_filename`, and since we 
are passing it a file called *test.md*, then the value for `_filename` is also 
`test.md`. 

Let's have our output file be the same name as the input file, minus the last 
three characters ("`.md`") and plus five new ones ("`.html`"). Our first task 
will be to get the name of the file without the extension. Again, we'll be 
assuming that the only kind of file being passed is a `*.md`. I'll leave it to 
you as a future challenge to accept different filetypes (like `*.markdown`). 

Back in Chapter 4, we learned that we can access specific parts of a string slice 
by using brackets. For example, if we wanted all but the first three 
characters of a string slice called `example`, we could get them like this:

`&example[3..]`

Likewise, if we wanted to get all *but* the last three characters, you might think 
we could do it like this--`&example[..-3]`--but this is *incorrect*. In Rust, 
the bracket notation for string slices must be an unsigned integer that is equal to 
or less than the length of the string slice. 

If we think about our intent with this code (`[..-3]`--which, again, is NOT valid 
Rust syntax), we are basically asking for the entire length of the slice minus 
the last three characters. Since Rust cannot infer that this is what we want, 
we can explicitly tell it how many characters to use by passing it the length 
of the string slice first.

The length of `example` would be `example.len()`, and to get all but the last 
three characters, we would put the call to `.len()` right in the 
brackets: `&example[..example.len()-3]`.

Substituting out `example` for `_filename`, how do you think we might create 
a mutable String variable called `output_filename` from a reference to 
`_filename` containing all of `_filename` except the length of `_filename` minus 
three characters?

Try it first, then check your work against the solution below. 

{{% deepdive title="One Solution" %}}

We want a mutable String variable from `_filename`, so already we know we are 
going to use `String::from()`. To get all but the last three characters of 
`_filename`, we need to pass the length of `_filename` minus three into the 
brackets of the first call to `_filename`:

```rust
  // Create an output file based on the input file, minus ".md"
  let mut output_filename = String::from(&_filename[.._filename.len()-3]);
```

{{% /deepdive %}}

At this point, if we passed in `test.md` as the filename argument, the value 
of `output_filename` would be `test`. What are we missing? 

The `.html`!

Do you remember how to push a string onto the end of a String object?
  
```rust
output_filename.push_str(".html");
```

With the name of our file ready, we now need to create the actual file. To do 
this, we will use `File::create()`, which returns a Result object. Instead of 
unpacking the Result object, though, we're going to continue to use 
`.expect()`:

```rust
let mut outfile = File::create(output_filename)
  .expect("[ ERROR ] Could not create output file!");
```

By this point, I hope you're comfortable reading the above code. We're creating 
a mutable variable `outfile` equal to the result of `File::create()`, into which 
we pass the `output_filename`. The call to `.expect()` will trigger only if 
there was an error creating the file. 

With the file created, we are FINALLY ready to loop through `tokens` and write 
each element to the output file. Assuming a successfully created `outfile`, 
we now have access to a byte-writer called `.write_all()`. The way it works is 
like this: for each line in `tokens`, write each line as a byte sequence to 
the outfile. 

Here it is in code:

```rust
for line in &tokens {
  outfile.write_all(line.as_bytes())
    .expect("[ ERROR ] Could not write to output file!");
}
```

So `outfile.write_all()` takes a string as bytes (`line.as_bytes()`) and 
stuffs it into the output file. Neat!

Remember that we borrow a reference to the `tokens` vector (like this: `&tokens`) 
because of Rust's ownership rules. If we didn't include that `&`, the value of 
each element in `tokens` would be moved into the for-loop and removed from 
outside of it--and we don't want that!

Remember, too, that `outfile` will automatically be closed once it falls out of 
scope--which will be the at end of the `parse_markdown_file()` function. 

Now that we have finished writing the `tokens` vector to the output file, let's 
add some helpful output to let the user know that the parsing is finished: 

```rust
println!("[ INFO ] Parsing complete!");
```

We can now put the closing bracket on our parsing function and test our compiler. 
You should already have a file called 
[test.md](https://gist.githubusercontent.com/jesselawson/007ea78b162c280c66fc96372a156895/raw/f155405be37f5924ac0d49f50d796ad038ea1735/test.md) in the root of your
project (the same directory as the manifest file). We can trigger a build 
and a quiet run of the tool by running:

```bash
$ cargo run -q test.md
```

You should see something like this:

```bash
$ cargo run -q test.md
tinymd (v0.1.0), A tiny markdown compiler based on Jesse's tutorials.
[ INFO ] Starting parser!
[ INFO ] Parsing complete!
$
```

Now check the root of your project directory for a new file called `test.html`. 
If you open it in your editor, you should see valid HTML:

```text
<h1>My favorite author</h1>
<p>This is a report about my favorite writer. His name is Jesse Lawson.</p>
<h1>Jesse's favorite food</h1>
<p>Jesse really likes enchiladas and any kind of sushi.</p>
<h1>Jesse's favorite drink</h1>
<p>Jesse likes to drink coffee in the mornings and iced tea throughout the day. Sometimes, he even drinks water.</p>
<h1>Jesse's favorite hobbies</h1>
<p>Jesse likes to write about computer programming and game design, and when he is not hunched over a computer, you can find him out on a run and listening to a podcast or the serenity of mother nature.</p>
```

And with that, we are *finished*! 

We have successfully built a tiny Markdown compiler in Rust!

![](/images/wedidit.gif)

In this chapter, we fleshed out the meat and potatoes (or tofu and beans) of our 
Markdown compiler. We developed confidence in opening a file, reading and 
parsing that file one line at a time, then writing to a file. We also took a 
deep dive into the basics of logical thinking for a very simple compiler. Now 
that the tool is finished, it's time to use Cargo to build a release version 
of tinymd--which we are going to do in the next and final chapter of this 
tutorial. 

{{<checkpoint 
        chapter="chapter-5"
        check="fifth"
        quote="Perseverance is not a long race; it is many short races one after another." 
        author="Walter Elliot"
        gist="https://gist.github.com/jesselawson/b5dea9e1abdff2d80f207f9dcfd732b8">}}

{{<pretty-spacer>}}

# Chapter 6

In the final chapter of this tutorial, we will build a *release* version of 
our project. Don't worry: this chapter is incredibly short!

## Building a release version of a project in Rust

Up until now, every time we have built our project using `cargo build` or 
`cargo run` to trigger a build, our executable (`tinymd.exe`) has been built 
in the "debug" folder that is located inside the "target" folder in the 
project root:

```text
C:\RustTutorials\
  tinymd\
    \target
      \debug
        tinymd.exe
```

When you run `cargo build` with the `--release` flag, Rust will do a special 
optimization build of your project, and store it in a "release" folder:

```text
C:\RustTutorials\
  tinymd\
    \target
      \debug
        tinymd.exe <- the development version of the compiler
      \release
        tinymd.exe <- the release version of the compiler
```

Let's go ahead and build it now:

```rust
$ cargo build --release
   Compiling tinymd v0.1.0 (C:\RustTutorials\tinymd)
    Finished release [optimized] target(s) in 0.76s
```

Pretty easy!

The development version has debug symbols that Cargo uses to help you during 
development, which is one reason that the debug version is always going to be 
slower than the release version. In the release version, Cargo removes all that 
and also performs some code optimizations. Building a release version takes a 
little bit more time, but the end result is a faster and smaller executable.

You can now take the executable from the release directory and put it in any 
folder you want, and as long as you pass it a path with a valid Markdown file 
that ends in `*.md` (that only has first-order headings and paragraphs), 
it will produce a valid HTML file!

After you run `cargo build --release`, any code changes you make will only 
appear in the `debug` version after running `cargo build` or `cargo run`. If you 
want your changes to appear in a new release version, run `cargo build --release` 
again. 

And with that, I think we'll call this tutorial complete.

If you made it this far, I want you to know that I am very proud of all the work 
you have done here. I have spent about four weeks off and on writing this 
tutorial with you in mind. Whoever you are, I hope that this tutorial has 
helped you gain confidence with Rust so that you are well-equipped mentally and 
emotionally to start tackling some of the more difficult aspects of this 
really fun and unique language. 

I would *love* to know how you felt about this tutorial. At the bottom of this 
page, you will find several ways to share your thoughts with me. Thank you so 
much!

## Where to go from here

This tutorial was *not* an exhaustive introduction to Rust--but I do hope that 
you have more confidence with Rust to tackle greater challenges. 

As far as our tiny Markdown compiler goes, I've added some challenges below for you 
to continue on your journey. Do note that you will have to do some self-study 
to solve these. The [Official Rust Book](https://doc.rust-lang.org/book/) is a 
great place to start. 

**Challenges**

* At the end of the `match first_char.pop()` block, we have to repeat ourselves 
  by checking whether the `_ptag` or `_htag` are open. How could you encapsulate 
  this logic in a function to better adhere to the [DRY principle](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)?

* The way we are opening and reading the input file is not the same as what you 
  find in the Rust docs for `read_lines` [here](https://doc.rust-lang.org/rust-by-example/std_misc/file/read_lines.html). How (and why) is the linked Rust code different? 

* How could you add in some other HTML tags at the top of your output file, like 
  a `<style>` tag or even a link to a CSS file *before* you iterate over each 
  line from the input file? 

* How would you add support for more than one character as the flag? For example, 
  how could you parse `## This is a second-order heading`? (Hint: the `first_char` 
  is only the first character of `line_contents` because we used `.take(1)`...)

* Try to add support for `<em>` (asterisks) and `<strong>` (double asterisks)

* When it comes to error checking, Rust has evolved over time to be less 
  verbose. Try out some of the ways Rust lets you [skip the verbosity in 
  error-handing](https://thefeedbackloop.xyz/stroustrups-rule-and-layering-over-time/) 
  by updating parts of the code we wrote to manually unwrap Result objects.

* This tutorial hasn't even touched on one of the coolest parts of Cargo--integrated 
  testing built right into the ecosystem! Challenge yourself to use some TDD 
  practices on your next tinker project in Rust.

## Sending feedback/comments

Feel free to reach out in whatever way works for you:

* <strong>Clone the repo!</strong> Feel free to [clone this repo](https://github.com/jesselawson/rust-tiny-markdown-compiler-tutorial), make some changes, and submit a PR. 
* <strong>Send me an email!</strong> {{<myemail>}} (or [this](https://forms.gle/VbDtvbep1z276Ei68) Google form if you don't want me to know your email)
* If you're feeling generous, <strong>[☕ buy me a coffee!](https://www.buymeacoffee.com/jesselawson)</strong>

## Special thanks

Many people have reached out with helpful feedback to improve this tutorial. While 
not an exhaustive list, these are some of the folks from our community who went 
above and beyond to help with fine-tuning:

* [Jordan Terrell](http://www.jordanterrell.com/)
* [Michael Leonard](http://mikelnrd.com/)
* [Nick Radcliffe](http://stochasticsolutions.com)
