---
date: "2022-05-28"
title: "About this book"
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
prev_relref: "_index.md"
next_relref: "about/1-what-is-markdown.md"
---

Hi 👋 I'm Jesse, and this is a free online book for anyone who is curious about 
the Rust programming language but doesn't know where to start. My teaching 
philosophy is _"move slow and make things."_

Over the next six chapters, we will learn about Rust while building a 
rudimentary Markdown compiler&mdash;a command-line interface tool that takes 
a Markdown file as input, converts it to HTML, and writes that HTML to another 
file.

For example:

```
$ echo "# Hello, world!" > some.md
$ tinymd some.md
Compile Markdown...
Done!
$ cat some.html
<h1>Hello, world!</h1>
$ 
```

If you'd like to see what we'll be building, check out [the repo on Github](https://github.com/jesselawson/tinymd).

{{< expandable label="Learning Objectives" level="4" >}}

**[Chapter 1]({{< ref "chapter1" >}})**

{{% showobjectives chapter="1" %}}

**[Chapter 2]({{< ref "chapter2" >}})**

{{% showobjectives chapter="2" %}}

**[Chapter 3]({{< ref "chapter3" >}})**

{{<showobjectives chapter="3">}}

**[Chapter 4]({{< ref "chapter4" >}})**

{{<showobjectives chapter="4">}}

**[Chapter 5]({{< ref "chapter5" >}})**

{{<showobjectives chapter="5">}}

**[Chapter 6]({{< ref "chapter6" >}})**

{{<showobjectives chapter="6">}}

{{< /expandable >}}

## How this book is written

Throughout this book you'll find **code snippets** and **command snippets**. 

Code snippets are blocks of code that you should be writing in your code editor: 

```javascript
// @todo worry
console.log("Don't worry--this is the only Javascript in the book.");
```

Command snippets are commands you execute in your terminal:

```
$ echo "Hello, world!"
Hello, world!
```

You may also find _expandable_ pieces of information;

{{< expandable label="They look like this! Click on me..." level="4" >}}
These just hold extra information that's relevant but not necessarily pertinent 
for the purposes of this book. 
{{< /expandable >}}

## Prerequisites

This book assumes that you

* have Rust installed,
* are familiar with the command line,
* have little to no experience with Rust, and
* have some experience with at least one other programming language.

If you don't have Rust installed yet, head to [rustup.rs](https://rustup.rs/#) 
and follow the instructions. 

{{< expandable label="🤍 Motivations for this book" level="4" >}}

There are so many ways to start learning Rust that it can be overwhelming. It was 
for me. That's why I wrote this book: to give people a real-life project to 
work on while learning the basics of Rust. Moving slow and building things is 
my favorite way to teach and learn, and I hope this book helps you see the joy 
of Rust just like I did.

When I first started learning Rust, I could only get a few weeks into it before my 
frustration would get the best of me and I would have to abandon it for a while. 
Eventually I would go back, have a few more "a-ha" moments, then get 
frustrated again and take a break. This cycle would continue for about four more 
times until finally I sat down with a pen and paper and wrote down exactly what it 
was that kept simultaneously bothering me about and attracting me to Rust. 

What I arrived at was something that will likely not be a surprise to you: Rust 
was frustrating to learn because it required a different mental model when approaching 
programming problems. I couldn't do what I did when going from C to C++, or C++ to PHP, 
or C++ to Java, or any of the other thirty or so languages I am familiar with, which 
was to just fit whatever syntactical differences the new language had into my 
existing framework for what a language should do and how it should feel. 

Rust requires, for the most part, a fairly new way of thinking&mdash;especially 
if you come from languages that do a lot of things for you behind the scenes.

So I took a different approach to an introductory Rust experience. There's only 
the basics here; variables and functions, reading and writing files, and 
some simple logical processing. We don't cover any advanced features on purpose; 
Rust can be a productive and dare I say _enjoyable_ experience depending on what 
you're building and why. 

In a survey of 6,000 Rust users, nearly [25%](https://www.infoworld.com/article/3324488/rust-language-is-too-hard-to-learn-and-use-says-user-survey.html) said the language 
was too hard or confusing to learn, with the two largest areas that all users said 
were very difficult being *lifetimes* and the *ownership/borrowing system*. We 
do cover ownership and borrowing a bit since it's so germane to Rust, but we don't 
cover lifetimes at all. 

{{< /expandable >}}
