---
date: "2022-06-05"
title: "Parsing command-line arguments"
slug: "parsing-command-line-args"
weight: 2
prev_relref: "1-how-a-markdown-compiler-works.md"
next_relref: "3-match-blocks.md"
---

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