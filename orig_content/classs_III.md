
# Classes and methods

Although we are using some of Python’s object-oriented features, the
programs from the last two chapters are not really object-oriented
because they don’t represent the relationships between
programmer-defined types and the functions that operate on them. The
next step is to transform those functions into methods that make the
relationships explicit.

Code examples from this chapter are available from
<http://thinkpython2.com/code/Time2.py>, and solutions to the exercises
are in <http://thinkpython2.com/code/Point2_soln.py>.

## Object-oriented features

Python is an <span>**object-oriented programming language**</span>,
which means that it provides features that support object-oriented
programming, which has these defining characteristics:

  - Programs include class and method definitions.

  - Most of the computation is expressed in terms of operations on
    objects.

  - Objects often represent things in the real world, and methods often
    correspond to the ways things in the real world interact.

For example, the <span>`Time`</span> class defined in
Chapter [17](#time) corresponds to the way people record the time of
day, and the functions we defined correspond to the kinds of things
people do with times. Similarly, the <span>`Point`</span> and
<span>`Rectangle`</span> classes in Chapter [16](#clobjects) correspond
to the mathematical concepts of a point and a rectangle.

So far, we have not taken advantage of the features Python provides to
support object-oriented programming. These features are not strictly
necessary; most of them provide alternative syntax for things we have
already done. But in many cases, the alternative is more concise and
more accurately conveys the structure of the program.

For example, in <span>`Time1.py`</span> there is no obvious connection
between the class definition and the function definitions that follow.
With some examination, it is apparent that every function takes at least
one <span>`Time`</span> object as an argument.

This observation is the motivation for <span>**methods**</span>; a
method is a function that is associated with a particular class. We have
seen methods for strings, lists, dictionaries and tuples. In this
chapter, we will define methods for programmer-defined types.

Methods are semantically the same as functions, but there are two
syntactic differences:

  - Methods are defined inside a class definition in order to make the
    relationship between the class and the method explicit.

  - The syntax for invoking a method is different from the syntax for
    calling a function.

In the next few sections, we will take the functions from the previous
two chapters and transform them into methods. This transformation is
purely mechanical; you can do it by following a sequence of steps. If
you are comfortable converting from one form to another, you will be
able to choose the best form for whatever you are doing.

## Printing objects

In Chapter [17](#time), we defined a class named <span>`Time`</span> and
in Section [17.1](#isafter), you wrote a function named `print_time`:

    class Time:
        """Represents the time of day."""
    
    def print_time(time):
        print('%.2d:%.2d:%.2d' % (time.hour, time.minute, time.second))

To call this function, you have to pass a <span>`Time`</span> object as
an argument:

    >>> start = Time()
    >>> start.hour = 9
    >>> start.minute = 45
    >>> start.second = 00
    >>> print_time(start)
    09:45:00

To make `print_time` a method, all we have to do is move the function
definition inside the class definition. Notice the change in
indentation.

    class Time:
        def print_time(time):
            print('%.2d:%.2d:%.2d' % (time.hour, time.minute, time.second))

Now there are two ways to call `print_time`. The first (and less common)
way is to use function syntax:

    >>> Time.print_time(start)
    09:45:00

In this use of dot notation, <span>`Time`</span> is the name of the
class, and `print_time` is the name of the method. <span>`start`</span>
is passed as a parameter.

The second (and more concise) way is to use method syntax:

    >>> start.print_time()
    09:45:00

In this use of dot notation, `print_time` is the name of the method
(again), and <span>`start`</span> is the object the method is invoked
on, which is called the <span>**subject**</span>. Just as the subject of
a sentence is what the sentence is about, the subject of a method
invocation is what the method is about.

Inside the method, the subject is assigned to the first parameter, so in
this case <span>`start`</span> is assigned to <span>`time`</span>.

By convention, the first parameter of a method is called
<span>`self`</span>, so it would be more common to write `print_time`
like this:

    class Time:
        def print_time(self):
            print('%.2d:%.2d:%.2d' % (self.hour, self.minute, self.second))

The reason for this convention is an implicit metaphor:

  - The syntax for a function call, `print_time(start)`, suggests that
    the function is the active agent. It says something like, “Hey
    `print_time`\! Here’s an object for you to print.”

  - In object-oriented programming, the objects are the active agents. A
    method invocation like `start.print_time()` says “Hey
    <span>`start`</span>\! Please print yourself.”

This change in perspective might be more polite, but it is not obvious
that it is useful. In the examples we have seen so far, it may not be.
But sometimes shifting responsibility from the functions onto the
objects makes it possible to write more versatile functions (or
methods), and makes it easier to maintain and reuse code.

As an exercise, rewrite `time_to_int` (from Section [17.4](#prototype))
as a method. You might be tempted to rewrite `int_to_time` as a method,
too, but that doesn’t really make sense because there would be no object
to invoke it on.

## Another example

Here’s a version of <span>`increment`</span> (from
Section [17.3](#increment)) rewritten as a method:

    # inside class Time:
    
        def increment(self, seconds):
            seconds += self.time_to_int()
            return int_to_time(seconds)

This version assumes that `time_to_int` is written as a method. Also,
note that it is a pure function, not a modifier.

Here’s how you would invoke <span>`increment`</span>:

    >>> start.print_time()
    09:45:00
    >>> end = start.increment(1337)
    >>> end.print_time()
    10:07:17

The subject, <span>`start`</span>, gets assigned to the first parameter,
<span>`self`</span>. The argument, <span>`1337`</span>, gets assigned to
the second parameter, <span>`seconds`</span>.

This mechanism can be confusing, especially if you make an error. For
example, if you invoke <span>`increment`</span> with two arguments, you
get:

    >>> end = start.increment(1337, 460)
    TypeError: increment() takes 2 positional arguments but 3 were given

The error message is initially confusing, because there are only two
arguments in parentheses. But the subject is also considered an
argument, so all together that’s three.

By the way, a <span>**positional argument**</span> is an argument that
doesn’t have a parameter name; that is, it is not a keyword argument. In
this function call:

    sketch(parrot, cage, dead=True)

<span>`parrot`</span> and <span>`cage`</span> are positional, and
<span>`dead`</span> is a keyword argument.

## A more complicated example

Rewriting `is_after` (from Section [17.1](#isafter)) is slightly more
complicated because it takes two Time objects as parameters. In this
case it is conventional to name the first parameter <span>`self`</span>
and the second parameter <span>`other`</span>:

    # inside class Time:
    
        def is_after(self, other):
            return self.time_to_int() > other.time_to_int()

To use this method, you have to invoke it on one object and pass the
other as an argument:

    >>> end.is_after(start)
    True

One nice thing about this syntax is that it almost reads like English:
“end is after start?”

## The init method

The init method (short for “initialization”) is a special method that
gets invoked when an object is instantiated. Its full name is `__init__`
(two underscore characters, followed by <span>`init`</span>, and then
two more underscores). An init method for the <span>`Time`</span> class
might look like this:

    # inside class Time:
    
        def __init__(self, hour=0, minute=0, second=0):
            self.hour = hour
            self.minute = minute
            self.second = second

It is common for the parameters of `__init__` to have the same names as
the attributes. The statement

``` 
        self.hour = hour
```

stores the value of the parameter <span>`hour`</span> as an attribute of
<span>`self`</span>.

The parameters are optional, so if you call <span>`Time`</span> with no
arguments, you get the default values.

    >>> time = Time()
    >>> time.print_time()
    00:00:00

If you provide one argument, it overrides <span>`hour`</span>:

    >>> time = Time (9)
    >>> time.print_time()
    09:00:00

If you provide two arguments, they override <span>`hour`</span> and
<span>`minute`</span>.

    >>> time = Time(9, 45)
    >>> time.print_time()
    09:45:00

And if you provide three arguments, they override all three default
values.

As an exercise, write an init method for the <span>`Point`</span> class
that takes <span>`x`</span> and <span>`y`</span> as optional parameters
and assigns them to the corresponding attributes.

## The <span>`__str__`</span> method

`__str__` is a special method, like `__init__`, that is supposed to
return a string representation of an object.

For example, here is a <span>`str`</span> method for Time objects:

    # inside class Time:
    
        def __str__(self):
            return '%.2d:%.2d:%.2d' % (self.hour, self.minute, self.second)

When you <span>`print`</span> an object, Python invokes the
<span>`str`</span> method:

    >>> time = Time(9, 45)
    >>> print(time)
    09:45:00

When I write a new class, I almost always start by writing `__init__`,
which makes it easier to instantiate objects, and `__str__`, which is
useful for debugging.

As an exercise, write a <span>`str`</span> method for the
<span>`Point`</span> class. Create a Point object and print it.

## Operator overloading

By defining other special methods, you can specify the behavior of
operators on programmer-defined types. For example, if you define a
method named `__add__` for the <span>`Time`</span> class, you can use
the <span>`+`</span> operator on Time objects.

Here is what the definition might look like:

    # inside class Time:
    
        def __add__(self, other):
            seconds = self.time_to_int() + other.time_to_int()
            return int_to_time(seconds)

And here is how you could use it:

    >>> start = Time(9, 45)
    >>> duration = Time(1, 35)
    >>> print(start + duration)
    11:20:00

When you apply the <span>`+`</span> operator to Time objects, Python
invokes `__add__`. When you print the result, Python invokes `__str__`.
So there is a lot happening behind the scenes\!

Changing the behavior of an operator so that it works with
programmer-defined types is called <span>**operator
overloading**</span>. For every operator in Python there is a
corresponding special method, like `__add__`. For more details, see
<http://docs.python.org/3/reference/datamodel.html#specialnames>.

As an exercise, write an <span>`add`</span> method for the Point class.

## Type-based dispatch

In the previous section we added two Time objects, but you also might
want to add an integer to a Time object. The following is a version of
`__add__` that checks the type of <span>`other`</span> and invokes
either `add_time` or <span>`increment`</span>:

    # inside class Time:
    
        def __add__(self, other):
            if isinstance(other, Time):
                return self.add_time(other)
            else:
                return self.increment(other)
    
        def add_time(self, other):
            seconds = self.time_to_int() + other.time_to_int()
            return int_to_time(seconds)
    
        def increment(self, seconds):
            seconds += self.time_to_int()
            return int_to_time(seconds)

The built-in function <span>`isinstance`</span> takes a value and a
class object, and returns <span>`True`</span> if the value is an
instance of the class.

If <span>`other`</span> is a Time object, `__add__` invokes `add_time`.
Otherwise it assumes that the parameter is a number and invokes
<span>`increment`</span>. This operation is called a <span>**type-based
dispatch**</span> because it dispatches the computation to different
methods based on the type of the arguments.

Here are examples that use the <span>`+`</span> operator with different
types:

    >>> start = Time(9, 45)
    >>> duration = Time(1, 35)
    >>> print(start + duration)
    11:20:00
    >>> print(start + 1337)
    10:07:17

Unfortunately, this implementation of addition is not commutative. If
the integer is the first operand, you get

    >>> print(1337 + start)
    TypeError: unsupported operand type(s) for +: 'int' and 'instance'

The problem is, instead of asking the Time object to add an integer,
Python is asking an integer to add a Time object, and it doesn’t know
how. But there is a clever solution for this problem: the special method
`__radd__`, which stands for “right-side add”. This method is invoked
when a Time object appears on the right side of the <span>`+`</span>
operator. Here’s the definition:

    # inside class Time:
    
        def __radd__(self, other):
            return self.__add__(other)

And here’s how it’s used:

    >>> print(1337 + start)
    10:07:17

As an exercise, write an <span>`add`</span> method for Points that works
with either a Point object or a tuple:

  - If the second operand is a Point, the method should return a new
    Point whose \(x\) coordinate is the sum of the \(x\) coordinates of
    the operands, and likewise for the \(y\) coordinates.

  - If the second operand is a tuple, the method should add the first
    element of the tuple to the \(x\) coordinate and the second element
    to the \(y\) coordinate, and return a new Point with the result.

## Polymorphism

Type-based dispatch is useful when it is necessary, but (fortunately) it
is not always necessary. Often you can avoid it by writing functions
that work correctly for arguments with different types.

Many of the functions we wrote for strings also work for other sequence
types. For example, in Section [12.2](#histogram) we used
<span>`histogram`</span> to count the number of times each letter
appears in a word.

    def histogram(s):
        d = dict()
        for c in s:
            if c not in d:
                d[c] = 1
            else:
                d[c] = d[c]+1
        return d

This function also works for lists, tuples, and even dictionaries, as
long as the elements of <span>`s`</span> are hashable, so they can be
used as keys in <span>`d`</span>.

    >>> t = ['spam', 'egg', 'spam', 'spam', 'bacon', 'spam']
    >>> histogram(t)
    {'bacon': 1, 'egg': 1, 'spam': 4}

Functions that work with several types are called
<span>**polymorphic**</span>. Polymorphism can facilitate code reuse.
For example, the built-in function <span>`sum`</span>, which adds the
elements of a sequence, works as long as the elements of the sequence
support addition.

Since Time objects provide an <span>`add`</span> method, they work with
<span>`sum`</span>:

    >>> t1 = Time(7, 43)
    >>> t2 = Time(7, 41)
    >>> t3 = Time(7, 37)
    >>> total = sum([t1, t2, t3])
    >>> print(total)
    23:01:00

In general, if all of the operations inside a function work with a given
type, the function works with that type.

The best kind of polymorphism is the unintentional kind, where you
discover that a function you already wrote can be applied to a type you
never planned for.

## Debugging

It is legal to add attributes to objects at any point in the execution
of a program, but if you have objects with the same type that don’t have
the same attributes, it is easy to make mistakes. It is considered a
good idea to initialize all of an object’s attributes in the init
method.

If you are not sure whether an object has a particular attribute, you
can use the built-in function <span>`hasattr`</span> (see
Section [16.7](#hasattr)).

Another way to access attributes is the built-in function
<span>`vars`</span>, which takes an object and returns a dictionary that
maps from attribute names (as strings) to their values:

    >>> p = Point(3, 4)
    >>> vars(p)
    {'y': 4, 'x': 3}

For purposes of debugging, you might find it useful to keep this
function handy:

    def print_attributes(obj):
        for attr in vars(obj):
            print(attr, getattr(obj, attr))

`print_attributes` traverses the dictionary and prints each attribute
name and its corresponding value.

The built-in function <span>`getattr`</span> takes an object and an
attribute name (as a string) and returns the attribute’s value.

## Interface and implementation

One of the goals of object-oriented design is to make software more
maintainable, which means that you can keep the program working when
other parts of the system change, and modify the program to meet new
requirements.

A design principle that helps achieve that goal is to keep interfaces
separate from implementations. For objects, that means that the methods
a class provides should not depend on how the attributes are
represented.

For example, in this chapter we developed a class that represents a time
of day. Methods provided by this class include `time_to_int`,
`is_after`, and `add_time`.

We could implement those methods in several ways. The details of the
implementation depend on how we represent time. In this chapter, the
attributes of a <span>`Time`</span> object are <span>`hour`</span>,
<span>`minute`</span>, and <span>`second`</span>.

As an alternative, we could replace these attributes with a single
integer representing the number of seconds since midnight. This
implementation would make some methods, like `is_after`, easier to
write, but it makes other methods harder.

After you deploy a new class, you might discover a better
implementation. If other parts of the program are using your class, it
might be time-consuming and error-prone to change the interface.

But if you designed the interface carefully, you can change the
implementation without changing the interface, which means that other
parts of the program don’t have to change.

## Glossary

  - object-oriented language:  
    A language that provides features, such as programmer-defined types
    and methods, that facilitate object-oriented programming.

  - object-oriented programming:  
    A style of programming in which data and the operations that
    manipulate it are organized into classes and methods.

  - method:  
    A function that is defined inside a class definition and is invoked
    on instances of that class.

  - subject:  
    The object a method is invoked on.

  - positional argument:  
    An argument that does not include a parameter name, so it is not a
    keyword argument.

  - operator overloading:  
    Changing the behavior of an operator like <span>`+`</span> so it
    works with a programmer-defined type.

  - type-based dispatch:  
    A programming pattern that checks the type of an operand and invokes
    different functions for different types.

  - polymorphic:  
    Pertaining to a function that can work with more than one type.

## Exercises

Download the code from this chapter from
<http://thinkpython2.com/code/Time2.py>. Change the attributes of
<span>`Time`</span> to be a single integer representing seconds since
midnight. Then modify the methods (and the function `int_to_time`) to
work with the new implementation. You should not have to modify the test
code in <span>`main`</span>. When you are done, the output should be the
same as before. Solution: <http://thinkpython2.com/code/Time2_soln.py>.

<span id="kangaroo" label="kangaroo">\[kangaroo\]</span>

This exercise is a cautionary tale about one of the most common, and
difficult to find, errors in Python. Write a definition for a class
named <span>`Kangaroo`</span> with the following methods:

1.  An `__init__` method that initializes an attribute named
    `pouch_contents` to an empty list.

2.  A method named `put_in_pouch` that takes an object of any type and
    adds it to `pouch_contents`.

3.  A `__str__` method that returns a string representation of the
    Kangaroo object and the contents of the pouch.

Test your code by creating two <span>`Kangaroo`</span> objects,
assigning them to variables named <span>`kanga`</span> and
<span>`roo`</span>, and then adding <span>`roo`</span> to the contents
of <span>`kanga`</span>’s pouch.

Download <http://thinkpython2.com/code/BadKangaroo.py>. It contains a
solution to the previous problem with one big, nasty bug. Find and fix
the bug.

If you get stuck, you can download
<http://thinkpython2.com/code/GoodKangaroo.py>, which explains the
problem and demonstrates a solution.
