---
layout: post
title:  All About Blocks Procs and Lambas
date:   2017-06-05 02:39:45 +0000
---

**Blocks**

Blocks are pieces of code that can be dropped into another method as input. They are often refered to as anonymous functions as they have no name and act as functions. They are very prevalent throughout ruby and you may have used them without even knowing what they were. 

Blocks are typically declared with either a `do..end` block or curly braces `{}`. By convention a `do..end` block is multiline while curly braces are single lined.

You can explicitly return a block, but the return keyword will return you to whatever method called the block.

A block can be an argument to a method like `#each`. Methods such as `#each` are built to accept blocks as arguments. The way this works is that the `#each` method will call a `yield` which tells the method to run the block at that specific point.

**Procs**

Blocks are part of the procedure class. A proc is a block that you can save to a variable. Procs are created with `Proc.new` which means that a Proc is an object. Unlike a Block. Another difference between a block and a proc is that a method can have multiple procs as arguments, but can only have at most one block. 

Both Blocks and procs are also known as closures. Closures in simplest terms are chunks of code that can be passed around but hold on to the variables you first gave it when you called it.

**Lambdas**
A lambda is also part of the proc class
Lambdas are created like this
```
my_lambda = -> { puts "This is a lambda" }
my_lambda.call
```

You can also use the lambda keyword instead of the arrow to define
a lambda.
Lambdas must be called with the `call` keyword

When inside of a lambda you can return wtihout returning from the enclosing method.
```
def lambda_test
  lam = lambda { return }
  lam.call
  puts "Hello world"
end

lambda_test
```
This will print out `Hello World` 
A proc would return outside the entire method and print nothing


You must always pass the correct number of arguments to a lambda not doing so will cause and argument error which is in contrast to procs which willl attempt to handle it. 

I know that this is all very confusing, but I hope this post was helpful for someone. :)

