---
layout: page
title: Programming with Python
subtitle: Command-Line Programs
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Use the values of command-line arguments in a program.
> *   Handle flags and files separately in a command-line program.
> *   Read data from standard input in a program so that it can be used in a pipeline.

The IPython Notebook and other interactive tools are great for prototyping code and exploring data,
but sooner or later we will want to use our program in a pipeline
or run it in a shell script to process thousands of data files. In order to do that,
we need to make our programs work like other Unix command-line tools. For example,
we may want a program that reads a dataset and prints the average inflammation per patient.

> ## Switching to Shell Commands {.callout}
> In this lesson we are switching from typing commands in a Python interpreter to typing
> commands in a shell terminal window (such as bash). When you see a `$` in front of a
> command that tells you to run that command in the shell rather than the Python interpreter.

This program does exactly what we want - we give it the name of a data file and it shows us the picture of a "heatmap" of those data:

~~~
$ python3 picture.py mandel-small.csv
[...shows a picture of an ugly little map...]
$ python3 picture.py mandel-4-big.csv
[...shows a picture of a beautiful bigger map...]
~~~

To make this work, we need to know how to handle command-line arguments in a program.
We'll tackle this below.

## Command-Line Arguments

Using the text editor of your choice, save the following in a text file called `sys-version.py`:

~~~ {.python}
import sys
print('version is', sys.version)
~~~

The first line imports a library called `sys`, which is short for "system". It defines values such as `sys.version`,
which describes which version of Python we are running. We can run this script from the command line like this:

~~~ {.input}
$ python sys-version.py
~~~

~~~ {.output}
$ python3 sys-version.py
version is 3.4.4 |Anaconda 2.3.0 (64-bit)| (default, Jan 11 2016, 13:54:01)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)]
~~~

Create another file called `argv-list.py` and save the following text to it.

~~~ {.python}
import sys
print('sys.argv is', sys.argv)
~~~

The strange name `argv` stands for "argument values". Whenever Python runs a program,
it takes all of the values given on the command line and puts them in the list `sys.argv`
so that the program can determine what they were. If we run this program with no arguments:

~~~ {.input}
$ python3 argv-list.py
~~~

~~~ {.output}
sys.argv is ['argv-list.py']
~~~

the only thing in the list is the full path to our script, which is always `sys.argv[0]`.
If we run it with a few arguments, however:

~~~ {.input}
$ python3 argv-list.py moe larry curly
~~~
~~~ {.output}
sys.argv is ['argv-list.py', 'moe', 'larry', 'curly']
~~~

then Python adds each of those arguments to that magic list.

With this in hand, let's build a version of `picture.py` that always prints the per-patient mean of a single data file.
The first step is to write a function that outlines our implementation, and a placeholder for the function that does the actual work.
By convention this function is usually called `main`, though we can call it whatever we want:

~~~ {.input}
$ cat picture.py
~~~

~~~ {.python}
import matplotlib.pyplot as plt
import numpy as np
import sys

def main():
    script = sys.argv[0]
    filename = sys.argv[1]
    data = np.loadtxt(filename, delimiter=',')
    image=plt.imshow(data)
    plt.show(image)
~~~

This function gets the name of the script from `sys.argv[0]`,
because that's where it's always put, and the name of the file to process from `sys.argv[1]`.
Here's a simple test:

~~~ {.input}
$ python3 picture.py mandel-big-2.csv
~~~

There is no output because we have defined a function, but haven't actually called it.
Let's add a call to `main` (se the editor):

~~~ {.python}
main()
~~~

and run that:

~~~ {.input}
$ python picture.py mandel-big-2.csv
[...another beautiful map...]
~~~

> ## The Right Way to Do It {.callout}
>
> If our programs can take complex parameters or multiple filenames,
> we shouldn't handle `sys.argv` directly.
> Instead,
> we should use Python's `argparse` library,
> which handles common cases in a systematic way,
> and also makes it easy for us to provide sensible error messages for our users.

## Handling Multiple Files

The next step is to teach our program how to handle multiple files. Let's use the ones that are called "mandel-big-*.csv":

~~~ {.input}
$ ls mandel-big-*.csv
~~~
~~~ {.output}
mandel-big-0.csv  mandel-big-2.csv  mandel-big-4.csv
mandel-big-1.csv  mandel-big-3.csv  mandel-big-5.csv
~~~

We want our program to process each file separately, so we need a loop that executes once for each filename.
If we specify the files on the command line, the filenames will be in `sys.argv`,
but we need to be careful:
`sys.argv[0]` will always be the name of our script,
rather than the name of a file.
We also need to handle an unknown number of filenames,
since our program could be run for any number of files.

The solution to both problems is to loop over the contents of `sys.argv[1:]`.
The '1' tells Python to start the slice at location 1,
so the program's name isn't included;
since we've left off the upper bound,
the slice runs to the end of the list,
and includes all the filenames.
Here's our changed program
`multipic.py`:

~~~ {.input}
$ cat multipic.py
~~~

~~~ {.python}
import sys
import numpy

def main():
    script = sys.argv[0]
    for filename in sys.argv[1:]:
        data = numpy.loadtxt(filename, delimiter=',')
        for m in data.mean(axis=1):
            print(m)

main()
~~~

and here it is in action:

~~~ {.input}
python3 multipic.py mandel-big-1.csv mandel-big-2.csv
[...two beautiful maps in a row...]
~~~

## Handling Standard Input

The next thing our program has to do is read data from standard input if no filenames are given
so that we can put it in a pipeline, redirect input to it, and so on.
Let's experiment in another script called `count-stdin.py`:

~~~ {.input}
$ cat count-stdin.py
~~~

~~~ {.python}
import sys

count = 0
for line in sys.stdin:
    count += 1

print(count, 'lines in standard input')
~~~

This little program reads lines from a special "file" called `sys.stdin`,
which is automatically connected to the program's standard input.
We don't have to open it --- Python and the operating system
take care of that when the program starts up ---
but we can do almost anything with it that we could do to a regular file.
Let's try running it as if it were a regular command-line program:

~~~ {.input}
$ python3 count-stdin.py <mandel-small.csv
~~~

~~~ {.output}
20 lines in standard input
~~~

A common mistake is to try to run something that reads from standard input like this:

~~~ {.input}
$ python3 count-stdin.py mandel-small.csv
~~~

i.e., to forget the `<` character that redirect the file to standard input.
In this case,
there's nothing in standard input,
so the program waits at the start of the loop for someone to type something on the keyboard.
Since there's no way for us to do this,
our program is stuck,
and we have to halt it using the `Interrupt` option from the `Kernel` menu in the Notebook.

~~~ {.error}
Traceback (most recent call last):
  File "count-stdin.py", line 4, in <module>
    for line in sys.stdin:
KeyboardInterrupt
~~~

## Back to the Mandelbrot Set:

Since until now we worked with the interpreter rather than, we haven't really kept track of things that we added to our Mandelbrot program. 
In order to avoid having to retype the whole thing, we're cheating a bit. We've hidden a complete version of the program as a file ".mandel.py" in the mandel directory, so let's copy that:

~~~ {.bash}
cp .mandel.py mandel.py
~~~

then load that into our favourite editor:

~~~ {.python}
#! /usr/bin/env python3

import cmath
import matplotlib.pyplot as plt
import numpy as np
import sys

nr=   int(sys.argv[1])
ni=   int(sys.argv[2])
rei=float(sys.argv[3])
ref=float(sys.argv[4])
imi=float(sys.argv[5])
imf=float(sys.argv[6])
imax= int(sys.argv[7])
dr=(ref-rei)/nr
di=(imf-imi)/ni

def point (ir,ii):
    rl=ir*dr+rei
    im=ii*di+imi
    return rl+im*1j

def mandit (b):
    z=0.0
    for it in range(1,imax+1):
        z=z*z+b
        if (abs(z)>2.0):
            break
    return it

manrow=np.zeros(nr)
mandel=np.zeros((nr,ni))

for iindex in range(0,ni):
    for rindex in range(0,nr):
        b=point(rindex,iindex)
        manrow[rindex]=mandit(b)
    mandel[iindex][:]=manrow[:]

image=plt.imshow(mandel)
plt.show(image)
~~~

Everything that we have done is right here:

* The first 4 lines load in the packages we need: numpy, matplotlib, complex math, and sys
* Next we're loading all the parameters we need to keep our plot flexible:
** The number of pixels in the real and imaginary direction (nr and ni, corresponding to the first and secon command line argument)
** The intitial and final values of the real part (rei, ref) and of the imaginary part (imi,imf) make up argument 3-6
** Finally, argument 7 is the "iteration depth" imax, the value when we stop trying
* We have two functions:
** point() converts "pixel numbers" in real and imaginary direction into a complex number
** mandit() does iterations on a complex number until 2 is exceeded or the cutoff is reached, then returns the number of iterations
* We use two data structures:
** manrow contains all the results from a single row with a fixed imaginary part
** mandel contains all the results on the segment of the complex plane we're looking at
* Two nested loops (over iindex and rindex) scan the whole segment and contain just calls to point() and madit()
* Two more lines at the end produce and image and display it

One more addition has been made: We put a "hash-bang" line 

~~~ {.python}
#! /usr/bin/env python3
~~~

at the top. This makes it easier to call the program, as it has the python3 call already built in and we can execute it directly, for instance

~~~ {.python}
$ mandel.py 200 200 -1.5 0.5 -1.0 1.0 1000
~~~

for a quick look at the whole set. Play around with it a little and experiment with different input values. This afternoon we're going to parallelize this.

> ## Arithmetic on the command line {.challenge}
>
> Write a command-line program that does addition and subtraction:
>
> ~~~ {.python}
> $ python arith.py add 1 2
> ~~~
> ~~~ {.output}
> 3
> ~~~
> ~~~ {.python}
> $ python arith.py subtract 3 4
> ~~~
> ~~~ {.output}
> -1
> ~~~
>

> ## Finding particular files {.challenge}
>
> Using the `glob` module introduced [earlier](04-files.html),
> write a simple version of `ls` that shows files in the current directory with a particular suffix.
> A call to this script should look like this:
>
> ~~~ {.python}
> $ python my_ls.py py
> ~~~
> ~~~ {.output}
> left.py
> right.py
> zero.py
> ~~~

