
# Functions

In the context of programming, a <span>**function**</span> is a named
sequence of statements that performs a computation. When you define a
function, you specify the name and the sequence of statements. Later,
you can “call” the function by name.

## Function calls

We have already seen one example of a <span>**function call**</span>:

    >>> type(42)
    <class 'int'>

The name of the function is <span>`type`</span>. The expression in
parentheses is called the <span>**argument**</span> of the function. The
result, for this function, is the type of the argument.

It is common to say that a function “takes” an argument and “returns” a
result. The result is also called the <span>**return value**</span>.

Python provides functions that convert values from one type to another.
The <span>`int`</span> function takes any value and converts it to an
integer, if it can, or complains otherwise:

    >>> int('32')
    32
    >>> int('Hello')
    ValueError: invalid literal for int(): Hello

<span>`int`</span> can convert floating-point values to integers, but it
doesn’t round off; it chops off the fraction part:

    >>> int(3.99999)
    3
    >>> int(-2.3)
    -2

<span>`float`</span> converts integers and strings to floating-point
numbers:

    >>> float(32)
    32.0
    >>> float('3.14159')
    3.14159

Finally, <span>`str`</span> converts its argument to a string:

    >>> str(32)
    '32'
    >>> str(3.14159)
    '3.14159'

## Math functions

Python has a math module that provides most of the familiar mathematical
functions. A <span>**module**</span> is a file that contains a
collection of related functions.

Before we can use the functions in a module, we have to import it with
an <span>**import statement**</span>:

    >>> import math

This statement creates a <span>**module object**</span> named math. If
you display the module object, you get some information about it:

    >>> math
    <module 'math' (built-in)>

The module object contains the functions and variables defined in the
module. To access one of the functions, you have to specify the name of
the module and the name of the function, separated by a dot (also known
as a period). This format is called <span>**dot notation**</span>.

    >>> ratio = signal_power / noise_power
    >>> decibels = 10 * math.log10(ratio)
    
    >>> radians = 0.7
    >>> height = math.sin(radians)

The first example uses `math.log10` to compute a signal-to-noise ratio
in decibels (assuming that `signal_power` and `noise_power` are
defined). The math module also provides <span>`log`</span>, which
computes logarithms base <span>`e`</span>.

The second example finds the sine of <span>`radians`</span>. The
variable name <span>`radians`</span> is a hint that <span>`sin`</span>
and the other trigonometric functions (<span>`cos`</span>,
<span>`tan`</span>, etc.) take arguments in radians. To convert from
degrees to radians, divide by 180 and multiply by \(\pi\):

    >>> degrees = 45
    >>> radians = degrees / 180.0 * math.pi
    >>> math.sin(radians)
    0.707106781187

The expression <span>`math.pi`</span> gets the variable
<span>`pi`</span> from the math module. Its value is a floating-point
approximation of \(\pi\), accurate to about 15 digits.

If you know trigonometry, you can check the previous result by comparing
it to the square root of two, divided by two:

    >>> math.sqrt(2) / 2.0
    0.707106781187

## Composition

So far, we have looked at the elements of a program—variables,
expressions, and statements—in isolation, without talking about how to
combine them.

One of the most useful features of programming languages is their
ability to take small building blocks and <span>**compose**</span> them.
For example, the argument of a function can be any kind of expression,
including arithmetic operators:

    x = math.sin(degrees / 360.0 * 2 * math.pi)

And even function calls:

    x = math.exp(math.log(x+1))

Almost anywhere you can put a value, you can put an arbitrary
expression, with one exception: the left side of an assignment statement
has to be a variable name. Any other expression on the left side is a
syntax error (we will see exceptions to this rule later).

    >>> minutes = hours * 60                 # right
    >>> hours * 60 = minutes                 # wrong!
    SyntaxError: can't assign to operator

## Adding new functions

So far, we have only been using the functions that come with Python, but
it is also possible to add new functions. A <span>**function
definition**</span> specifies the name of a new function and the
sequence of statements that run when the function is called.

Here is an example:

    def print_lyrics():
        print("I'm a lumberjack, and I'm okay.")
        print("I sleep all night and I work all day.")

<span>`def`</span> is a keyword that indicates that this is a function
definition. The name of the function is `print_lyrics`. The rules for
function names are the same as for variable names: letters, numbers and
underscore are legal, but the first character can’t be a number. You
can’t use a keyword as the name of a function, and you should avoid
having a variable and a function with the same name.

The empty parentheses after the name indicate that this function doesn’t
take any arguments.

The first line of the function definition is called the
<span>**header**</span>; the rest is called the <span>**body**</span>.
The header has to end with a colon and the body has to be indented. By
convention, indentation is always four spaces. The body can contain any
number of statements.

The strings in the print statements are enclosed in double quotes.
Single quotes and double quotes do the same thing; most people use
single quotes except in cases like this where a single quote (which is
also an apostrophe) appears in the string.

All quotation marks (single and double) must be “straight quotes”,
usually located next to Enter on the keyboard. “Curly quotes”, like the
ones in this sentence, are not legal in Python.

If you type a function definition in interactive mode, the interpreter
prints dots (<span>`...`</span>) to let you know that the definition
isn’t complete:

    >>> def print_lyrics():
    ...     print("I'm a lumberjack, and I'm okay.")
    ...     print("I sleep all night and I work all day.")
    ...

To end the function, you have to enter an empty line.

Defining a function creates a <span>**function object**</span>, which
has type `function`:

    >>> print(print_lyrics)
    <function print_lyrics at 0xb7e99e9c>
    >>> type(print_lyrics)
    <class 'function'>

The syntax for calling the new function is the same as for built-in
functions:

    >>> print_lyrics()
    I'm a lumberjack, and I'm okay.
    I sleep all night and I work all day.

Once you have defined a function, you can use it inside another
function. For example, to repeat the previous refrain, we could write a
function called `repeat_lyrics`:

    def repeat_lyrics():
        print_lyrics()
        print_lyrics()

And then call `repeat_lyrics`:

    >>> repeat_lyrics()
    I'm a lumberjack, and I'm okay.
    I sleep all night and I work all day.
    I'm a lumberjack, and I'm okay.
    I sleep all night and I work all day.

But that’s not really how the song goes.

## Definitions and uses

Pulling together the code fragments from the previous section, the whole
program looks like this:

    def print_lyrics():
        print("I'm a lumberjack, and I'm okay.")
        print("I sleep all night and I work all day.")
    
    def repeat_lyrics():
        print_lyrics()
        print_lyrics()
    
    repeat_lyrics()

This program contains two function definitions: `print_lyrics` and
`repeat_lyrics`. Function definitions get executed just like other
statements, but the effect is to create function objects. The statements
inside the function do not run until the function is called, and the
function definition generates no output.

As you might expect, you have to create a function before you can run
it. In other words, the function definition has to run before the
function gets called.

As an exercise, move the last line of this program to the top, so the
function call appears before the definitions. Run the program and see
what error message you get.

Now move the function call back to the bottom and move the definition of
`print_lyrics` after the definition of `repeat_lyrics`. What happens
when you run this program?

## Flow of execution

To ensure that a function is defined before its first use, you have to
know the order statements run in, which is called the <span>**flow of
execution**</span>.

Execution always begins at the first statement of the program.
Statements are run one at a time, in order from top to bottom.

Function definitions do not alter the flow of execution of the program,
but remember that statements inside the function don’t run until the
function is called.

A function call is like a detour in the flow of execution. Instead of
going to the next statement, the flow jumps to the body of the function,
runs the statements there, and then comes back to pick up where it left
off.

That sounds simple enough, until you remember that one function can call
another. While in the middle of one function, the program might have to
run the statements in another function. Then, while running that new
function, the program might have to run yet another function\!

Fortunately, Python is good at keeping track of where it is, so each
time a function completes, the program picks up where it left off in the
function that called it. When it gets to the end of the program, it
terminates.

In summary, when you read a program, you don’t always want to read from
top to bottom. Sometimes it makes more sense if you follow the flow of
execution.

## Parameters and arguments

Some of the functions we have seen require arguments. For example, when
you call <span>`math.sin`</span> you pass a number as an argument. Some
functions take more than one argument: <span>`math.pow`</span> takes
two, the base and the exponent.

Inside the function, the arguments are assigned to variables called
<span>**parameters**</span>. Here is a definition for a function that
takes an argument:

    def print_twice(bruce):
        print(bruce)
        print(bruce)

This function assigns the argument to a parameter named
<span>`bruce`</span>. When the function is called, it prints the value
of the parameter (whatever it is) twice.

This function works with any value that can be printed.

    >>> print_twice('Spam')
    Spam
    Spam
    >>> print_twice(42)
    42
    42
    >>> print_twice(math.pi)
    3.14159265359
    3.14159265359

The same rules of composition that apply to built-in functions also
apply to programmer-defined functions, so we can use any kind of
expression as an argument for `print_twice`:

    >>> print_twice('Spam '*4)
    Spam Spam Spam Spam
    Spam Spam Spam Spam
    >>> print_twice(math.cos(math.pi))
    -1.0
    -1.0

The argument is evaluated before the function is called, so in the
examples the expressions `'Spam '*4` and
<span>`math.cos(math.pi)`</span> are only evaluated once.

You can also use a variable as an argument:

    >>> michael = 'Eric, the half a bee.'
    >>> print_twice(michael)
    Eric, the half a bee.
    Eric, the half a bee.

The name of the variable we pass as an argument (<span>`michael`</span>)
has nothing to do with the name of the parameter (<span>`bruce`</span>).
It doesn’t matter what the value was called back home (in the caller);
here in `print_twice`, we call everybody <span>`bruce`</span>.

## Variables and parameters are local

When you create a variable inside a function, it is
<span>**local**</span>, which means that it only exists inside the
function. For example:

    def cat_twice(part1, part2):
        cat = part1 + part2
        print_twice(cat)

This function takes two arguments, concatenates them, and prints the
result twice. Here is an example that uses it:

    >>> line1 = 'Bing tiddle '
    >>> line2 = 'tiddle bang.'
    >>> cat_twice(line1, line2)
    Bing tiddle tiddle bang.
    Bing tiddle tiddle bang.

When `cat_twice` terminates, the variable <span>`cat`</span> is
destroyed. If we try to print it, we get an exception:

    >>> print(cat)
    NameError: name 'cat' is not defined

Parameters are also local. For example, outside `print_twice`, there is
no such thing as <span>`bruce`</span>.

## Stack diagrams

To keep track of which variables can be used where, it is sometimes
useful to draw a <span>**stack diagram**</span>. Like state diagrams,
stack diagrams show the value of each variable, but they also show the
function each variable belongs to.

Each function is represented by a <span>**frame**</span>. A frame is a
box with the name of a function beside it and the parameters and
variables of the function inside it. The stack diagram for the previous
example is shown in Figure [4.1](#fig.stack).

![Stack diagram.](figs/stack.pdf)

The frames are arranged in a stack that indicates which function called
which, and so on. In this example, `print_twice` was called by
`cat_twice`, and `cat_twice` was called by `__main__`, which is a
special name for the topmost frame. When you create a variable outside
of any function, it belongs to `__main__`.

Each parameter refers to the same value as its corresponding argument.
So, <span>`part1`</span> has the same value as <span>`line1`</span>,
<span>`part2`</span> has the same value as <span>`line2`</span>, and
<span>`bruce`</span> has the same value as <span>`cat`</span>.

If an error occurs during a function call, Python prints the name of the
function, the name of the function that called it, and the name of the
function that called <span>*that*</span>, all the way back to
`__main__`.

For example, if you try to access <span>`cat`</span> from within
`print_twice`, you get a <span>`NameError`</span>:

    Traceback (innermost last):
      File "test.py", line 13, in __main__
        cat_twice(line1, line2)
      File "test.py", line 5, in cat_twice
        print_twice(cat)
      File "test.py", line 9, in print_twice
        print(cat)
    NameError: name 'cat' is not defined

This list of functions is called a <span>**traceback**</span>. It tells
you what program file the error occurred in, and what line, and what
functions were executing at the time. It also shows the line of code
that caused the error.

The order of the functions in the traceback is the same as the order of
the frames in the stack diagram. The function that is currently running
is at the bottom.

## Fruitful functions and void functions

Some of the functions we have used, such as the math functions, return
results; for lack of a better name, I call them <span>**fruitful
functions**</span>. Other functions, like `print_twice`, perform an
action but don’t return a value. They are called <span>**void
functions**</span>.

When you call a fruitful function, you almost always want to do
something with the result; for example, you might assign it to a
variable or use it as part of an expression:

    x = math.cos(radians)
    golden = (math.sqrt(5) + 1) / 2

When you call a function in interactive mode, Python displays the
result:

    >>> math.sqrt(5)
    2.2360679774997898

But in a script, if you call a fruitful function all by itself, the
return value is lost forever\!

    math.sqrt(5)

This script computes the square root of 5, but since it doesn’t store or
display the result, it is not very useful.

Void functions might display something on the screen or have some other
effect, but they don’t have a return value. If you assign the result to
a variable, you get a special value called <span>`None`</span>.

    >>> result = print_twice('Bing')
    Bing
    Bing
    >>> print(result)
    None

The value <span>`None`</span> is not the same as the string `'None'`. It
is a special value that has its own type:

    >>> type(None)
    <class 'NoneType'>

The functions we have written so far are all void. We will start writing
fruitful functions in a few chapters.

## Why functions?

It may not be clear why it is worth the trouble to divide a program into
functions. There are several reasons:

  - Creating a new function gives you an opportunity to name a group of
    statements, which makes your program easier to read and debug.

  - Functions can make a program smaller by eliminating repetitive code.
    Later, if you make a change, you only have to make it in one place.

  - Dividing a long program into functions allows you to debug the parts
    one at a time and then assemble them into a working whole.

  - Well-designed functions are often useful for many programs. Once you
    write and debug one, you can reuse it.

## Debugging

One of the most important skills you will acquire is debugging. Although
it can be frustrating, debugging is one of the most intellectually rich,
challenging, and interesting parts of programming.

In some ways debugging is like detective work. You are confronted with
clues and you have to infer the processes and events that led to the
results you see.

Debugging is also like an experimental science. Once you have an idea
about what is going wrong, you modify your program and try again. If
your hypothesis was correct, you can predict the result of the
modification, and you take a step closer to a working program. If your
hypothesis was wrong, you have to come up with a new one. As Sherlock
Holmes pointed out, “When you have eliminated the impossible, whatever
remains, however improbable, must be the truth.” (A. Conan Doyle,
<span>*The Sign of Four*</span>)

For some people, programming and debugging are the same thing. That is,
programming is the process of gradually debugging a program until it
does what you want. The idea is that you should start with a working
program and make small modifications, debugging them as you go.

For example, Linux is an operating system that contains millions of
lines of code, but it started out as a simple program Linus Torvalds
used to explore the Intel 80386 chip. According to Larry Greenfield,
“One of Linus’s earlier projects was a program that would switch
between printing AAAA and BBBB. This later evolved to Linux.”
(<span>*The Linux Users’ Guide*</span> Beta Version 1).

## Glossary

  - function:  
    A named sequence of statements that performs some useful operation.
    Functions may or may not take arguments and may or may not produce a
    result.

  - function definition:  
    A statement that creates a new function, specifying its name,
    parameters, and the statements it contains.

  - function object:  
    A value created by a function definition. The name of the function
    is a variable that refers to a function object.

  - header:  
    The first line of a function definition.

  - body:  
    The sequence of statements inside a function definition.

  - parameter:  
    A name used inside a function to refer to the value passed as an
    argument.

  - function call:  
    A statement that runs a function. It consists of the function name
    followed by an argument list in parentheses.

  - argument:  
    A value provided to a function when the function is called. This
    value is assigned to the corresponding parameter in the function.

  - local variable:  
    A variable defined inside a function. A local variable can only be
    used inside its function.

  - return value:  
    The result of a function. If a function call is used as an
    expression, the return value is the value of the expression.

  - fruitful function:  
    A function that returns a value.

  - void function:  
    A function that always returns <span>`None`</span>.

  - <span>`None`</span>:  
    A special value returned by void functions.

  - module:  
    A file that contains a collection of related functions and other
    definitions.

  - import statement:  
    A statement that reads a module file and creates a module object.

  - module object:  
    A value created by an <span>`import`</span> statement that provides
    access to the values defined in a module.

  - dot notation:  
    The syntax for calling a function in another module by specifying
    the module name followed by a dot (period) and the function name.

  - composition:  
    Using an expression as part of a larger expression, or a statement
    as part of a larger statement.

  - flow of execution:  
    The order statements run in.

  - stack diagram:  
    A graphical representation of a stack of functions, their variables,
    and the values they refer to.

  - frame:  
    A box in a stack diagram that represents a function call. It
    contains the local variables and parameters of the function.

  - traceback:  
    A list of the functions that are executing, printed when an
    exception occurs.

## Exercises

Write a function named `right_justify` that takes a string named
<span>`s`</span> as a parameter and prints the string with enough
leading spaces so that the last letter of the string is in column 70 of
the display.

    >>> right_justify('monty')
                                                                     monty

Hint: Use string concatenation and repetition. Also, Python provides a
built-in function called <span>`len`</span> that returns the length of a
string, so the value of `len('monty')` is 5.

A function object is a value you can assign to a variable or pass as an
argument. For example, `do_twice` is a function that takes a function
object as an argument and calls it twice:

    def do_twice(f):
        f()
        f()

Here’s an example that uses `do_twice` to call a function named
`print_spam` twice.

    def print_spam():
        print('spam')
    
    do_twice(print_spam)

1.  Type this example into a script and test it.

2.  Modify `do_twice` so that it takes two arguments, a function object
    and a value, and calls the function twice, passing the value as an
    argument.

3.  Copy the definition of `print_twice` from earlier in this chapter to
    your script.

4.  Use the modified version of `do_twice` to call `print_twice` twice,
    passing `'spam'` as an argument.

5.  Define a new function called `do_four` that takes a function object
    and a value and calls the function four times, passing the value as
    a parameter. There should be only two statements in the body of this
    function, not four.

Solution: <http://thinkpython2.com/code/do_four.py>.

Note: This exercise should be done using only the statements and other
features we have learned so far.

1.  Write a function that draws a grid like the following:
    
        + - - - - + - - - - +
        |         |         |
        |         |         |
        |         |         |
        |         |         |
        + - - - - + - - - - +
        |         |         |
        |         |         |
        |         |         |
        |         |         |
        + - - - - + - - - - +
    
    Hint: to print more than one value on a line, you can print a
    comma-separated sequence of values:
    
        print('+', '-')
    
    By default, <span>`print`</span> advances to the next line, but you
    can override that behavior and put a space at the end, like this:
    
        print('+', end=' ')
        print('-')
    
    The output of these statements is `'+ -'` on the same line. The
    output from the next print statement would begin on the next line.

2.  Write a function that draws a similar grid with four rows and four
    columns.

Solution: <http://thinkpython2.com/code/grid.py>. Credit: This exercise
is based on an exercise in Oualline, <span> *Practical C Programming,
Third Edition*</span>, O’Reilly Media, 1997.
