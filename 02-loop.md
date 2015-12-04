---
layout: page
title: Programming with Python
subtitle: Repeating Actions with Loops
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Explain what a for loop does.
> *   Correctly write for loops to repeat simple calculations.
> *   Trace changes to a loop variable as the loop runs.
> *   Trace changes to other variables as they are updated by a for loop.

In the last lesson, we wrote some code that plots values from a
pre-calculated dataset of Mandelbrot iteration numbers.We fond that a
small dataset does not really properly show the finer features of the
set, so maybe we should write a program that lets us decide which
region we would like to look at, and at what resolution.

Since for each of that data-points, we'll have to do up to 1000
iteration, we better find out how to get the computer how to "repeat
stuff". This is done with a loop.

Here's an example: If we want to print out the letters in a word
one-by-one, it would look something like this:

~~~ {.python}
word = 'word'
print(word[0])
print(word[1])
print(word[2])
print(word[3])

~~~
~~~ {.output}
w
o
r
d
~~~

That's not exactly elegant: We're spending a lot of effort for little
gain, and in case of a string with hundreds of letter, we're typing
ourselves to death. On top of it, this approach is error-prone:

~~~ {.python}
word = 'tin'
print(word[0])
print(word[1])
print(word[2])
print(word[3])

~~~
~~~ {.output}
t
i
n
~~~
~~~ {.error}
>>> print(word[3])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
~~~

because the element word[3] does not exist in 'tin'. 
Here's a much better approach:

~~~ {.python}
word = 'tin'
for char in word:
    print(char)

~~~

~~~ {.output}
t
i
n
~~~

This is not only shorter, its also more robust because it works with any string size:

~~~ {.python}
word = 'This is the winter of our discontent'
for char in word:
    print(char)
~~~

~~~ {.output}
T
h
i
s

i
s

t
h
e

w
i
n
t
e
r

o
f

o
u
r

d
i
s
c
o
n
t
e
n
t
~~~

The improved version of `print_characters` uses a [for
loop](reference.html#for-loop) to repeat an operation---in this case,
printing---once for each thing in a collection.  The general form of a
loop is:

~~~ {.python}
for variable in collection:
    do things with variable
~~~

We can call the [loop variable](reference.html#loop-variable) anything
we like, but there must be a colon at the end of the line starting the
loop, and we must indent anything we want to run inside the
loop. Unlike many other languages, there is no command to end a loop
(e.g. end for); what is indented after the for statement belongs to
the loop.

Here's another loop that repeatedly updates a variable:

~~~ {.python}
length=0
for letter in 'Mandelbrot':
    length=length+1
print ("There are ",length," letters in \'Mandelbrot\'")
~~~

~~~ {.output}
There are  10  letters in 'Mandelbrot'
~~~

It’s worth tracing the execution of this little program step by
step. For each of the characters in the string 'Mandelbrot' the
statement on line 3 is executed once. Beginning with length zero (the
value assigned to it on line 1) the variable letter is 'M'. The
statement adds 1 to the old value of length, producing 1, and updates
length to refer to that new value. The next time around, vowel is 'a'
and length is 1, so length is updated to be 2, etc. After eight more
updates, length is 10 and letter is t; since there is nothing left in
'Mandelbrot' for Python to process, the loop finishes and the print
statement on line 4 tells us our final answer.

Note that a loop variable is just a variable that's being used to
record progress in a loop.  It still exists after the loop is over,
and we can re-use variables previously defined as loop variables as
well:

~~~ {.python}
letter = 'A'
for letter in 'Mandelbrot':
    length=length+1
print('After the loop, letter is', letter)
~~~

~~~ {.output}
After the loop, letter is t
~~~

Note that Python also has a built-in function to determine the length
of a string:

~~~ {.python}
print(len('Mandelbrot'))
~~~

~~~ {.output}
10
~~~

which does the same job with a one-liner. len is much faster than any
function we could write ourselves, and much easier to read than a
two-line loop; it will also give us the length of many other things
that we haven’t met yet, so we should always use it when we can.

> ## From 1 to N {.challenge}
>
> Python has a built-in function called range that creates a sequence
> of numbers. Range can accept 1-3 parameters. If one parameter is
> input, range creates an array of that length, starting at zero and
> incrementing by 1. If 2 parameters are input, range starts at the
> first and ends at the second, incrementing by one. If range is
> passed 3 parameters, it stars at the first one, ends at the second
> one, and increments by the third one. For instance,
>
> ~~~ {.python}
> for i in range(4,10,2):
>     print(i)
> ~~~
>
> ~~~ {.output}
> 4
> 6
> 8
> ~~~
> ## Computing powers with loops {.challenge}
>
> Exponentiation is built into Python:
>
> ~~~ {.python}
> print(5**3)
> 125
> ~~~
>
> Write a loop that calculates the same result as `5 ** 3` using
> multiplication (and without exponentiation).

> ## Reverse a string {.challenge}
>
> Write a loop that takes a string,
> and produces a new string with the characters in reverse order,
> so `'Newton'` becomes `'notweN'`.
