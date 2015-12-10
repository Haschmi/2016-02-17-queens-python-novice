---
layout: page
title: Programming with Python
subtitle: Errors and Exceptions
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   To be able to read a traceback, and determine the following relevant pieces of information:
>     * The file, function, and line number on which the error occurred
>     * The type of the error
>     * The error message
> *   To be able to describe the types of situations in which the following errors occur:
>     * `SyntaxError` and `IndentationError`
>     * `NameError`
>     * `IndexError`
>     * `FileNotFoundError`

Every programmer encounters errors, both those who are just beginning,
and those who have been programming for years.  Encountering errors
and exceptions can be very frustrating at times, and can make coding
feel like a hopeless endeavour.  However, understanding what the
different types of errors are and when you are likely to encounter
them can help a lot.  Once you know *why* you get certain types of
errors, they become much easier to fix.

Errors in Python have a very specific form, called a
[traceback](reference.html#traceback).  Let's examine one:

~~~ {.python}
import stooges
stooges.stooges()
~~~
~~~ {.output}
larry
curly
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "./stooges.py", line 5, in stooges
    print(stooges[3])
IndexError: list index out of range
~~~

This particular traceback has two levels:

The first (line 1) refers to the stooges.stooges() call (not very
clear, I'm afraid), the second points to code in another function
(stooges, located in the file stooges.py), with an arrow pointing to
Line 5 (which is print(stooges[3])).

The last level is the actual place where the error occurred. The other
level(s) show what function the program executed to get to the next
level down. So, in this case, the program first performed a function
call to the function stooges(). Inside this function, the program
encountered an error on Line 5, when it tried to run the code
print(stooges[3]).

Sometimes, you might see a traceback that is very long – sometimes
they might even be 20 levels deep! This can make it seem like
something horrible happened, but really it just means that your
program called many functions before it ran into the error. Most of
the time, you can just pay attention to the bottom-most level, which
is the actual place where the error occurred.

So what error did the program actually encounter? In the last line of
the traceback, Python helpfully tells us the category or type of error
(in this case, it is an IndexError) and a more detailed error message
(in this case, it says “list index out of range”).

If you encounter an error and don’t know what it means, it is still
important to read the traceback closely. That way, if you fix the
error, but encounter a new one, you can tell that the error
changed. Additionally, sometimes just knowing where the error occurred
is enough to fix it, even if you don’t entirely understand the
message.

If you do encounter an error you don’t recognize, try looking at the
official documentation on errors. However, note that you may not
always be able to find the error there, as it is possible to create
custom errors. In that case, hopefully the custom error message is
informative enough to help you figure out what went wrong.

## Syntax Errors

When you forget a colon at the end of a line, accidentally add one
space too many when indenting under an `if` statement, or forget a
parentheses, you will encounter a [syntax
error](reference.html#syntax-error).  This means that Python couldn't
figure out how to read your program.  This is similar to forgetting
punctuation in English:

People can typically figure out what is meant by text with no
punctuation, but people are much smarter than computers.  If Python
doesn't know how to read the program, it will just give up and inform
you with an error.  For example:

~~~ {.python}
def some_function()
~~~
~~~ {.error}
  File "<stdin>", line 1
    def some_function()
                      ^
SyntaxError: invalid syntax
~~~

Here, Python tells us that there is a `SyntaxError` on line 1, and
even puts a little arrow in the place where there is an issue.  In
this case the problem is that the function definition is missing a
colon at the end.

Another common class of Syntax errors has to do with indentation:

~~~ {.python}
def some_function():
    msg="Hellow World!"
    print(msg)
     return(msg)

~~~
~~~ {.error}
  File "<stdin>", line 4
    return(msg)
    ^
IndentationError: unexpected indent
~~~

Both `SyntaxError` and `IndentationError` indicate a problem with the
syntax of your program, but an `IndentationError` is more specific: it
*always* means that there is a problem with how your code is indented.

## Variable Name Errors

Another very common type of error is called a `NameError`, and occurs
when you try to use a variable that does not exist.  For example:

~~~ {.python}
print(newname)
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'newname' is not defined
~~~

Variable name errors come with some of the most informative error
messages, which are usually of the form "name 'variable_name' is
not defined".

Why does this error message occur?  That's harder question to answer,
because it depends on what your code is supposed to do.  However,
there are a few very common reasons why you might have an undefined
variable.  The first is that you meant to use a
[string](reference.html#string), but forgot to put quotes around it:

~~~ {.python}
print(hello)
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'hello' is not defined
~~~

The second is that you just forgot to create the variable before using
it.  In the following example, `c` should have been defined or
initialized (e.g., with `c=0`) before the for loop:

~~~ {.python}
for n in range(10):
    c=c+n
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: name 'c' is not defined
~~~

Finally, you could have made a typo when you were writing your
code. Let’s say we fixed the error above by adding the line C=0 before
the for loop. Frustratingly, this actually does not fix the
error. Remember that variables are case-sensitive, so the variable c
is different from C. We still get the same error, because we still
have not defined c:

~~~ {.python}
C=0
for n in range(10):
    c=c+n
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: name 'c' is not defined
~~~

## Item Errors

Next up are errors having to do with containers (like lists and
dictionaries) and the items within them. If you try to access an item
in a list or a dictionary that does not exist, then you will get an
error:

~~~ {.python}
stooges = ['Moe', 'Larry', 'Curly']
print("Stooge #1 is " + stooges[1])
~~~
~~~ {.output}
Stooge #1 is Larry
~~~
~~~ {.python}
print("Stooge #2 is " + stooges[2])
~~~
~~~ {.output}
Stooge #2 is Curly
~~~
~~~ {.python}
print("Stooge #3 is " + stooges[3])
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
~~~

Remember, the Python Stooges are labelled 0-2 not 1-3. Python is
telling us that there is an IndexError in our code, meaning we tried
to access a list index that did not exist.

## File Errors

The last type of error we'll cover today are those associated with reading and writing files: `FileNotFoundError`.
If you try to read a file that does not exist,
you will recieve an `FileNotFoundError` telling you so.

~~~ {.python}
file_handle = open('myfile.txt', 'r')
~~~
~~~ {.error}
---------------------------------------------------------------------------
FileNotFoundError                         Traceback (most recent call last)
<ipython-input-14-f6e1ac4aee96> in <module>()
----> 1 file_handle = open('myfile.txt', 'r')

FileNotFoundError: [Errno 2] No such file or directory: 'myfile.txt'
~~~

One reason for receiving this error is that you specified an incorrect path to the file.
For example,
if I am currently in a folder called `myproject`,
and I have a file in `myproject/writing/myfile.txt`,
but I try to just open `myfile.txt`,
this will fail.
The correct path would be `writing/myfile.
xt`. It is also possible (like with `NameError`) that you just made a typo.

Another issue could be that you used the "read" flag instead of the "write" flag.
Python will not give you an error if you try to open a file for writing when the file does not exist.
However,
if you meant to open a file for reading,
but accidentally opened it for writing,
and then try to read from it,
you will get an `UnsupportedOperation` error
telling you that the file was not opened for reading:

~~~ {.python}
file_handle = open('myfile.txt', 'w')
file_handle.read()
~~~
~~~ {.error}
---------------------------------------------------------------------------
UnsupportedOperation                      Traceback (most recent call last)
<ipython-input-15-b846479bc61f> in <module>()
      1 file_handle = open('myfile.txt', 'w')
----> 2 file_handle.read()

UnsupportedOperation: not readable
~~~

These are the most common errors with files,
though many others exist.
If you get an error that you've never seen before,
searching the Internet for that error type
often reveals common reasons why you might get that error.

> ## Reading Error Messages {.challenge}
>
> Read the traceback below, and identify the following pieces of information about it:
>
> 1.  How many levels does the traceback have?
> 2.  What is the file name where the error occurred?
> 3.  What is the function name where the error occurred?
> 4.  On which line number in this function did the error occurr?
> 5.  What is the type of error?
> 6.  What is the error message?
>
> ~~~ {.python}
> import errors_02
> errors_02.print_friday_message()
> ~~~
> ~~~ {.error}
> ---------------------------------------------------------------------------
> KeyError                                  Traceback (most recent call last)
> <ipython-input-2-e4c4cbafeeb5> in <module>()
>       1 import errors_02
> ----> 2 errors_02.print_friday_message()
>
> /Users/jhamrick/project/swc/novice/python/errors_02.py in print_friday_message()
>      13
>      14 def print_friday_message():
> ---> 15     print_message("Friday")
>
> /Users/jhamrick/project/swc/novice/python/errors_02.py in print_message(day)
>       9         "sunday": "Aw, the weekend is almost over."
>      10     }
> ---> 11     print(messages[day])
>      12
>      13
>
> KeyError: 'Friday'
> ~~~

> ## Identifying Syntax Errors {.challenge}
>
> 1. Read the code below, and (without running it) try to identify what the errors are.
> 2. Run the code, and read the error message. Is it a `SyntaxError` or an `IndentationError`?
> 3. Fix the error.
> 4. Repeat steps 2 and 3, until you have fixed all the errors.
>
> ~~~ {.python}
> def another_function
>   print("Syntax errors are annoying.")
>    print("But at least python tells us about them!")
>   print("So they are usually not too hard to fix.")
> ~~~

> ## Identifying Variable Name Errors {.challenge}
>
> 1. Read the code below, and (without running it) try to identify what the errors are.
> 2. Run the code, and read the error message. What type of `NameError` do you think this is? In other words, is it a string with no quotes, a misspelled variable, or a variable that should have been defined but was not?
> 3. Fix the error.
> 4. Repeat steps 2 and 3, until you have fixed all the errors.
>
> ~~~ {.python}
> for number in range(10):
>     # use a if the number is a multiple of 3, otherwise use b
>     if (Number % 3) == 0:
>         message = message + a
>     else:
>         message = message + "b"
> print(message)
> ~~~

> ## Identifying Item Errors {.challenge}
>
> 1. Read the code below, and (without running it) try to identify what the errors are.
> 2. Run the code, and read the error message. What type of error is it?
> 3. Fix the error.
>
> ~~~ {.python}
> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
> print('My favorite season is ', seasons[4])
> ~~~
