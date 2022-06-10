---
date: "2022-06-05"
title: "Chapter 5 Checkpoint"
slug: "checkpoint"
weight: 8
prev_relref: "7-writing-file.md"
next_relref: "chapter6/_index.md"
---

In this chapter, we built a Markdown compiler! We opened a file, read it into 
memory, parsed it one line at a time, then wrote results to a file. Great job!

Try creating some Markdown files and passing them to the compiler. While you're 
doing that, ask yourself: 
- Is it working as expected? 
- What happens when you use a Markdown tag that isn't supported yet? 
- How could you implement support for a new tag? 

The only thing left to do now that our compiler has been constructed is to 
build a release version&mdash;which we are going to do in the next and final chapter of this book.

{{<checkpoint 
        chapter="chapter-5"
        check="fifth"
        quote="Perseverance is not a long race; it is many short races one after another." 
        author="Walter Elliot"
        gist="https://gist.github.com/jesselawson/b5dea9e1abdff2d80f207f9dcfd732b8">}}