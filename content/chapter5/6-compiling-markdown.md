---
date: "2022-06-05"
title: "Compiling Markdown to HTML"
slug: "compiling"
weight: 6
prev_relref: "5-first-character.md"
next_relref: "7-writing-file.md"
---

Let's now write some HTML based on what the first character of the line is. 

Remember that `first_char` is a vector that only has one element. To see it, 
we will use the `pop()` method that Rust provides to vectors, which will not 
return the first character of the line, but rather, an `Option` object. 

Just like `Result`, `Option` is made up of two pieces--but they're called `Some()` 
and `None()` instead of `Result`'s `Ok()` and `Err()`. 

When you pop and element from a vector in Rust, you will either get *some* 
value or *none*. We really only care if we are getting *some* character that 
looks like `#`--or, put another way, `Some('#')`.

The `match` block has the same kind of syntax that you may recall from before, 
this time with the default case (`_`) and `Some("#")`:

```rust

match first_char.pop() {
  Some('#') => {},// The first character is #
  _ => {} // The first character is not # 
}
```

Let's work on the first-order heading matches first. 

What are the things that need to happen when we arrive at a line that starts 
with the `#` character? Don't think about the lines in sequence; think about the 
problem and try to come up with what would need to happen algorithmically, 
regardless of which line we are on. 

What do we know about the `#` character? Well, we know it corresponds to a `<h1>` 
tag--an opening tag for a first-order heading. We also know that these should 
not be nested; in no case should a `<h1>` tag be followed by a `<p>` tag, nor 
should a `<p>` tag be followed by a `<h1>` tag. So the first thing we should do 
is check whether either of these tags are open. 

Remember those boolean flag variables we created earlier? They're finally 
coming into play! 

We're going to do the `Some('#')` match block first, and then the default case, 
denoted by an underscore character (`_`). 

We'll first check if the `ptag` is set, since we don't want to start a new 
heading tag without first closing an open `<p>` tag. 

If it's set, we'll unset it (by marking it `false`), then send a closing `</p>` 
tag and a newline character to the `output_line` string--which is the string we 
are creating inside this loop iteration that will be pushed to `tokens` when we 
are done processing `line_contents`:

```rust
Some('#') => {
  if ptag {
    ptag = false;
    output_line.push_str("</p>\n");
  }

```

Other than the syntax for the if block, there's nothing new here. We are setting 
`ptag` to false and then using `.push_str()` to append a string literal onto 
the end of `output_line`.

Next, we are going to perform the same check for `htag`:

```rust
Some('#') => {
  if ptag {
    ptag = false;
    output_line.push_str("</p>\n");
  }

  if htag {
    htag = false;
    output_line.push_str("</h1>\n");
  }
```

At this point, we have accounted for the two kinds of tags that our compiler 
knows about; we checked for open paragraph and first-order heading tags, and if 
we found an open one, we closed it properly. The next thing to do is to set the 
`htag` flag to true, then push a new heading tag to `output_line`.

How do you think we will do that?

{{% deepdive title="Three Possible Solutions" %}}

One way to do this is exactly the way we did the checks above:

```rust
htag = true;
output_line.push_str("<h1>");
```

Another way is to add some newline characters, depending on how you want your 
resultant HTML file to be organized:

```rust
htag = true;
output_line.push_str("\n\n<h1>");
```

This is the part of your compiler where you can add this kind of sugar. For 
example, if you wanted all your headings to be part of a certain class, you 
could do something like this:

```rust
htag = true;
output_line.push_str("\n\n<h1 class=\"report-title\">");
```

If you're feeling confident, go ahead and make this compiler your own by 
adding in some of these fun customizations!

{{% /deepdive %}}

The easiest way is to just use the `.push_str()` method from way back in 
Chapter 3:

```rust
htag = true;
output_line.push_str("\n\n<h1>");
```

At this point, we are almost done processing this line! The last step of this 
iteration is to actually push the contents of `line_contents` minus the starting 
character `#` and the space next to it onto `output_line`. 

We want `line_contents` minus the first two characters. Here's why:

```text
# Sometext
^^
||
|+--- This space is unnecessary for <h1>Sometext</h1>
|
+--- This character converts to <h1>
```

To get all of `line_contents` except the first two characters, we can use a 
special string slice generation method that will feel very familiar to Python 
developers: `&line_contents[2..]`. The `[2..]` says *"Take a string slice of 
`line_contents` starting at the element in index 2 (so, the third character) and 
go all the way until the end of the string."*

To push this to `output_line`, we just pass it by reference to `.push_str()`:

```rust
  htag = true;
  output_line.push_str("\n\n<h1>");
  output_line.push_str(&line_contents[2..]); // Get all but the first two characters
}, // end of the Some('#') => { ... } block
```

The `Some('#')` match block is complete, but we have one more case to account for 
before closing it completely: the default case. 

For this, the only check we care about is whether there is no `ptag`. If we read 
a line that doesn't start with a `#`, then what are our priorities? Well, to start 
a paragraph tag if one isn't already open! So we'll check if `ptag` is false, 
and if it is, we'll set it to true and then push `<p>` to `output_line`. When 
we're done, we will push *all* of `line_contents` to `output_line`. 

With those parameters, see if you can finish the match block's default case by 
yourself, then open the solution below to see how you did.

{{% deepdive title="One Solution" %}}  

```rust
 _ => {
    
    if !ptag {                      // If ptag is false,
      ptag = true;                  // set it to true, then 
      output_line.push_str("<p>");  // push a <p> to the output line.
    } 

    output_line.push_str(&line_contents); // Push the whole line to the output line.
    
  }
};
```

{{% /deepdive %}}

At this point, the match block is finished! 

Here's about what it should look like so far:

```rust {linenos=true}
match first_char.pop() {
    Some('#') => {
      if ptag {
        ptag = false;
        output_line.push_str("</p>\n"); // adding \n for instructional clarity
      } 
      if htag {
        htag = false;
        output_line.push_str("</h1>\n"); // close it if we're already open
      }

      htag = true;
      output_line.push_str("<h1>");
      output_line.push_str(&line_contents[2..]); // Get all but the first two characters
    },

    _ => {
      
      if !ptag {
        ptag = true;
        output_line.push_str("<p>");
      } 

      output_line.push_str(&line_contents);
      
    }
};
```

We're almost finished with this section. Before we can push `output_line` into 
the `tokens` vector (which we will ultimately be writing to the output file), 
we have three more checks we have to do.

At this point in the program, we have completed checking whether the line is a 
heading or a paragraph, and we have both opened the appropriate tag and pulled 
in the contents of the line appropriately. The final three checks we need to do 
are to 1) check if there's still an open paragraph tag, 2) check if there's still 
an open heading tag, and 3) check if the line is empty, since we don't really 
care to write an empty set of tags to the output file. 

Right after the closing bracket of the match block, we'll do our first check: if 
the paragraph tag is open, close it and push a closing HTML tag.

```rust
  if ptag {
    ptag = false;
    output_line.push_str("</p>\n");
  }
```

Next, we'll do the same for the heading tag:

```rust    
  if htag {
    htag = false;
    output_line.push_str("</h1>\n");      
  }
```

Finally, we'll avoid pushing blank lines by making sure `output_line` is not 
equal to two empty paragraph tags:

```rust
  if output_line != "<p></p>\n" {
    tokens.push(output_line);
  }

} // end of "for line in reader.lines()" block
```

Notice that after the final check above, we are closing out the for loop. Finally!

At this point, our compiler successfully reads and parses paragraph and first-order 
headings from Markdown to HTML. In the next section, we will derive the name of 
our output file based on the name of the input file, then write the contents of 
`tokens` (which, remember, holds all of the `output_line` iterations from the 
for-loop) to our output file. 

Let's add a quick loop that will iterate over `tokens` and then 
print out the value of each element--which will give us the resultant HTML that 
will be written to the output file in the next section. 

Try to write a for-loop that iterates over `tokens` and calls `println!()` on 
each element, then check your work against the solution below.

{{% deepdive title="One Solution" %}}

```rust
for t in &tokens {
  println!("{}", t);
}
```

You can also download all the code up to this point, including the above 
for-loop that just prints `tokens` straight to the console, from [this gist link](https://gist.github.com/jesselawson/f2833e1aa02ed9320792291bbd2a1457).

{{% /deepdive %}}

You may be wondering about that open `reader` that we never closed. In other 
languages it's often necessary to *close* file pointers. Thankfully, Rust will do 
this automatically when `reader` falls out of scope. 

Let's continue on and the finishing touches on the `parse_markdown_file()` 
function&mdash;writing our results to a file. 