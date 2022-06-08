---
date: "2022-06-05"
title: "Challenges"
slug: "next"
weight: 3
prev_relref: "1-release.md"
next_relref: "/_index.md"
---

I hope you've enjoyed this experience with Rust, because if you don't enjoy it at 
this point I really don't think you'll enjoy what comes after this: traits and 
lifetimes and, depending on what you're building, mutexes and `unsafe` and all 
sorts of little goodies. We've really only begun to scratch the surface of 
what you can do with Rust!

As far as our tiny Markdown compiler goes, I've added some challenges below for you 
to continue on your journey. Do note that you will have to do some self-study 
to solve these. The [Official Rust Book](https://doc.rust-lang.org/book/) is a 
great place to start.

_I'm also working on a follow-up book to this one, in which we build out the 
[Campfire compiler](https://github.com/jesselawson/campfire). If it's already out and this is still 
here, can you please let me know? Thanks!_

**Challenges**

* At the end of the `match first_char.pop()` block, we have to repeat ourselves 
  by checking whether the `ptag` or `htag` are open. How could you encapsulate 
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


**What now?**

That's it. That's the end of the book. I hope you enjoyed the journey.

_-Jesse_