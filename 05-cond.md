---
layout: page
title: Programming with Python
subtitle: Making Choices
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Explain the similarities and differences between tuples and lists.
> *   Write conditional statements including `if`, `elif`, and `else` branches.
> *   Correctly evaluate expressions containing `and` and `or`.

The Mandelbrot set is based on an iterative process. As explained
earlier, as soon as the absolute value of our iteration variable
exceeds 2, the underlying series diverges, and we're "done". In this
lesson, we’ll learn how to write code that runs only when certain
conditions are true.

## Conditionals

We can ask Python to take different actions, depending on a condition,
with an if statement:

~~~ {.python}
num = 37
if num > 100:
    print('greater')
else:
    print('not greater')
print('done')
~~~
~~~ {.output}
not greater
done

~~~

The second line of this code uses the keyword `if` to tell Python that
we want to make a choice.  If the test that follows the `if` statement
is true, the body of the `if` (i.e., the lines indented underneath it)
are executed.  If the test is false, the body of the `else` is
executed instead.  Only one or the other is ever executed:

![Executing a Conditional](fig/python-flowchart-conditional.svg)\

Conditional statements don't have to include an `else`.
If there isn't one,
Python simply does nothing if the test is false:

~~~ {.python}
num = 37
if num > 100:
    print('53 is greater than 100')

~~~

(nothing happens). We can also chain several tests together using
`elif`, which is short for "else if".  The following Python code uses
`elif` to print the sign of a number.

~~~ {.python}
if num > 0:
    print("Positive")
elif num == 0:
    print("Zero")
else:
    print("Negative")
~~~
~~~ {.output}
Positive
~~~

Note that for all this stuff we use the same convention as for loops:
Everything that is indented by the same amount and follows a colon ":"
belongs to the same "block" and is executed together.

One important thing to notice in the code above is that we use a
double equals sign `==` to test for equality rather than a single
equals sign because the latter is used to mean assignment.

We can also combine tests using `and` and `or`.
`and` is only true if both parts are true:

~~~ {.python}
if (1>0) and (1<0):
    print ("Both are true")
else:
    print ("At least one is not true")
~~~
~~~ {.output}
At least one is not true
~~~

while `or` is true if at least one part is true:

~~~ {.python}
if (1>0) or (1<0):
    print ("At least one is true")
else:
    print ("Neither is true")
~~~
~~~ {.output}
At least one is true
~~~

## Counting Mandelbrot Iterations

Now that we’ve seen how conditionals work, we can use them to run one
of our Mandelbrot series. The first thing we need is a package called
cmath that teaches Python complex math:

~~~ {.python}
import cmath
~~~

This package has a constant 1j that stands for the imaginary unit i,
i.e. the square-root of -1. Otherwise the complex-math package handles
numbers that contain this constant just like any other number, so
let's set a variable to a complex number:

~~~{.python}
b=-1.0+1j
~~~

Now we initialize our test variable and try out, say a thousand times
at maximum, if the Mandelbrot iteration yields a number that is
greater than 2:

~~~ {.python}
z=0.0
for it in range(1,1001):
    z=z*z+b
    if (abs(z)>2.0):
        break
~~~

The second line defines how many iterations we try (1000 with
iteration number it). The third one performs the "square and add"
iteration. Line three tests if it's going to diverge; note that the
abs function computes the absolute value which is a measure of the
size of a complex number. The break' command in the fourth line tells
it to "jump out of the loop" if the divergence criterion is fulfilled.

Now we can print out the result of our little test:

~~~ {.python}
print b, "bails out after", it, "iterations"
~~~

~~~ {.output}
(-1+1j) bails out after 3 iterations
~~~

In this way, we have asked Python to do something different depending
on the condition of our data.  Here we printed messages in all cases,
but we could also imagine not using the `else` catch-all so that
messages are only printed when something is wrong, freeing us from
having to manually examine every plot for features we've seen before.

> ## How many paths? {.challenge}
>
> Which of the following would be printed if you were to run this
> code? Why did you pick this answer?
>
> 1.  A
> 2.  B
> 3.  C
> 4.  B and C
>
> ~~~ {.python}
> if 4 > 5:
>     print('A')
> elif 4 == 5:
>     print('B')
> elif 4 < 5:
>     print('C')
> ~~~

> ## What is truth? {.challenge}
> `True` and `False` are special words in Python called `booleans`
> which represent true and false statements. However, they aren't the
> only values in Python that are true and false.  In fact, *any* value
> can be used in an `if` or `elif`.  After reading and running the
> code below, explain what the rule is for which values are considered
> true and which are considered false.
> ~~~ {.python}
> if '':
>     print('empty string is true')
> if 'word':
>     print('word is true')
> if []:
>     print('empty list is true')
> if [1, 2, 3]:
>     print('non-empty list is true')
> if 0:
>     print('zero is true')
> if 1:
>     print('one is true')
> ~~~

> ## Close enough {.challenge}
>
> Write some conditions that print `True` if the variable `a` is
> within 10% of the variable `b` and `False` otherwise.  Compare your
> implementation with your partner's: do you get the same answer for
> all possible pairs of numbers?


> ## In-place operators {.challenge}
>
> Python (and most other languages in the C family) provides [in-place
> operators](reference.html#in-place-operator) that work like this:
>
> ~~~ {.python}
> x = 1  # original value
> x += 1 # add one to x, assigning result back to x
> x *= 3 # multiply x by 3
> print(x)
> ~~~
> ~~~ {.output}
> 6
> ~~~
>
> Write some code that sums the positive and negative numbers in a
> list separately, using in-place operators.  Do you think the result
> is more or less readable than writing the same without in-place
> operators?

> ## Tuples and exchanges {.challenge}
>
> Explain what the overall effect of this code is:
>
> ~~~ {.python}
> left = 'L'
> right = 'R'
>
> temp = left
> left = right
> right = temp
> ~~~
>
> Compare it to:
>
> ~~~ {.python}
> left, right = right, left
> ~~~
>
> Do they always do the same thing?
> Which do you find easier to read?
