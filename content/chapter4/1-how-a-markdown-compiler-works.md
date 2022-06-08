---
date: "2022-06-05"
title: "How the Markdown compiler works"
slug: "how-the-markdown-compiler-works"
weight: 1
prev_relref: "_index.md"
next_relref: "2-how-to-parse-cli-args.md"
---

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