---
date: "2022-05-28"
title: "What is Markdown?"
slug: "markdown"
weight: 1
prev_relref: "about/_index.md"
next_relref: "chapter1/_index.md"
---

_If you're already familiar with Markdown, feel free to [skip ahead](/chapter1)._

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
