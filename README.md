# Getting Started with Rust

Hi 👋 I'm Jesse, and this is a free online book for anyone who is curious about 
the Rust programming language but doesn't know where to start. My teaching 
philosophy is _"move slow and make things."_

Over this book's six chapters, we'll learn how to program in Rust while building a 
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

# Contributing

If you see something, please say something!

This book is built with [Hugo](https://gohugo.io/). You can contribute 
without installing Hugo, but if you want to test your changes locally,
you'll need to run Hugo's dev server:

```
$ hugo server
```
