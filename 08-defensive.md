---
layout: page
title: Programming with Python
subtitle: Defensive Programming
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> * Explain what an assertion is.
> * Add assertions that check the program's state is correct.
> * Correctly add precondition and postcondition assertions to functions.
> * Explain what test-driven development is, and use it when creating new functions.
> * Explain why variables should be initialized using actual data values rather than arbitrary constants.

Our previous lessons have introduced the basic tools of programming:
variables and lists, file I/O, loops, conditionals, and functions.
What they *haven't* done is show us how to tell whether a program is
getting the right answer, and how to tell if it's *still* getting the
right answer as we make changes to it.

To achieve that,
we need to:

*   Write programs that check their own operation.
*   Write and run tests for widely-used functions.
*   Make sure we know what "correct" actually means.

The good news is, doing these things will speed up our programming,
not slow it down.  The time saved by measuring carefully before
deploying the software wood is much greater than the time that testing
takes.

## Assertions

The first step toward getting the right answers from our programs is
to assume that mistakes will happen and to guard against them. This is
called defensive programming, and the most common way to do it is to
add assertions to our code so that it checks itself as it runs. An
assertion is simply a statement that something must be true at a
certain point in a program. When Python sees one, it evaluates the
assertion’s condition. If it’s true, Python does nothing, but if it’s
false, Python halts the program immediately and prints the error
message if one is provided.

For example, in our "Mandelbrot" program, we will have to specify how
many points (pixels) we are going to compute in the real and imaginary
direction on the complex plane. These numbers obviously have to be
positive integers:

~~~ {.python}
nr=200
ni=-200.0
assert nr>0 and ni>0, "One of the number of points is zero or negative."
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError: One of the number of points is zero or negative.
assert type(nr)==type(1) and type(ni)==type(1), "One of the numbers of points is not of integer type."
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError: One of the numbers of points is not of integer type.
~~~

Programs like the Firefox browser are full of assertions: 10-20% of
the code they contain are there to check that the other 80-90% are
working correctly.  Broadly speaking, assertions fall into three
categories:

*   A [precondition](reference.html#precondition) is something that must be true at the start of a function in order for it to work correctly.

*   A [postcondition](reference.html#postcondition) is something that the function guarantees is true when it finishes.

*   An [invariant](reference.html#invariant) is something that is always true at a particular point inside a piece of code.

For example, for our Mandelbrot example, we want to keep the
boundaries of the complex numbers flexible. We can do this by letting
the user specify the smallest and largest real and imaginary part of
the numbers, respectively. We then have to ensure that the minimum is
always smaller than the maximum. This is a precondition.

From the number of points and minimum and maximum real and imaginary
parts we can compute values for the increase in either direction,
i.e. the pixel size. These should be positive values, which is a
post-condition for this code segment:

~~~ {.python}
nr=200
ni=200
rei=-1.5
ref=0.5
imi=-1.0
imf=1.0
assert rei < ref, 'Initial real part must be smaller than final.'
assert imi < imf, 'Initial imaginary part must be smaller than final.'
dr=(ref-rei)/nr
di=(imi-imf)/ni
assert dr>0 and di>0, 'Negative pixelsize.'
~~~
~~~ {.error}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError: Negative pixelsize.
~~~

The post-conditions help us catch bugs by telling us when our
calculations cannot have been correct. In this case the input seems
fine, so there must be something wrong in the code that causes the
asserion of the post condition to fail. What?

But assertions aren’t just about catching errors: they also help
people understand programs. Each assertion gives the person reading
the program a chance to check (consciously or otherwise) that their
understanding matches what the code is doing.

Most good programmers follow two rules when adding assertions to their
code:

* The first is, fail early, fail often. The greater the distance
between when and where an error occurs and when it’s noticed, the
harder the error will be to debug, so good code catches mistakes as
early as possible.

* The second rule is, turn bugs into assertions or tests. Whenever you
fix a bug, write an assertion that catches the mistake should you make
it again. If you made a mistake in a piece of code, the odds are good
that you have made other mistakes nearby, or will make the same
mistake (or a related one) the next time you change it. Writing
assertions to check that you haven’t regressed (i.e., haven’t
re-introduced an old problem) can save a lot of time in the long run,
and helps to warn people who are reading the code (including your
future self) that this bit is tricky.

## Test-Driven Development

An assertion checks that something is true at a particular point in
the program.  The next step is to check the overall behavior of a
piece of code, i.e., to make sure that it produces the right output
when it's given a particular input.  For example, suppose we need to
find where two or more time series overlap.  The range of each time
series is represented as a pair of numbers, which are the time the
interval started and ended.  The output is the largest range that they
all include:

![Overlapping Ranges](fig/python-overlapping-ranges.svg)\

Most novice programmers would solve this problem like this:

1.  Write a function `range_overlap`.
2.  Call it interactively on two or three different inputs.
3.  If it produces the wrong answer, fix the function and re-run that test.

This clearly works --- after all, thousands of scientists are doing it
right now --- but there's a better way:

1.  Write a short function for each test.
2.  Write a `range_overlap` function that should pass those tests.
3.  If `range_overlap` produces any wrong answers, fix it and re-run the test functions.

Writing the tests *before* writing the function they exercise is
called [test-driven development](reference.html#test-driven-development) (TDD).  Its
advocates believe it produces better code faster because:

1.  If people write tests after writing the thing to be tested,
    they are subject to confirmation bias,
    i.e.,
    they subconsciously write tests to show that their code is correct,
    rather than to find errors.
2.  Writing tests helps programmers figure out what the function is actually supposed to do.

Here are three test functions for `range_overlap`:

~~~ {.python}
assert range_overlap([ (0.0, 1.0) ]) == (0.0, 1.0)
assert range_overlap([ (2.0, 3.0), (2.0, 4.0) ]) == (2.0, 3.0)
assert range_overlap([ (0.0, 1.0), (0.0, 2.0), (-1.0, 1.0) ]) == (0.0, 1.0)
~~~
~~~ {.error}
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
<ipython-input-25-d8be150fbef6> in <module>()
----> 1 assert range_overlap([ (0.0, 1.0) ]) == (0.0, 1.0)
      2 assert range_overlap([ (2.0, 3.0), (2.0, 4.0) ]) == (2.0, 3.0)
      3 assert range_overlap([ (0.0, 1.0), (0.0, 2.0), (-1.0, 1.0) ]) == (0.0, 1.0)

AssertionError:
~~~

The error is actually reassuring:
we haven't written `range_overlap` yet,
so if the tests passed,
it would be a sign that someone else had
and that we were accidentally using their function.

And as a bonus of writing these tests,
we've implicitly defined what our input and output look like:
we expect a list of pairs as input,
and produce a single pair as output.

Something important is missing, though.
We don't have any tests for the case where the ranges don't overlap at all:

~~~ {.python}
assert range_overlap([ (0.0, 1.0), (5.0, 6.0) ]) == ???
~~~

What should `range_overlap` do in this case:
fail with an error message,
produce a special value like `(0.0, 0.0)` to signal that there's no overlap,
or something else?
Any actual implementation of the function will do one of these things;
writing the tests first helps us figure out which is best
*before* we're emotionally invested in whatever we happened to write
before we realized there was an issue.

And what about this case?

~~~ {.python}
assert range_overlap([ (0.0, 1.0), (1.0, 2.0) ]) == ???
~~~

Do two segments that touch at their endpoints overlap or not?
Mathematicians usually say "yes",
but engineers usually say "no".
The best answer is "whatever is most useful in the rest of our program",
but again,
any actual implementation of `range_overlap` is going to do *something*,
and whatever it is ought to be consistent with what it does when there's no overlap at all.

Since we're planning to use the range this function returns
as the X axis in a time series chart,
we decide that:

1.  every overlap has to have non-zero width, and
2.  we will return the special value `None` when there's no overlap.

`None` is built into Python,
and means "nothing here".
(Other languages often call the equivalent value `null` or `nil`).
With that decision made,
we can finish writing our last two tests:

~~~ {.python}
assert range_overlap([ (0.0, 1.0), (5.0, 6.0) ]) == None
assert range_overlap([ (0.0, 1.0), (1.0, 2.0) ]) == None
~~~
~~~ {.error}
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
<ipython-input-26-d877ef460ba2> in <module>()
----> 1 assert range_overlap([ (0.0, 1.0), (5.0, 6.0) ]) == None
      2 assert range_overlap([ (0.0, 1.0), (1.0, 2.0) ]) == None

AssertionError:
~~~

Again,
we get an error because we haven't written our function,
but we're now ready to do so:

~~~ {.python}
def range_overlap(ranges):
    '''Return common overlap among a set of [low, high] ranges.'''
    lowest = 0.0
    highest = 1.0
    for (low, high) in ranges:
        lowest = max(lowest, low)
        highest = min(highest, high)
    return (lowest, highest)
~~~

(Take a moment to think about why we use `max` to raise `lowest`
and `min` to lower `highest`).
We'd now like to re-run our tests,
but they're scattered across three different cells.
To make running them easier,
let's put them all in a function:

~~~ {.python}
def test_range_overlap():
    assert range_overlap([ (0.0, 1.0), (5.0, 6.0) ]) == None
    assert range_overlap([ (0.0, 1.0), (1.0, 2.0) ]) == None
    assert range_overlap([ (0.0, 1.0) ]) == (0.0, 1.0)
    assert range_overlap([ (2.0, 3.0), (2.0, 4.0) ]) == (2.0, 3.0)
    assert range_overlap([ (0.0, 1.0), (0.0, 2.0), (-1.0, 1.0) ]) == (0.0, 1.0)
~~~

We can now test `range_overlap` with a single function call:

~~~ {.python}
test_range_overlap()
~~~
~~~ {.error}
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
<ipython-input-29-cf9215c96457> in <module>()
----> 1 test_range_overlap()

<ipython-input-28-5d4cd6fd41d9> in test_range_overlap()
      1 def test_range_overlap():
----> 2     assert range_overlap([ (0.0, 1.0), (5.0, 6.0) ]) == None
      3     assert range_overlap([ (0.0, 1.0), (1.0, 2.0) ]) == None
      4     assert range_overlap([ (0.0, 1.0) ]) == (0.0, 1.0)
      5     assert range_overlap([ (2.0, 3.0), (2.0, 4.0) ]) == (2.0, 3.0)

AssertionError:
~~~

The first test that was supposed to produce `None` fails, so we know
something is wrong with our function.  We *don't* know whether the
other tests passed or failed because Python halted the program as soon
as it spotted the first error.  Still, some information is better than
none, and if we trace the behavior of the function with that input, we
realize that we're initializing `lowest` and `highest` to 0.0 and 1.0
respectively, regardless of the input values.  This violates another
important rule of programming: *always initialize from data*.

> ## Pre- and post-conditions {.challenge}
>
> Suppose you are writing a function called `average` that calculates the average of the numbers in a list.
> What pre-conditions and post-conditions would you write for it?
> Compare your answer to your neighbor's:
> can you think of a function that will pass your tests but not hers or vice versa?

> ## Testing assertions {.challenge}
>
> Given a sequence of values, the function `running` returns
> a list containing the running totals at each index.
>
> ~~~{.python}
> running([1, 2, 3, 4])
> ~~~
>
> ~~~{.output}
> [1, 3, 6, 10]
> ~~~
>
> ~~~{.python}
> running('abc')
> ~~~
>
> ~~~{.output}
> ['a', 'ab', 'abc']
> ~~~
>
> Explain in words what the assertions in this function check,
> and for each one,
> give an example of input that will make that assertion fail.
>
> ~~~ {.python}
> def running(values):
>     assert len(values) > 0
>     result = [values[0]]
>     for v in values[1:]:
>         assert result[-1] >= 0
>         result.append(result[-1] + v)
>         assert result[-1] >= result[0]
>     return result
> ~~~

> ## Fixing and testing {.challenge}
>
> Fix `range_overlap`. Re-run `test_range_overlap` after each change you make.
