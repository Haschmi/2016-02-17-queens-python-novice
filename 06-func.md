---
layout: page
title: Programming with Python
subtitle: Creating Functions
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Define a function that takes parameters.
> *   Return a value from a function.
> *   Test and debug a function.
> *   Set default values for function parameters.
> *   Explain why we should divide programs into small, single-purpose functions.

At this point, we’ve written code to perform our Mandelbrot iterations
for a specific complex number but that is a far cry of covering all of
them. Later we want to loop through thousands of those numbers and
check for each how many iterations it is going to take before they
"bail out". For this, it would be best to turn our few lines of code
that perform and count the iterations into a function. Functions are
re-usable pieces of code that make repeated reference to that code
easy. Instead of "cutting and pasting" several lines of code every
time we want to use them, we refer to them by name.

Let's start by defining a function `fahr_to_kelvin` that converts
temperatures from Fahrenheit to Kelvin:

~~~ {.python}
def fahr_to_kelvin(temp):
    return ((temp - 32) * (5/9)) + 273.15
~~~

The function definition opens with the word `def`, which is followed
by the name of the function and a parenthesized list of parameter
names.  The [body](reference.html#function-body) of the function ---
the statements that are executed when it runs --- is indented below
the definition line, typically by four spaces.

When we call the function, the values we pass to it are assigned to
those variables so that we can use them inside the function.  Inside
the function, we use a [return
statement](reference.html#return-statement) to send a result back to
whoever asked for it.

Let's try running our function.  Calling our own function is no
different from calling any other function:

~~~ {.python}
print('freezing point of water:', fahr_to_kelvin(32),'\n boiling point of water:', fahr_to_kelvin(212))
~~~
~~~ {.output}
freezing point of water: 273.15
boiling point of water: 373.15
~~~

We've successfully called the function that we defined,
and we have access to the value that we returned.

> ## Differences in Python 2/3 {.callout}
>
> In Python 3, a division always returns a floating point number. In
> Python 2, division of one integer by another yields another integer
> that is truncated from the real value; this is called "integer
> division". Another difference is that Python 3 requires parenthesis
> for the print command (because it's a function), while Python 2 does
> not:
> ~~~ {.python}
> $ python3 -c "print(5/9)"
> ~~~
> ~~~ {.output}
> 0.5555555555555556
> ~~~
> ~~~ {.python}
> $ python -c "print 5/9"
> ~~~
> ~~~ {.output}
> 0
> ~~~
>
> If you are using Python 2 and want to keep the fractional part of division
> you need to convert one or the other number to floating point:
>
> ~~~ {.python}
> $ python -c "print float(5)/9"
> ~~~
> ~~~ {.output}
> 0.555555555556
> ~~~
>
> And if you want an integer result from division in Python 3,
> use a double-slash:
> ~~~ {.python}
> $ python3 -c "print(3//2)"
> ~~~
> ~~~ {.output}
> 1
> ~~~

In most cases it's best to use real number instead of integers (just
put a decimal point at the end like we do in our FtoC example), if the
full accuracy is required and we want to avoid trouble from integer
divisions.

## Composing Functions

Now let's try our iteration counter in the form of a function: 

~~~ {.python}
def mandit (b):
    z=0.0
    for it in range(1,1001):
        z=z*z+b
        if (abs(z)>2.0):
            break
    return it

print ("Number of iterations for (-1,i):", mandit(-1+1j))
~~~
~~~ {.output}
Number of iterations for (-1,i): 3
~~~
~~~ {.python}
print ("Number of iterations for (-0.5,0.5i):", mandit(-0.5+0.5j))
~~~
~~~ {.output}
Number of iterations for (-0.5,0.5i): 1000
~~~

This is our first taste of how larger programs are built: we define
basic operations, then combine them in ever-large chunks to get the
effect we want.  Real-life functions will usually be larger than the
ones shown here --- typically half a dozen to a few dozen lines ---
but they shouldn't ever be much longer than that, or the next person
who reads it won't be able to understand what's going on.

By giving our functions human-readable names, we can more easily read
and understand what is happening in the `for` loop.  Even better, if
at some later date we want to use either of those pieces of code
again, we can do so in a single line.

## Using Functions in Loops

At this point it is easy to see how we can repeatedly call the
function mandit inside a two nested loops to compute as many points on
the Mandelbrot set as we want:

~~~ {.python}
for i in range(0,4):
    for j in range(0,4):
         c=-1.5+0.5*i+(-1.0+0.5*j)*1j
         print (c,"bails after",mandit(c),"its")
	 
~~~
~~~ {.output}
(-1.5-1j) bails after 2 its
(-1.5-0.5j) bails after 3 its
(-1.5+0j) bails after 1000 its
(-1.5+0.5j) bails after 3 its
(-1-1j) bails after 3 its
(-1-0.5j) bails after 5 its
(-1+0j) bails after 1000 its
(-1+0.5j) bails after 5 its
(-0.5-1j) bails after 4 its
(-0.5-0.5j) bails after 1000 its
(-0.5+0j) bails after 1000 its
(-0.5+0.5j) bails after 1000 its
-1j bails after 1000 its
-0.5j bails after 1000 its
0j bails after 1000 its
0.5j bails after 1000 its
~~~

This is getting pretty close to the data that we used at the beginning
of the section. Those were 20x20 and they were in the form of a numpy
array, so let's try to reproduce this:

~~~ {.python}
for i in range(0,20):
    for j in range(0,20):
         c=-1.5+0.1*i+(-1.0+0.1*j)*1j
         a[j][i]=mandit.mandit(c)

print(a)
~~~

~~~ {.output}
[[    2.     3.     3.     3.     3.     3.     3.     3.     3.     4.
      4.     4.     5.     7.     9.  1000.     4.     4.     3.     3.]
 [    3.     3.     3.     3.     3.     3.     3.     4.     4.     4.
      4.     5.     6.     9.    24.     8.     5.     4.     4.     3.]
 [    3.     3.     3.     3.     3.     3.     4.     4.     4.     4.
      5.     6.     8.  1000.  1000.    18.     6.     5.     5.     4.]
 [    3.     3.     3.     3.     3.     4.     4.     4.     5.     7.
      8.     8.    10.  1000.  1000.    13.     9.     6.     6.     5.]
 [    3.     3.     3.     3.     4.     4.     5.     5.     6.    12.
   1000.    26.  1000.  1000.  1000.  1000.  1000.    12.    15.    15.]
 [    3.     3.     4.     5.     5.     5.     5.     6.     8.    12.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.     7.]
 [    4.     5.     7.     7.     7.     7.     7.     7.    11.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.     9.]
 [    5.     5.     7.    16.    12.    35.    11.     9.    24.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.    15.]
 [    5.     6.     8.    18.  1000.  1000.  1000.    15.  1000.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.    31.]
 [    7.    12.    14.  1000.  1000.  1000.  1000.  1000.  1000.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.     8.]
 [ 1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.    12.     7.]
 [    7.    12.    14.  1000.  1000.  1000.  1000.  1000.  1000.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.     8.]
 [    5.     6.     8.    18.  1000.  1000.  1000.    15.  1000.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.    31.]
 [    5.     5.     7.    16.    12.    35.    11.     9.    24.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.    15.]
 [    4.     5.     7.     7.     7.     7.     7.     7.    11.  1000.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.     9.]
 [    3.     3.     4.     5.     5.     5.     5.     6.     8.    12.
   1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.  1000.     7.]
 [    3.     3.     3.     3.     4.     4.     5.     5.     6.    12.
   1000.    26.  1000.  1000.  1000.  1000.  1000.    12.    15.    15.]
 [    3.     3.     3.     3.     3.     4.     4.     4.     5.     7.
      8.     8.    10.  1000.  1000.    13.     9.     6.     6.     5.]
 [    3.     3.     3.     3.     3.     3.     4.     4.     4.     4.
      5.     6.     8.  1000.  1000.    18.     6.     5.     5.     4.]
 [    3.     3.     3.     3.     3.     3.     3.     4.     4.     4.
      4.     5.     6.     9.    24.     8.     5.     4.     4.     3.]]
~~~

The beauty of this is that once we defined the function, we can re-use
it in every new loop we are making, and don't have to re-type (or
cut-and-paste) the instructions.

## Testing and Documenting

Once we start putting things in functions so that we can re-use them,
we need to start testing that those functions are working
correctly. The easiest way to do this in our case is to plot our
results. Since we already have stored our results in the form of a
"numpy array", this should be easy to do:

~~~ {.python}
plt.show(plt.imshow(a))
~~~

Well, it looks right. In fact the picture is pretty much identical of
what we got when we looked at the input data. We could of course also
directly compare the data by reading in the original data and
subtracting them in bulk from our array. The result should be all
close to zero. Why not write a function for that:

~~~ {.python}
def compare(data, reference):
    '''Compares one set of data to another and returns the difference.'''
    return data-reference

ref=np.loadtxt(fname='mandel-small.csv', delimiter=',')
print(compare(a,ref))
~~~
~~~ {.output}
[[ 0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.
   0.  0.]
(...etc...)
 [ 0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.
   0.  0.]]
~~~

Bingo. A bit long though, particularly if we are checking larger
amounts of data. We can take all the absolute values of the
differences and sum them up across the whole array, for one single
value:

~~~ {.python}
print(sum(sum(abs(compare(a,ref)))))
~~~
~~~ {.output}
0.0
~~~

which means that all the errors combined in the two data sets yield
exactly zilch, i.e. they are identical. But what is this stuff in the
triple-quotes in compare()? This is to insert some documentation for
our function to remind ourselves later what it’s for and how to use
it.

If the first thing in a function is a string that isn’t assigned to a
variable, that string is attached to the function as its
documentation: We can now ask Python’s built-in help system to show us
the documentation for the function:

~~~ {.python}
help(compare)
~~~
~~~ {.output}
Help on function compare in module __main__:

compare(data, reference)
    Compares one set of data to another and returns the difference.
~~~

A string like this is called a docstring. We don’t need to use triple
quotes when we write one, but if we do, we can break the string across
multiple lines:

~~~ {.python}
def compare(data, reference):
    '''Compares one set of data to another and returns the difference. 
       Both data sets need to have the same shape.
       Example: compare(3,5) returns -2'''
    return data-reference

help(compare)
~~~
~~~ {.output}
Help on function compare in module __main__:

compare(data, reference)
    Compares one set of data to another and returns the difference. 
    Both data sets need to have the same shape.
    Example: compare(3,5) returns -2
~~~

## Defining Defaults

We have passed parameters to functions in two ways: directly, as in
compare(a,ref), and by name, as in
np.loadtxt(fname='mandel-small.csv', delimiter=','). In fact, we can
pass the filename to loadtxt without the fname=:

~~~ {.python}
np.loadtxt('mandel-small.csv',delimiter=',')
~~~
~~~ {.output}
array([[    2.,     3.,     3.,     3.,     3.,     3.,     3.,     3.,
            3.,     4.,     4.,     4.,     5.,     7.,     9.,  1000.,
            4.,     4.,     3.,     3.,     2.], ...bla bla bla...
~~~

but we still need to say `delimiter=`:

~~~ {.python}
numpy.loadtxt('mandel-small.csv', ',')
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/opt/python/anaconda3-2.3.0/lib/python3.4/site-packages/numpy/lib/npyio.py", line 805, in loadtxt
    dtype = np.dtype(dtype)
TypeError: data type "," not understood
~~~

To understand what's going on, and make our own functions easier to
use, let's re-define our 'compare()' function like this:

~~~ {.python}
def compare(data, reference=0.0):
    ''' Compares one set of data to another and returns the difference. (0 by default).
    Example: compare(2, 1) => 1'''
    return data - reference
~~~

The key change is that the second parameter is now written
reference=0.0 instead of just reference. If we call the function with
two arguments, it works as it did before, but with just one parameter,
the reference is automatically assigned the default value of 0:

~~~ {.python}
compare(2,1); compare(2)
~~~
~~~ {.output}
1
2
~~~

But we can also now call it with just one parameter,
in which case `desired` is automatically assigned the [default value](reference.html#default-value) of 0.0:

~~~ {.python}
more_data = 5 + numpy.zeros((2, 2))
print('data before centering:')
print(more_data)
print('centered data:')
print(center(more_data))
~~~
~~~ {.output}
data before centering:
[[ 5.  5.]
 [ 5.  5.]]
centered data:
[[ 0.  0.]
 [ 0.  0.]]
~~~

This is handy:
if we usually want a function to work one way,
but occasionally need it to do something else,
we can allow people to pass a parameter when they need to
but provide a default to make the normal case easier.
The example below shows how Python matches values to parameters:

~~~ {.python}
def display(a=1, b=2, c=3):
    print('a:', a, 'b:', b, 'c:', c)

print('no parameters:')
display()
print('one parameter:')
display(55)
print('two parameters:')
display(55, 66)
~~~
~~~ {.output}
no parameters:
a: 1 b: 2 c: 3
one parameter:
a: 55 b: 2 c: 3
two parameters:
a: 55 b: 66 c: 3
~~~

As this example shows, parameters are matched up from left to right,
and any that haven't been given a value explicitly get their default
value.  We can override this behavior by naming the value as we pass
it in:

~~~ {.python}
display(c=77)
~~~
~~~ {.output}
a: 1 b: 2 c: 77
~~~

With that in hand, let's look at the help for `numpy.loadtxt`:

~~~ {.python}
help(numpy.loadtxt)
~~~
~~~ {.output}

~~~help(numpy.loadtxt)
Help on function loadtxt in module numpy.lib.npyio:

loadtxt(fname, dtype=<class 'float'>, comments='#', delimiter=None, converters=None, skiprows=0, usecols=None, unpack=False, ndmin=0)
    Load data from a text file.

    Each row in the text file must have the same number of values.

    Parameters
    ----------
(...lots of info about parameters + examples removed for brevity...

This tells us that `loadtxt` has one parameter called `fname` that
doesn't have a default value, and eight others that do.  If we call
the function like this:

~~~python
numpy.loadtxt('inflammation-01.csv', ',')
~~~

then the filename is assigned to `fname` (which is what we want), but
the delimiter string `','` is assigned to `dtype` rather than
`delimiter`, because `dtype` is the second parameter in the
list. However ',' isn't a known `dtype` so our code produced an error
message when we tried to run it.  When we call `loadtxt` we don't have
to provide `fname=` for the filename because it's the first item in
the list, but if we want the ',' to be assigned to the variable
`delimiter`, we *do* have to provide `delimiter=` for the second
parameter since `delimiter` is not the second parameter in the list.

> ## Combining strings {.challenge}
>
> "Adding" two strings produces their concatenation: `'a' + 'b'` is
> `'ab'`.  Write a function called `fence` that takes two parameters
> called `original` and `wrapper` and returns a new string that has
> the wrapper character at the beginning and end of the original.  A
> call to your function should look like this: ~~~ {.python}
> print(fence('name', '*')) ~~~ ~~~ {.output} *name* ~~~

> ## Selecting characters from strings {.challenge}
>
> If the variable `s` refers to a string, then `s[0]` is the string's
> first character and `s[-1]` is its last.  Write a function called
> `outer` that returns a string made up of just the first and last
> characters of its input.  A call to your function should look like
> this:
>
> ~~~ {.python}
> print(outer('helium'))
> ~~~
> ~~~ {.output}
> hm
> ~~~

> ## Rescaling an array {.challenge}
>
> Write a function `rescale` that takes an array as input and returns
> a corresponding array of values scaled to lie in the range 0.0 to
> 1.0.  (Hint: If $L$ and $H$ are the lowest and highest values in the
> original array, then the replacement for a value $v$ should be
> $(v-L) / (H-L)$.)

> ## Testing and documenting your function {.challenge}
>
> Run the commands `help(numpy.arange)` and `help(numpy.linspace)` to
> see how to use these functions to generate regularly-spaced values,
> then use those values to test your `rescale` function.  Once you've
> successfully tested your function, add a docstring that explains
> what it does.

> ## Defining defaults {.challenge}
>
> Rewrite the `rescale` function so that it scales data to lie between
> 0.0 and 1.0 by default, but will allow the caller to specify lower
> and upper bounds if they want.  Compare your implementation to your
> neighbor's: do the two functions always behave the same way?

> ## Variables inside and outside functions {.challenge}
>
> What does the following piece of code display when run - and why?
>
> ~~~ {.python}
> f = 0
> k = 0
>
> def f2k(f):
>   k = ((f-32)*(5.0/9.0)) + 273.15
>   return k
>
> f2k(8)
> f2k(41)
> f2k(32)
>
> print(k)
> ~~~
