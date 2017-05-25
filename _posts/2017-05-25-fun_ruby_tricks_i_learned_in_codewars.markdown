---
layout: post
title:  Fun Ruby tricks I learned in codewars
date:   2017-05-25 16:18:59 -0400
---

 
I have been playing around with codewars a lot lately. I like working with code challenges they are pretty fun. One thing that is great about playing with codewars is that I got to use a lot of tools and methods I would have never else thought of for example


## delete

This comes in handy for challenges that ask you to remove a series of elements from a string.
For example, if you need to remove some vowels from a string
```
 my_string = "This is such a wonderful method"
 my_string.delete("aeiou") 
 
 => Ths s sch  wndrfl mthd
 ```
 
 This is much easier than using gsub!


## to_s(base)

So most everyone knows how to_s works, you are converting a non string object to a string representation of that object. But one thing a lot of people forget is that you can pass in an argument to to_s when its placed on a Fixnum. This argument represents a numerical base which by default is 10. Therefore, this method can be used to convert numbers to different bases
```
  35.to_s(2)
  
  => "100011"
```

## partition

This is a great way to split up some data. It accepts a block and creates 2 arrays. For the elements that are true, they are first array, while the rest are placed in the second array.  Imagine we have a problem where we need to seperate some evens and odds

```
  my_array = [1,2,3,4,5,6]
	my_array.partition{|i| i.even?}
	
	=> [[2, 4, 6], [1, 3, 5]] 
```


# even? odd?
Which reminds me, I have a tendency to forget about these ruby methods. They work just as you expect. The question mark signify's this method returns some sort of boolean.

```
  4.even?
  => true
	
	4.odd?
	=> false
```

Its a shame how often I resort to modulo to check for evens and odds knowing this exists

## chars
chars will split a string into an array of chars

```
 "This is my string".chars
 => ["T", "h", "i", "s", " ", "i", "s", " ", "m", "y", " ", "s", "t", "r", "i", "n", "g"] 
```
of course split('') would do the same, but I like this better


Code challenges are super fun, and its great to learn new tricks. I recomment trying codwars sometime or even hacker rank or leetcode. I think Im going to go do some SQL challenges now!

