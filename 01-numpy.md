---
layout: page
title: Programming with Python
subtitle: Analyzing Patient Data
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Explain what a library is, and what libraries are used for.
> *   Load a Python library and use the things it contains.
> *   Read tabular data from a file into a program.
> *   Assign values to variables.
> *   Select individual values and subsections from data.
> *   Perform operations on arrays of data.
> *   Display simple graphs.

Since Python is a high-level language, one of its best features is the
large number of packages that are available for it. These let you do
rather complex things using surprisingly simple commands. So whenever
there is a need to do something that requires a large number of simple
operations to make up a complex one, the first question you should ask
is not

How do I build this from scratch ?

but rather

Is the a package that does that ?

For instance, there's a bunch of data files in our directory, and
we're going to have a look at the data they contain, and learn some
Python while we're doing it. First thing is to import a library called
NumPy'. This is a numerical package that you pretty much need
everytime you want to do something more advanced with numbers,
especially with matrices or arrays. We can load NumPy using:

~~~ {.python}
import numpy
~~~

Importing a library is like getting a piece of lab equipment out of a storage locker
and setting it up on the bench. Libraries provide additional functionality to the basic Python package, much like a new piece of equipment adds functionality to a lab space.
Once you've loaded the library,
we can ask the library to read our data file for us:

~~~ {.python}
numpy.loadtxt(fname='mandel-small.csv', delimiter=',')
~~~
~~~ {.output}
[[    2.     3.     3.     3.     3.     3.     3.     3.     3.     4.
      4.     4.     5.     7.     9.  1000.     4.     4.     3.     3.
      2.]
 [    3.     3.     3.     3.     3.     3.     3.     4.     4.     4.
      4.     5.     6.     9.    24.     8.     5.     4.     4.     3.
      3.]
 [    3.     3.     3.     3.     3.     3.     4.     4.     4.     4.
      5.     6.     8.  1000.  1000.    18.     6.     5.     5.     4.
      3.]
(...and so on...)
~~~

The expression `numpy.loadtxt(...)` is a [function
call](reference.html#function-call) that asks Python to run the
function `loadtxt` that belongs to the `numpy` library.  This [dotted
notation](reference.html#dotted-notation) is used everywhere in Python
to refer to the parts of things as `thing.component`.

`numpy.loadtxt` has two [parameters](reference.html#parameter): the
name of the file we want to read, and the
[delimiter](reference.html#delimiter) that separates values on a line.
These both need to be character strings (or
[strings](reference.html#string) for short), so we put them in quotes.

When we are finished typing and press Enter, Python executes our
command.  Since we haven't told it to do anything else with the
function's output, it displays it.  In this case, that
output is the data we just loaded.  By default, only a few rows and
columns are shown (with `...` to omit elements when displaying big
arrays).  To save space, Python displays numbers as `1.` instead of
`1.0` when there's nothing interesting after the decimal point.

Our call to `numpy.loadtxt` read our file, but didn't save the data in
memory.  To do that, we need to [assign](reference.html#assignment)
the array to a [variable](reference.html#variable).  A variable is
just a name for a value, such as `x`, `current_temperature`, or
`subject_id`.  Python's variables must begin with a letter and are
[case sensitive](reference.html#case-sensitive).  We can create a new
variable by assigning a value to it using `=`.  As an illustration,
let's step back and instead of considering a table of data, consider
the simplest "collection" of data, a single value.  The line below
assigns the value `55` to a variable `weight_kg`:

~~~ {.python}
weight_kg = 55
~~~

Once a variable has a value, we can print it to the screen:

~~~ {.python}
print(weight_kg)
~~~
~~~ {.output}
55
~~~

and do arithmetic with it:

~~~ {.python}
print('weight in pounds:', 2.2 * weight_kg)
~~~
~~~ {.output}
weight in pounds: 121.0
~~~

We can also change a variable's value by assigning it a new one:

~~~ {.python}
weight_kg = 57.5
print('weight in kilograms is now:', weight_kg)
~~~
~~~ {.output}
weight in kilograms is now: 57.5
~~~

Note that assigning a value to one variable does *not* change the
values of other variables.  For example, let's store the subject's
weight in pounds in a variable:

~~~ {.python}
weight_lb = 2.2 * weight_kg
print('weight in kilograms:', weight_kg, 'and in pounds:', weight_lb)
~~~
~~~ {.output}
weight in kilograms: 57.5 and in pounds: 126.5
~~~

~~~ {.python}
weight_kg = 100.0
print('weight in kilograms is now:', weight_kg, 'and weight in pounds is still:', weight_lb)
~~~
~~~ {.output}
weight in kilograms is now: 100.0 and weight in pounds is still: 126.5
~~~

Since `weight_lb` doesn't "remember" where its value came from, it
isn't automatically updated when `weight_kg` changes.  This is
different from the way spreadsheets work.

Just as we can assign a single value to a variable, we can also assign
an array of values to a variable using the same syntax.  Let's re-run
`numpy.loadtxt` and save its result:

~~~ {.python}
data = numpy.loadtxt(fname='mandel-small.csv', delimiter=',')
~~~

This statement doesn't produce any output because assignment doesn't
display anything.  If we want to check that our data has been loaded,
we can print the variable's value:

~~~ {.python}
print(data)
~~~
~~~ {.output}
[[    2.     3.     3.     3.     3.     3.     3.     3.     3.     4.
      4.     4.     5.     7.     9.  1000.     4.     4.     3.     3.
      2.]
 [    3.     3.     3.     3.     3.     3.     3.     4.     4.     4.
      4.     5.     6.     9.    24.     8.     5.     4.     4.     3.
      3.]
 [    3.     3.     3.     3.     3.     3.     4.     4.     4.     4.
      5.     6.     8.  1000.  1000.    18.     6.     5.     5.     4.
      3.]
(...and so on...)
~~~

Now that our data is in memory, we can start doing things with it.
First, let's ask what [type](reference.html#type) of thing `data`
refers to:

~~~ {.python}
print(type(data))
~~~
~~~ {.output}
<class 'numpy.ndarray'>
~~~

The output tells us that `data` currently refers to an N-dimensional
array created by the NumPy library. These data corresponds to
arthritis patient's inflammation. The rows are the individual patients
and the columns are there daily inflammation measurements.  We can see
what its [shape](reference.html#shape) is like this:

~~~ {.python}
print(data.shape)
~~~
~~~ {.output}
(21, 21)
~~~

This tells us that `data` has 60 rows and 40 columns. When we created
the variable `data` to store our arthritis data, we didn't just create
the array, we also created information about the array, called
[members](reference.html#member) or attributes. This extra information
describes `data` in the same way an adjective describes a noun.
`data.shape` is an attribute of `data` which described the dimensions
of `data`.  We use the same dotted notation for the attributes of
variables that we use for the functions in libraries because they have
the same part-and-whole relationship.

If we want to get a single number from the array, we must provide an
[index](reference.html#index) in square brackets, just as we do in
math:

~~~ {.python}
print('first value in data:', data[0, 0])
~~~
~~~ {.output}
first value in data: 2.0
~~~

~~~ {.python}
print('middle value in data:', data[10, 10])
~~~
~~~ {.output}
middle value in data: 1000.0
~~~

The expression `data[30, 20]` may not surprise you, but `data[0, 0]`
might.  Programming languages like Fortran and MATLAB start counting
at 1, because that's what human beings have done for thousands of
years.  Languages in the C family (including C++, Java, Perl, and
Python) count from 0 because that's simpler for computers to do.  As a
result, if we have an M&times;N array in Python, its indices go from 0
to M-1 on the first axis and 0 to N-1 on the second.  It takes a bit
of getting used to, but one way to remember the rule is that the index
is how many steps we have to take from the start to get the item we
want.

> ## In the Corner {.callout}
>
> What may also surprise you is that when Python displays an array, it
> shows the element with index `[0, 0]` in the upper left corner
> rather than the lower left.  This is consistent with the way
> mathematicians draw matrices, but different from the Cartesian
> coordinates.  The indices are (row, column) instead of (column, row)
> for the same reason, which can be confusing when plotting data.

An index like `[30, 20]` selects a single element of an array, but we
can select whole sections as well.  For example, we can select the
first ten days (columns) of values for the first four patients (rows)
like this:

~~~ {.python}
print(data[0:4, 0:10])
~~~
~~~ {.output}
[[ 2.  3.  3.  3.  3.  3.  3.  3.  3.  4.]
 [ 3.  3.  3.  3.  3.  3.  3.  4.  4.  4.]
 [ 3.  3.  3.  3.  3.  3.  4.  4.  4.  4.]
 [ 3.  3.  3.  3.  3.  4.  4.  4.  5.  7.]]
~~~

The [slice](reference.html#slice) `0:4` means, "Start at index 0 and
go up to, but not including, index 4."  Again, the
up-to-but-not-including takes a bit of getting used to, but the rule
is that the difference between the upper and lower bounds is the
number of values in the slice.

We don't have to start slices at 0:

~~~ {.python}
print(data[5:10, 0:10])
~~~
~~~ {.output}
[[    3.     3.     4.     5.     5.     5.     5.     6.     8.    12.]
 [    4.     5.     7.     7.     7.     7.     7.     7.    11.  1000.]
 [    5.     5.     7.    16.    12.    35.    11.     9.    24.  1000.]
 [    5.     6.     8.    18.  1000.  1000.  1000.    15.  1000.  1000.]
 [    7.    12.    14.  1000.  1000.  1000.  1000.  1000.  1000.  1000.]]
~~~

We also don't have to include the upper and lower bound on the slice.
If we don't include the lower bound, Python uses 0 by default; if we
don't include the upper, the slice runs to the end of the axis, and if
we don't include either (i.e., if we just use ':' on its own), the
slice includes everything:

~~~ {.python}
small = data[:3, 16:]
print(small)
~~~
~~~ {.output}
[[ 4.  4.  3.  3.  2.]
 [ 5.  4.  4.  3.  3.]
 [ 6.  5.  5.  4.  3.]]
~~~

Arrays also know how to perform common mathematical operations on
their values.  The simplest operations with data are arithmetic: add,
subtract, multiply, and divide.  When you do such operations on
arrays, the operation is done on each individual element of the array.
Thus:

~~~ {.python}
doubledata = data * 2.0
~~~

will create a new array `doubledata` whose elements have the value of
two times the value of the corresponding elements in `data`:

~~~ {.python}
print("original:\n",data[:3, 16:],"\ndouble data:\n",doubledata[:3,16:])
~~~
~~~ {.output}
original:
 [[ 4.  4.  3.  3.  2.]
 [ 5.  4.  4.  3.  3.]
 [ 6.  5.  5.  4.  3.]]
double data:
 [[  8.   8.   6.   6.   4.]
 [ 10.   8.   8.   6.   6.]
 [ 12.  10.  10.   8.   6.]]
~~~

Note how we can pile the whole output into a single print
command. That includes the line breaks which are denoted by the string
"\n".

If, instead of taking an array and doing arithmetic with a single
value (as above) you did the arithmetic operation with another array
of the same shape, the operation will be done on corresponding
elements of the two arrays.  Thus:

~~~ {.python}
tripledata = doubledata + data
~~~

will give you an array where `tripledata[0,0]` will equal `doubledata[0,0]` plus `data[0,0]`,
and so on for all other elements of the arrays.

~~~ {.python}
print('tripledata:\n',tripledata[:3, 36:])
~~~
~~~ {.output}
tripledata:
[[ 6.  9.  0.  0.]
 [ 3.  3.  0.  3.]
 [ 6.  6.  3.  3.]]
~~~

For giggles, we used single quotes here and double quotes
earlier. They both work. Often, we want to do more than add, subtract,
multiply, and divide values of data.  Arrays also know how to do more
complex operations on their values.  If we want to find the average
value of the whole data field, for example, we can just ask the array
for its mean value

~~~ {.python}
print(data.mean())
~~~
~~~ {.output}
362.53968254
~~~

`mean` is a [method](reference.html#method) of the array, i.e., a
function that belongs to it in the same way that the member `shape`
does.  If variables are nouns, methods are verbs: they are what the
thing in question knows how to do.  We need empty parentheses for
`data.mean()`, even when we're not passing in any parameters, to tell
Python to go and do something for us. `data.shape` doesn't need `()`
because it is just a description but `data.mean()` requires the `()`
because it is an action.

NumPy arrays have lots of useful methods:

~~~ {.python}
print('maximum:', data.max()); print('minimum:', data.min()); print('std dev:', data.std())
~~~
~~~ {.output}
maximum: 1000.0
minimum: 2.0
std dev: 476.328259961
~~~
Note how we put several print commands in a single line, separated by ";". Seems to do the job. 

When analyzing data, though, we often want to look at partial
statistics, such as the maximum value per patient or the average value
per day.  One way to do this is to create a new temporary array of the
data we want, then ask it to do the calculation:

~~~ {.python}
top_row = data[0, :] # 0 on the first axis, everything on the second
print('Maximum in top row:', top_row.max())
~~~
~~~ {.output}
Maximum in top row: 18.0
~~~

Note how everything after "#" is ignored by python. "#" is used to
make comments. Later, that will help to keep code readable and clarify
what's going on.

We don't actually need to store the row in a variable of its own.
Instead, we can combine the selection and the method call:

~~~ {.python}
print('Maximum value in bottom row', data[-1,:].max())
~~~
~~~ {.output}
Maximum value in bottom row 1000.0
~~~

Note the weird "-1" index ? The convention is that negative indices
count from the bottom starting with -1 (remember, 0 is taken for the
top). It appears that 1000.0 is a frequently occurring value. That's
because we used that as a cut-off when we calculated the data, but we
get to that later.

What if we need the maximum inflammation for *all* patients (as in the
next diagram on the left), or the average for each day (as in the
diagram on the right)? As the diagram below shows, we want to perform the
operation across an axis:

To support this, most array methods allow us to specify the axis we
want to work on.  If we ask for the average across axis 0 (rows in our
2D example), we get:

~~~ {.python}
print(data.mean(axis=0))
~~~
~~~ {.output}
[  51.23809524   52.           52.85714286  148.66666667  241.9047619
  244.19047619  242.0952381   148.28571429  244.28571429  432.66666667
  621.04761905  528.47619048  621.80952381  811.04761905  812.66666667
  718.          621.33333333  526.76190476  479.9047619     9.85714286
    4.23809524]
~~~

As a quick check, we can ask this array what its shape is:

~~~ {.python}
print(data.mean(axis=0).shape)
~~~
~~~ {.output}
(21,)
~~~

The expression `(21,)` tells us we have an N&times;1 vector,
so this is the average inflammation per day for all patients.
If we average across axis 1 (columns in our 2D example), we get:

~~~ {.python}
print(data.mean(axis=1))
~~~
~~~ {.output}
[  51.19047619    5.14285714   99.71428571  100.38095238  291.42857143
  431.80952381  479.80952381  483.0952381   670.85714286  764.0952381
  858.28571429  764.0952381   670.85714286  483.0952381   479.80952381
  431.80952381  291.42857143  100.38095238   99.71428571    5.14285714
   51.19047619]
~~~

which is the average value across columns.

The mathematician Richard Hamming once said, "The purpose of computing
is insight, not numbers," and the best way to develop insight is often
to visualize data.  Visualization deserves an entire lecture (or
course) of its own, but we can explore a few features of Python's
`matplotlib` library here.  While there is no "official" plotting
library, this package is the de facto standard.  First, we will import
the `pyplot` module from `matplotlib` and use two of its functions to
create and display a heat map of our data:

~~~ {.python}
import matplotlib.pyplot
image  = matplotlib.pyplot.imshow(data)
matplotlib.pyplot.show(image)
~~~

![Heatmap of the Data](fig/01-numpy_71_0.png)

Blue regions in this heat map are low values, while red shows high
values. Of course, 21x21 isn't exactly great resolution, and it appear
that the values are either very large (namely, 1000), or quite small
(<20), with very little in between. It is one of the features of the
Mandelbrot set that the "interesting stuff" is happening at the border
region between the read and the blue parts of this low-resolution
plot. Therefore it is exactly the fine structure of that region which
is only visible at much higher resolutions is what we are going to
explore.

> ## Most people dislike typing {.callout}
>
>We used the syntax `import numpy` to import NumPy.  However, in order
>to save typing, it is [often suggested](http://www.scipy.org/getting-started.html#an-example-script)
>to make a shortcut like so: `import numpy as np`.  If you ever see
>Python code online using a NumPy function with `np` (for example,
>`np.loadtxt(...)`), it's because they've used this shortcut.

> ## Check your understanding {.challenge}
>
> Draw diagrams showing what variables refer to what values after each statement in the following program:
>
> ~~~ {.python}
> mass = 47.5
> age = 122
> mass = mass * 2.0
> age = age - 20
> ~~~

> ## Sorting out references {.challenge}
>
> What does the following program print out?
>
> ~~~ {.python}
> first, second = 'Grace', 'Hopper'
> third, fourth = second, first
> print(third, fourth)
> ~~~

> ## Slicing strings {.challenge}
>
> A section of an array is called a [slice](reference.html#slice).
> We can take slices of character strings as well:
>
> ~~~ {.python}
> element = 'oxygen'
> print('first three characters:', element[0:3])
> print('last three characters:', element[3:6])
> ~~~
>
> ~~~ {.output}
> first three characters: oxy
> last three characters: gen
> ~~~
>
> What is the value of `element[:4]`?
> What about `element[4:]`?
> Or `element[:]`?
>
> What is `element[-1]`?
> What is `element[-2]`?
> Given those answers,
> explain what `element[1:-1]` does.

> ## Thin slices {.challenge}
>
> The expression `element[3:3]` produces an [empty string](reference.html#empty-string),
> i.e., a string that contains no characters.
> If `data` holds our array of patient data,
> what does `data[3:3, 4:4]` produce?
> What about `data[3:3, :]`?

> ## Check your understanding: plot scaling {.challenge}
>
> Why do all of our plots stop just short of the upper end of our graph?

> ## Check your understanding: drawing straight lines {.challenge}
>
> Why are the vertical lines in our plot of the minimum inflammation per day not perfectly vertical?

> ## Make your own plot {.challenge}
>
> Create a plot showing the standard deviation (`numpy.std`) of the inflammation data for each day across all patients.

> ## Moving plots around {.challenge}
>
> Modify the program to display the three plots on top of one another instead of side by side.
