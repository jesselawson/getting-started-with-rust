---
date: "2022-06-05"
title: "Opening & reading files"
slug: "files"
weight: 1
prev_relref: "_index.md"
next_relref: "2-parser-design.md"
---

The primary function that will contain the logic of our Markdown compiler is 
`parse_markdown_file()`, which takes a single argument: a string slice (`&str`) 
that corresponds to the Markdown file we want to create. 

Let's go ahead and create that dummy file right now. Copy all of the below text 
into a new file called `test.md`, and save it in the root of your Rust project. 
The root of your Rust project also contains the manifest file (`Cargo.toml`), so 
just make sure `test.md` and `Cargo.toml` are in the same directory. 

*If you would rather just download `test.md` directly, you can right-click and 
save a copy via [this link](https://gist.githubusercontent.com/jesselawson/007ea78b162c280c66fc96372a156895/raw/f155405be37f5924ac0d49f50d796ad038ea1735/test.md)*

```markdown
# My favorite author

This is a report about my favorite writer. His name is Jesse Lawson.

# Jesse's favorite food

Jesse really likes enchiladas and any kind of sushi. 

# Jesse's favorite drink

Jesse likes to drink coffee in the mornings and iced tea throughout the day. Sometimes, he even drinks water. 

# Jesse's favorite hobbies

Jesse likes to write about computer programming and game design, and when he is not hunched over a computer, you can find him out on a run and listening to a podcast or the serenity of mother nature. 
 
```

As you can see, one of my most endearing qualities is my humble modesty.

With `test.md` in the root of our project folder, we now have an actual file to 
open and read from.

