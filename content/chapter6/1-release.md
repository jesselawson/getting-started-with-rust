---
date: "2022-06-05"
title: "Releasing & sharing our tool"
slug: "releasing"
weight: 1
prev_relref: "_index.md"
next_relref: "2-chapter6-checkpoint.md"
---

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
