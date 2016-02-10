---
layout: page
title: Programming with Python
subtitle: Analyzing Data from Multiple Files
minutes: 20
---
> ## Learning Objectives {.objectives}
>
> *   Use a library function to get a list of filenames that match a simple wildcard pattern.
> *   Use a for loop to process multiple files.

We can use our skills with loops to process multiple files in a
directory, for instance by visualizing them.  The only thing that's
missing for that is a library with a rather unfortunate name:

~~~ {.python}
import glob
~~~

The `glob` library contains a single function, also called `glob`,
that finds files whose names match a pattern.  We provide those
patterns as strings: the character `*` matches zero or more
characters, while `?` matches any one character.  We can use this to
get the names of all the csv files that start with "mandel-big-" in the current directory:

~~~ {.python}
print(glob.glob('mandel-big-?.html'))
~~~

~~~ {.output}
['mandel-big-2.csv', 'mandel-big-5.csv', 'mandel-big-4.csv', 'mandel-big-3.csv', 'mandel-big-0.csv', 'mandel-big-1.csv']
~~~

As these examples show, `glob.glob`'s result is a list of strings,
which means we can loop over it to do something with each filename in
turn.  In our case, the "something" we want to do generate a 
plot for each of our Mandelbrot datasets.  Let's test it by
analyzing the first three files in the list:

~~~ {.python}
import numpy as np
import matplotlib.pyplot as plt
import glob

filenames = glob.glob('mandel-big-?.csv')
for f in filenames:
    print(f)
    data = np.loadtxt(fname=f, delimiter=',')
    image=plt.imshow(data)
    plt.show(image)
~~~

~~~ {.output}
mandel-big-2.csv [...shows picture...]
mandel-big-5.csv [...shows picture...]
mandel-big-4.csv [...shows picture...]
mandel-big-3.csv [...shows picture...]
mandel-big-0.csv [...shows picture...]
mandel-big-1.csv [...shows picture...]
~~~

Sure enough, this is a nice way to create a "slide-show" of the data files. The only thing that is not quite right here is the order in which the files get put into the "filenames" list. But there is a fix for that: when you read it in, use the function sorted() to do the job:

~~~ (.python)
filenames = sorted(glob.glob('mandel-big-?.csv')
print(filenames)
~~~

~~~ (.output)
['mandel-big-0.csv', 'mandel-big-1.csv', 'mandel-big-2.csv', 'mandel-big-3.csv', 'mandel-big-4.csv', 'mandel-big-5.csv']
~~~

Using the sorted filenames in the loop should produce the pictures in a sensible order.
