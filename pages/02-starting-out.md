---
layout: post
title: Starting Out
---

Starting Out
============

Ready, set, go!
---------------

![egg](img/startingout.png) Alright, let's get
started! If you're the sort of horrible person who doesn't read
introductions to things and you skipped it, you might want to read the
last section in the introduction anyway because it explains what you
need to follow this tutorial and how we're going to load functions.

The first thing we're going to do is set up a skeleton file.
It has a variable named `toPrint`. If you paste this on
[elm-try](http://elm-lang.org/try) and hit "Compile",
it will print the value of the variable `toPrint` to the screen to the right.
We'll use this to get a feel for Elm's syntax, and to see the result of some
basic computations.

```elm
import Html

main =
  Html.text (Debug.toString toPrint)
  
toPrint = 0
```

Congratulations, you're coding in Elm.
To see different values, just change what is after
 `toPrint =` sign, and hit "Compile" again.

 To try our first arithmetic, your code should look like this:

```elm
import Html

main =
  Html.text (Debug.toString toPrint)
  
toPrint =
  2 + 15
```

 We won't write out the `import` and `main` lines each time:
 just leave them in the file and change the value of `toPrint`.

Here's some simple arithmetic.

```elm
toPrint = 2 + 15
17
toPrint = 49 * 100
4900
toPrint = 1892 - 1472
420
toPrint = 5 / 2
2.5
```

This is pretty self-explanatory. We can also use several operators on
one line and all the usual precedence rules are obeyed. We can use
parentheses to make the precedence explicit or to change it.

```elm
toPrint = (50 * 100) - 4999
1
toPrint = 50 * 100 - 4999
1
toPrint = 50 * (100 - 4999)
-244950
```

Pretty cool, huh? Yeah, I know it's not but bear with me.

Boolean algebra is also pretty straightforward. As you probably know, `&&`
means a boolean *and*, `||` means a boolean *or*. `not` negates a `True` or a
`False`.

```elm
toPrint = True && False
False
toPrint = True && True
True
toPrint = False || True
True
toPrint = not False
True
toPrint = not (True && True)
False
```

Testing for equality is done like so.

```elm
toPrint = 5 == 5
True
toPrint = 1 == 0
False
toPrint = 5 /= 5
False
toPrint = 5 /= 4
True
toPrint = "hello" == "hello"
True
```

What about doing `5 + "llama"` or `5 == True`? Well, if we try the first
snippet, we get a nice error message!

    The right argument of (+) is causing a type mismatch.
    3| toPrint = 5 + "llama"

    As I infer the type of values flowing through your program, I see a conflict
    between these two types:

        number

        String

Long, but informative!
What Elm is telling us here is that `"llama"` is not a number and
so it doesn't know how to add it to 5. Even if it wasn't `"llama"` but
`"four"` or `"4"`, Elm still wouldn't consider it to be a number. `+`
expects its left and right side to be numbers. If we tried to do `True ==
5`, Elm would tell us that the types don't match. Whereas `+` works only
on things that are considered numbers, `==` works on any two things that
can be compared. But the catch is that they both have to be the same
type of thing. You can't compare apples and oranges. We'll take a closer
look at types a bit later. Note: you can do `5 + 4.0` because `5` is sneaky
and can act like an integer or a floating-point number. `4.0` can't act
like an integer, so `5` is the one that has to adapt.

You may not have known it but we've been using functions now all along.
For instance, `*` is a function that takes two numbers and multiplies
them. As you've seen, we call it by sandwiching it between them. This is
what we call an *infix* function. Most functions that aren't used with
numbers are *prefix* functions. Let's take a look at them.

![phoen](img/ringring.png) Functions are
usually prefix so from now on we won't explicitly state that a function
is of the prefix form, we'll just assume it. In most imperative
languages functions are called by writing the function name and then
writing its parameters in parentheses, usually separated by commas. In
Elm, functions are called by writing the function name, a space and
then the parameters, separated by spaces. For a start, we'll try calling
one of the most boring functions in Elm.

```elm
toPrint = identity 8
8
```

The `identity` function takes a value and returns that value.
As you can see, we just separate the function
name from the parameter with a space. Calling a function with several
parameters is also simple. The functions `min` and `max` take two things
that can be put in an order (like numbers!). `min` returns the one that's
lesser and `max` returns the one that's greater. See for yourself:

```elm
toPrint = min 9 10
9
toPrint = min 3.4 3.2
3.2
toPrint = max 100 101
101
```

Function application (calling a function by putting a space after it and
then typing out the parameters) has the highest precedence of them all.
What that means for us is that these two statements are equivalent.

```elm
toPrint = identity 9 + max 5 4 + 1
15
toPrint = (identity 9) + (max 5 4) + 1
15
```

Lots of people who come from imperative languages tend to stick to the
notion that parentheses should denote function application. For example,
in C, you use parentheses to call functions like `foo()`, `bar(1)` or `baz(3,
"haha")`. Like we said, spaces are used for function application in
Elm. So those functions in Elm would be `foo`, `bar 1` and `baz 3
"haha"`. So if you see something like `bar (bar 3)`, it doesn't mean that
`bar` is called with `bar` and `3` as parameters. It means that we first call
the function `bar` with `3` as the parameter to get some number and then we
call `bar` again with that number. In C, that would be something like
`bar(bar(3))`.

Baby's first functions
----------------------

In the previous section we got a basic feel for calling functions. Now
let's try making our own! Go to our editor window and add this
below the `import` line:
a function that takes a number and multiplies it by two.

```elm
doubleMe x = x + x
```

Functions are defined in a similar way that they are called. The
function name is followed by parameters separated by spaces. But when
defining functions, there's an `=` and after that we define what the
function does. Now we can play with the function that we
defined.

```elm
toPrint = doubleMe 9
18
toPrint = doubleMe 8.3
16.6
```

Because `+` works on integers as well as on floating-point numbers
(anything that can be considered a number, really), our function also
works on any number. Let's make a function that takes two numbers and
multiplies each by two and then adds them together.

```elm
doubleUs x y = x*2 + y*2
```

Simple. We could have also defined it as `doubleUs x y = x + x + y + y`.
Testing it out produces pretty predictable results.

```elm
toPrint = doubleUs 4 9
26
toPrint = doubleUs 2.3 34.2
73.0
toPrint = doubleUs 28 88 + doubleMe 123
478
```

As expected, you can call your own functions from other functions that
you made. With that in mind, we could redefine `doubleUs` like this:

```elm
doubleUs x y = doubleMe x + doubleMe y
```

This is a very simple example of a common pattern you will see
throughout Elm. Making basic functions that are obviously correct
and then combining them into more complex functions. This way you also
avoid repetition. What if some mathematicians figured out that 2 is
actually 3 and you had to change your program? You could just redefine
`doubleMe` to be `x + x + x` and since `doubleUs` calls `doubleMe`, it would
automatically work in this strange new world where 2 is 3.

Functions in Elm don't have to be in any particular order, so it
doesn't matter if you define `doubleMe` first and then `doubleUs` or if you
do it the other way around.

Now we're going to make a function that multiplies a number by 2 but
only if that number is smaller than or equal to 100 because numbers
bigger than 100 are big enough as it is!

```elm
doubleSmallNumber x = if x > 100 then x else x*2
```

![this is you](img/baby.png)

Right here we introduced Elm's if statement. You're probably
familiar with if statements from other languages. The difference between
Elm's if statement and if statements in imperative languages is that
the else part is mandatory in Elm. In imperative languages you can
just skip a couple of steps if the condition isn't satisfied but in
Elm every expression and function must return something. We could
have also written that if statement in one line but I find this way more
readable. Another thing about the if statement in Elm is that it is
an *expression*. An expression is basically a piece of code that returns
a value. `5` is an expression because it returns `5`, `4 + 8` is an
expression, `x + y` is an expression because it returns the sum of `x` and
`y`. Because the else is mandatory, an if statement will always return
something and that's why it's an expression. If we wanted to add one to
every number that's produced in our previous function, we could have
written its body like this.

```elm
doubleSmallNumber2 x = (if x > 100 then x else x*2) + 1
```

Had we omitted the parentheses, it would have added one only if `x` wasn't
greater than 100.

```elm
conanOBrien = "It's a-me, Conan O'Brien!"
```

There are two noteworthy things here. The first is that in the function
name we didn't capitalize Conan's name. That's because functions can't
begin with uppercase letters. We'll see why a bit later. The second
thing is that this function doesn't take any parameters. When a function
doesn't take any parameters, we usually say it's a *definition* (or a
*name*). Because we can't change what names (and functions) mean once
we've defined them, conanO'Brien and the string "It's a-me, Conan
O'Brien!" can be used interchangeably.

An intro to lists
-----------------

![BUY A DOG](img/list.png) Much like shopping
lists in the real world, lists in Elm are very useful. It's a common
used data structure and it can be used in a multitude of different ways
to model and solve a whole bunch of problems. Lists are SO awesome. In
this section we'll look at the basics of lists, as well as strings (which are
similar to lists).

In Elm, lists are a *homogenous* data structure. It stores several
elements of the same type. That means that we can have a list of
integers or a list of characters but we can't have a list that has a few
integers and then a few characters. And now, a list!


```elm
lostNumbers = [4,8,15,16,23,42]

toPrint = lostNumbers

[4,8,15,16,23,42]
```

As you can see, lists are denoted by square brackets and the values in
the lists are separated by commas. If we tried a list like
`[1,2,'a',3,'b','c',4]`, Elm would complain that characters (which
are, by the way, denoted as a character between single quotes) are not
numbers.

A common task is putting two lists together. This is done by using the
`++` operator.

```elm
toPrint = [1,2,3,4] ++ [9,10,11,12]
[1,2,3,4,9,10,11,12]
```

Watch out when repeatedly using the `++` operator on long strings. When
you put together two lists (even if you append a singleton list to a
list, for instance: `[1,2,3] ++ [4]`), internally, Elm has to walk
through the whole list on the left side of `++`. That's not a problem when
dealing with lists that aren't too big. But putting something at the end
of a list that's fifty million entries long is going to take a while.
However, putting something at the beginning of a list using the `::`
operator (also called the cons operator) is instantaneous.

```elm
toPrint = 5 :: [1,2,3,4,5]
[5,1,2,3,4,5]
```

Notice how `::` takes a number and a list of numbers or a character and a
list of characters, whereas `++` takes two lists. Even if you're adding an
element to the end of a list with `++`, you have to surround it with
square brackets so it becomes a list.

`[1,2,3] `is actually just syntactic sugar for `1::2::3::[]`. `[]` is an empty
list. If we prepend 3 to it, it becomes `[3]`. If we prepend 2 to that, it
becomes `[2,3]`, and so on.

*Note:* `[]`, `[[]]` and `[[],[],[]]` are all different things. The first one
is an empty list, the seconds one is a list that contains one empty
list, the third one is a list that contains three empty lists.


#### TODO section on Nth element?


The lists within a list can be of different lengths but they can't be of
different types. Just like you can't have a list that has some
characters and some numbers, you can't have a list that has some lists
of characters and some lists of numbers.

What else can you do with lists? Here are some basic functions that
operate on lists.

#### TODO section on Maybe?

`head` takes a list and returns its head. The head of a list is basically
its first element.

```elm
toPrint = List.head [5,4,3,2,1]
Just 5
```

`tail` takes a list and returns its tail. In other words, it chops off a
list's head.

```elm
toPrint = List.tail [5,4,3,2,1]
Just [4,3,2,1]
```


But what happens if we try to get the head of an empty list?

```elm
toPrint = List.head []
Nothing
```

What is `Nothing`? And why did `head` return `Just 5` before?
We'll talk about that a bit more later, but for now,
think of `Nothing` as a way to indicate when there's no correct value to return,
and `Just` as a way to show that we could return a value in a place
where `Nothing` could have also been returned.


`length` takes a list and returns its length, obviously.

```elm
toPrint = List.length [5,4,3,2,1]
5
```

`isEmpty` checks if a list is empty. If it is, it returns `True`, otherwise it
returns `False`. Use this function instead of `xs == []` (if you have a list
called `xs`).

```elm
toPrint = List.isEmpty [1,2,3]
False
toPrint = List.isEmpty []
True
```

`reverse` reverses a list.

```elm
toPrint = List.reverse [5,4,3,2,1]
[1,2,3,4,5]
```

`take` takes a number and a list. It extracts that many elements from the
beginning of the list. Watch.

```elm
toPrint = List.take 3 [5,4,3,2,1]
[5,4,3]
toPrint = List.take 1 [3,9,3]
[3]
toPrint = List.take 5 [1,2]
[1,2]
toPrint = List.take 0 [6,6,6]
[]
```

See how if we try to take more elements than there are in the list, it
just returns the list. If we try to take 0 elements, we get an empty
list.

`drop` works in a similar way, only it drops the number of elements from
the beginning of a list.

```elm
toPrint = List.drop 3 [8,4,2,1,5,6]
[1,5,6]
toPrint = List.drop 0 [1,2,3,4]
[1,2,3,4]
toPrint = List.drop 100 [1,2,3,4]
[]
```

`maximum` takes a list of stuff that can be put in some kind of order and
returns the biggest element.

`minimum` returns the smallest.

```elm
toPrint = List.minimum [8,4,2,1,5,6]
Just 1
toPrint = List.maximum [1,9,2,3,4]
Just 9
toPrint = List.maximum []
Nothing
```

What is the largest element of an empty list? There's no value we could
return that would make sense, so we use `Just` and `Nothing` here.

`sum` takes a list of numbers and returns their sum.

`product` takes a list of numbers and returns their product.

```elm
toPrint = List.sum [5,2,1,6,3,2,5,7]
31
toPrint = List.product [6,2,1,2]
24
toPrint = List.product [1,2,5,6,7,9,2,0]
0
```

`member` takes a thing and a list of things and tells us if that thing is a
member of the list.

```elm
toPrint = List.member 4 [3,4,5,6]
True
toPrint = List.member 10 [3,4,5,6]
False
```

Those were a few basic functions that operate on lists. We'll take a
look at more list functions [later](modules#data-list).

Texas ranges
------------

![draw](img/cowboy.png) What if we want a list
of all numbers between 1 and 20? Sure, we could just type them all out
but obviously that's not a solution for gentlemen who demand excellence
from their programming languages. Instead, we'll use ranges. Ranges are
a way of making lists that are sequences of numbers.

To make a list containing all the natural numbers from 1 to 20, you just
write `List.range 1 20`. That is the equivalent of writing
`[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]` and there's no
difference between writing one or the other except that writing out long
enumeration sequences manually is stupid.

```elm
toPrint = List.range 1 20
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]
```



Tuples
------

![tuples](img/tuple.png)

In some ways, tuples are like lists — they are a way to store several
values into a single value. However, there are a few fundamental
differences. A list of numbers is a list of numbers. That's its type and
it doesn't matter if it has only one number in it or a million numbers. 
Tuples, however, are used when you know exactly how many
values you want to combine and its type depends on how many components
it has and the types of the components. They are denoted with
parentheses and their components are separated by commas.

Another key difference is that they don't have to be homogenous. Unlike
a list, a tuple can contain a combination of several types.

Think about how we'd represent a two-dimensional vector in Elm. One
way would be to use a list. That would kind of work. So what if we
wanted to put a couple of vectors in a list to represent points of a
shape on a two-dimensional plane? We could do something like
`[[1,2],[8,11],[4,5]]`. The problem with that method is that we could also
do stuff like `[[1,2],[8,11,5],[4,5]]`, which Elm has no problem with
since it's still a list of lists with numbers but it kind of doesn't
make sense. But a tuple of size two (also called a pair) is its own
type, which means that a list can't have a couple of pairs in it and
then a triple (a tuple of size three), so let's use that instead.
Instead of surrounding the vectors with square brackets, we use
parentheses: `[(1,2),(8,11),(4,5)]`. What if we tried to make a shape like
`[(1,2),(8,11,5),(4,5)]`? Well, we'd get this error:

    The 2nd element of this list is an unexpected type of value.
    3| toPrint = [(1,2),(8,11,5),(4,5)]

    All elements should be the same type of value so that we can iterate over the
    list without running into unexpected values.

    As I infer the type of values flowing through your program, I see a conflict
    between these two types:

        _Tuple2

        (number)


It's telling us that we tried to use a pair and a triple in the same
list, which is not supposed to happen. You also couldn't make a list
like `[(1,2),("One",2)]` because the first element of the list is a pair
of numbers and the second element is a pair consisting of a string and a
number. Tuples can also be used to represent a wide variety of data. For
instance, if we wanted to represent someone's name and age in Elm,
we could use a triple: `("Christopher", "Walken", 55)`. As seen in this
example, tuples can also contain lists or strings.

Use tuples when you know in advance how many components some piece of
data should have. Tuples are much more rigid because each different size
of tuple is its own type, so you can't write a general function to
append an element to a tuple — you'd have to write a function for
appending to a pair, one function for appending to a triple, one
function for appending to a 4-tuple, etc.

While there are singleton lists, there's no such thing as a singleton
tuple. It doesn't really make much sense when you think about it. A
singleton tuple would just be the value it contains and as such would
have no benefit to us.

Two useful functions that operate on pairs:

`first` takes a pair and returns its first component.

```elm
toPrint = Tuple.first (8,11)
8
toPrint = Tuple.first ("Wow", False)
"Wow"
```

`second` takes a pair and returns its second component. Surprise!

```elm
toPrint = Tuple.second (8,11)
11
toPrint = Tuple.second ("Wow", False)
False
```

*Note:* these functions operate only on pairs. They won't work on
triples, 4-tuples, 5-tuples, etc. We'll go over extracting data from
tuples in different ways a bit later.
