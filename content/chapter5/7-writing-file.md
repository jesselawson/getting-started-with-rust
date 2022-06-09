---
date: "2022-06-05"
title: "Writing to a file"
slug: "writing"
weight: 7
prev_relref: "6-compiling-markdown.md"
next_relref: "8-chapter5-checkpoint.md"
---

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

```
$ cargo run -q test.md
```

You should see something like this:

```
$ cargo run -q test.md
tinymd (v0.1.0), A tiny markdown compiler based on Jesse's tutorials.
[ INFO ] Starting parser!
[ INFO ] Parsing complete!
$
```

Now check the root of your project directory for a new file called `test.html`. 
If you open it in your editor, you should see valid HTML:

```html
<h1>Welcome</h1>
<p>Welcome to my website.</p>
<p>Here's what I like: enchiladas, coffee, and plants.</p>
```

And with that, we are *finished*! 

We have successfully built a tiny Markdown compiler in Rust!