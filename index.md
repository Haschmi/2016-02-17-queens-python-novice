---
layout: page
title: Programming with Python
---

The best way to learn how to program is to do something either fun or
useful, so this introduction to Python is built a hopefully fun
example: The computation and visualization of the so-called Mandelbrot
Set.

The [Mandelbrot set] (https://en.wikipedia.org/wiki/Mandelbrot_set) is
a set of complex numbers for which a certain iterative sequence does
not diverge. Since complex numbers have a real and an imaginary part,
they can be visualized on a plane, the so-called complex plane. It
happens that the Mandelbrot set has a very rich and "complex"
structure which we are going to explore.

If you are curious, the sequence mentioned is "add-and-square":
Starting with 0, you add a given complex number, square the result,
add the same number, square again, etc.

Our real goal isn't to teach you about the Mandelbrot set of
course. It's not even to teach you Python, but to teach you the basic
concepts that all programming depends on.  We use Python in our
lessons because:

1.  we have to use *something* for examples;
2.  it's free, well-documented, and runs almost everywhere;
3.  it has a large (and growing) user base among scientists; and
4.  experience shows that it's easier for novices to pick up than most other languages.

But the two most important things are to use whatever language your
colleagues are using, so that you can share your work with them
easily, and to use that language *well*.

When dealing with the Mandelbrot fractal set, we will generate and
handle data sets are stored in [comma-separated
values](reference.html#comma-separated-values) (CSV) format: each row
corresponds to a fixed imaginary part and each column to a real part
of a number in the complex plane. The first few rows of our first file
look something like this:

~~~
2,3,3,3,3,3,3,3,3,4,4,4,5,7,9,1000,4,4,3,3
3,3,3,3,3,3,3,4,4,4,4,5,6,9,24,8,5,4,4,3
3,3,3,3,3,3,4,4,4,4,5,6,8,1000,1000,18,6,5,5,4
3,3,3,3,3,4,4,4,5,7,8,8,10,1000,1000,13,9,6,6,5
3,3,3,3,4,4,5,5,6,12,1000,26,1000,1000,1000,1000,1000,12,15,15
3,3,4,5,5,5,5,6,8,12,1000,1000,1000,1000,1000,1000,1000,1000,1000,7
4,5,7,7,7,7,7,7,11,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,9
5,5,7,16,12,35,11,9,24,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,15
5,6,8,18,1000,1000,1000,15,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,31
7,12,14,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,8
1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,12,7
7,12,14,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,8
5,6,8,18,1000,1000,1000,15,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,31
5,5,7,16,12,35,11,9,24,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,15
4,5,7,7,7,7,7,7,11,1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,9
3,3,4,5,5,5,5,6,8,12,1000,1000,1000,1000,1000,1000,1000,1000,1000,7
3,3,3,3,4,4,5,5,6,12,1000,26,1000,1000,1000,1000,1000,12,15,15
3,3,3,3,3,4,4,4,5,7,8,8,10,1000,1000,13,9,6,6,5
3,3,3,3,3,3,4,4,4,4,5,6,8,1000,1000,18,6,5,5,4
3,3,3,3,3,3,3,4,4,4,4,5,6,9,24,8,5,4,4,3
~~~

As an aside, from the 'typographical density' of the numbers in this small file we can almost guess the basic outline of the set we are investigatign.

But for now We want to:

*   load that data into memory,
*   plot the result.

To do that, we'll have to learn a little bit about programming.

> ## Prerequisites {.prereq}
>
>Learners need to understand the concepts of files and directories
>(including the working directory) and how to start a Python
>interpreter before tackling this lesson.  The commands in this lesson
>pertain to **Python 3**.

> ## Getting ready {.getready}
>
> All the required data can be found in your home directory on your account
>
> 1. There is a folder (directory) python-novice-mandelbrot in your home directory.
> 2. It contains another folder called "data". 
> 3. You can access this folder from the Unix shell with:
>
> ~~~ {.input}
> $ cd && cd python
> ~~~

## Topics

1.  [Data Files and Data Visualization](01-numpy.html)
2.  [Repeating Actions with Loops](02-loop.html)
3.  [Storing Multiple Values in Lists](03-lists.html)
4.  [Analyzing Data from Multiple Files](04-files.html)
5.  [Making Choices](05-cond.html)
6.  [Creating Functions](06-func.html)
7.  [Errors and Exceptions](07-errors.html)
8.  [Defensive Programming](08-defensive.html)
9.  [Debugging](09-debugging.html)
10. [Command-Line Programs](10-cmdline.html)

## Other Resources

*   [Reference](reference.html)

