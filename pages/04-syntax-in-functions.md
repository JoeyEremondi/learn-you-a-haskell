---
layout: post
title: Syntax in Functions
---

Syntax in Functions
===================

Pattern matching
----------------

![four!](img/pattern.png)

This chapter will cover some of Elm's cool syntactic constructs and
we'll start with pattern matching. Pattern matching consists of
specifying patterns to which some data should conform and then checking
to see if it does and deconstructing the data according to those
patterns.

![case](img/case.png)

Many imperative languages (C, C++, Java, etc.) have case syntax and if
you've ever programmed in them, you probably know what it's about. It's
about taking a variable and then executing blocks of code for specific
values of that variable and then maybe including a catch-all block of
code in case the variable has some value for which we didn't set up a
case.

Elm takes that concept and one-ups it. Like the name implies, case
expressions are, well, expressions. Not only can we evaluate expressions 
based on the possible cases of the value of a variable, we can also do pattern
matching. This leads to really neat code that's simple and
readable. You can pattern match on any data type — numbers, characters,
lists, tuples, etc. Let's make a really trivial function that checks if
the number we supplied to it is a seven or not.

```elm
lucky : number -> String
lucky x =
    case x of
        7 -> "LUCKY NUMBER SEVEN!"
        _ -> "Sorry, you're out of luck, pal!"
```

When you call `lucky`, the patterns will be checked from top to bottom and
when it conforms to a pattern, the corresponding function body will be
used. The only way a number can conform to the first pattern here is if
it is 7. If it's not, it falls through to the second pattern, which
matches anything and discards it. The `_` is a wildcard that's used when we
don't care what the specific value is. If you do care what the value is,
you can write any variable name in this place and the value will be bound
to that variable, like this:

```elm
lucky : number -> String
lucky x =
    case x of
        7 -> "LUCKY NUMBER SEVEN!"
        n -> "Sorry, you're out of luck, pal! You had " ++ toString n
```

This function could have also been implemented by using an if statement. 
But what if we wanted a function that says the numbers from 1 to 5 and 
says `"Not between 1 and 5"` for any other number? Without pattern 
matching, we'd have to make a pretty convoluted if then else tree. 
However, with it:

```elm
sayMe : number -> String
sayMe x = 
    case x of
        1 -> "One!"
        2 -> "Two!"
        3 -> "Three!"
        4 -> "Four!"
        5 -> "Five!"
        _ -> "Not between 1 and 5"
```

Note that if we moved the last pattern (the catch-all one) to the top,
it would always say `"Not between 1 and 5"`, because it would catch all
the numbers and they wouldn't have a chance to fall through and be
checked for any other patterns.

We could use pattern matching to define a factorial function 
*recursively*, the way it is usually defined in mathematics. 
We start by saying that the factorial of 0 is 1. Then we
state that the factorial of any positive integer is that integer
multiplied by the factorial of its predecessor. Here's how that looks
like translated in Elm terms.

```elm
factorial : number -> number
factorial n =
    case n of
        0 -> 1
        _ -> n * factorial (n - 1)
```

This is the first time we've defined a function recursively. Recursion
is important in functional languages and we'll take a closer look at it later. But in
a nutshell, this is what happens if we try to get the factorial of, say,
3. It tries to compute `3 * factorial 2`. The factorial of 2 is `2 *
factorial 1`, so for now we have `3 * (2 * factorial 1)`. `factorial 1` is
`1 * factorial 0`, so we have `3 * (2 * (1 * factorial 0))`. Now here
comes the trick — we've defined the factorial of 0 to be just 1 and
because it encounters that pattern before the catch-all one, it just
returns 1. So the final result is equivalent to `3 * (2 * (1 * 1))`.
Had we written the second pattern on top of the first one, it would
catch all numbers, including 0 and our calculation would never
terminate. That's why order is important when specifying patterns and
it's always best to specify the most specific ones first and then the
more general ones later.

Pattern matching must be complete. If we define a function like this:

```elm
charName : Char -> String
charName c = 
    case c of
        'a' -> "Albert"
        'b' -> "Broseph"
        'c' -> "Cecil"
```

The compiler will notice that we haven't accounted for all possibilites,
either by specifying a pattern for every possible value of `c`, or by
including a wildcard that matches on every value. It will say:

```
This `case` does not have branches for all possibilities.
3|     case c of
4|         'a' -> "Albert"
5|         'b' -> "Broseph"
6|         'c' -> "Cecil"

You need to account for the following values:

    <values besides 'a', 'b', and 'c'>

Add a branch to cover this pattern!
```

It complains that we have missing patterns, and rightfully so.
When making patterns, we should always include a catch-all pattern so
that our program doesn't crash if we get some unexpected input. Features
like this are why Elm can all but guarantee no runtime errors in 
your code.

As you can see, the syntax for case expressions is pretty simple:

```elm
case expression of 
    pattern -> result
    pattern -> result
    pattern -> result
    ...
```

`expression` is matched against the patterns. The pattern matching action
is the same as expected: the first pattern that matches the expression
is used. If it falls through the whole case expression and no suitable
pattern is found, a compilation error occurs.

Pattern matching can also be used in the declared parameters of
functions, and on tuples. What if we wanted to make a
function that takes two vectors in a 2D space (that are in the form of
pairs) and adds them together? To add together two vectors, we add their
x components separately and then their y components separately. Here's
how we would have done it if we didn't know about pattern matching:

```elm
addVectors : (Float, Float) -> (Float, Float) -> (Float, Float)
addVectors a b = 
    (Tuple.first a + Tuple.first b, Tuple.second a + Tuple.second b)
```

Well, that works, but there's a better way to do it. Let's modify the
function so that it uses pattern matching.

```elm
addVectors : (Float, Float) -> (Float, Float) -> (Float, Float)
addVectors (x1, y1) (x2, y2) = 
    (x1 + x2, y1 + y2)
```

There we go! Much better. Note that this is already a catch-all pattern.
The type of `addVectors` (in both cases) is `addVectors : (Float, Float) -> 
(Float, Float) - > (Float, Float)`, so we are guaranteed to get two pairs as
parameters.

`first` and `second` extract the components of pairs. But what about triples?
Well, there are no provided functions that do that but we can make our
own.

```elm
first : (a, b, c) -> a
first (x, _, _) = x

second : (a, b, c) -> b
second (_, y, _) = y

third : (a, b, c) -> c
third (_, _, z) = z
```

As a reminder, the `_` means that we really don't care what that part is, 
so we just write a `_`.

Lists themselves can also be used in pattern matching. You can match
with the empty list `[]` or any pattern that involves `::` and the empty
list. But since `[1,2,3]` is just syntactic sugar for `1::2::3::[]`, you can
also use the former pattern. A pattern like `x::xs` will bind the head of
the list to `x` and the rest of it to `xs`, even if there's only one element
so `xs` ends up being an empty list.

*Note*: The `x::xs` pattern is used a lot, especially with recursive
functions. But patterns that have `::` in them only match against lists of
length 1 or more.

*Note*: xs is not some special "pattern matching" variable. It's just like 
any other variable. x::y would bind the head of the list to x and the rest of
it to y. The s on the end is just used by convention.

If you want to bind, say, the first three elements to variables and the
rest of the list to another variable, you can use something like
`x::y::z::zs`. It will only match against lists that have three elements or
more.

Now that we know how to pattern match against list, let's make our own
implementation of the `List.member` function, which checks if a value
is present in a list, using 

```elm
member : a -> List a -> Bool
member value list =
    case list of
        [] -> False
        (x::xs) ->
            if x == value then
                True
            else 
                member value xs
```

Checking if it works:

```elm
toPrint = member 1 [1,2,3,4]
True
toPrint = member 1 [2,3,4,5]
False
```

Nice! Notice that if you want to bind to several variables (even if one
of them is just `_` and doesn't actually bind at all), we have to
surround them in parentheses. Also notice the if then else expression we
used. It's very similar to if statements in other languages, except it's
an expression instead of a statement. That means it evaluates to a specific
value, so we can't ignore the else.

Let's make a trivial function that tells us some of the first elements
of the list in (in)convenient English form.

```elm
tell : List a -> String
tell list =
    case list of
        [] -> "The list is empty"
        (x::[]) -> "The list has one element: " ++ toString x
        (x::y::[]) -> "The list has two elements: " ++ toString x ++ " and " ++ toString y
        (x::y::_) -> "This list is long. The first two elements are: " ++ toString x ++ " and " ++ toString y
```

This function is safe because it takes care of the empty list, a
singleton list, a list with two elements and a list with more than two
elements. Note that `(x::[])` and `(x::y::[])` could be rewriten as `[x]` and
`[x,y]` (because its syntatic sugar, we don't need the parentheses). We
can't rewrite `(x::y::_)` with square brackets because it matches any list
of length 2 or more.

This type of function construction is very common in Elm. Let's get some
more practice by using pattern matching and a little recursion to implement
our own `List.length` function:

```elm
length : List a -> Int
length list =
    case list of
        [] -> 0
        (_::xs) -> 1 + length xs
```

This is similar to the factorial and member functions we wrote earlier. First we
defined the result of a known input — the empty list. This is also known
as the edge condition. Then in the second pattern we take the list apart
by splitting it into a head and a tail. We say that the length is equal
to 1 plus the length of the tail. We use `_` to match the head because we
don't actually care what it is. Also note that we've taken care of all
possible patterns of a list. The first pattern matches an empty list and
the second one matches anything that isn't an empty list.

Let's see what happens if we call `length` on `['h','a','m']`. First, it will check
if it's an empty list. Because it isn't, it falls through to the second
pattern. It matches on the second pattern and there it says that the
length is `1 + length ['a','m']`, because we broke it into a head and a tail
and discarded the head. O-kay. The `length` of `['a','m']` is, similarly, `1 +
length ['m']`. So right now we have `1 + (1 + length ['m'])`. `length ['m'] is
1 + length []`. And we've defined `length []` to be `0`. So in the end we have 
`1 + (1 + (1 + 0))`.

Let's implement `List.sum`. We know that the sum of an empty list is 0. We
write that down as a pattern. And we also know that the sum of a list is
the head plus the sum of the rest of the list. So if we write that down,
we get:

```elm
sum : List number -> number
sum list =
    case list of
        [] -> 0
        (x::xs) -> x + sum xs
```

One more thing — you can't use `++` in pattern matches. If you tried to
pattern match against `(xs ++ ys)`, what would be in the first and what
would be in the second list? It doesn't make much sense. It would make
sense to match stuff against `(xs ++ [x,y,z])` or just `(xs ++ [x])`, but
because of the nature of lists, you can't do that.

Let it be
---------

Let bindings let you bind to variables anywhere and are expressions
themselves, so they are very useful for naming the results of more
complicated expressions. Just like any construct in Elm that is used 
to bind values to names, let bindings can be used for pattern matching. 
Let's see them in action! This is how we could define a function that 
gives us a cylinder's surface area based on its height and radius:

```elm
cylinder : Float -> Float -> Float
cylinder r h =
    let 
        sideArea = 2 * pi * r * h
        topArea = pi * r ^2
    in  
        sideArea + 2 * topArea
```

![let it be](img/letitbe.png)

The form is `let <bindings> in <expression>`. The names that you
define in the *let* part are accessible to the expression after the *in*
part. Notice that the names are also aligned in a single column.

Let bindings are expressions themselves. Remember when we did the
if statement and it was explained that an if then else statement is an
expression? That means you can cram it in almost anywhere.

```elm
toPrint = [if 5 > 3 then "Woo" else "Boo", if 'a' > 'b' then "Foo" else "Bar"]
["Woo", "Bar"]
toPrint = 4 * (if 10 > 5 then 10 else 0) + 2
42
```

You can also do that with let bindings.

```elm
toPrint = 4 * (let a = 9 in a + 1) + 2
42
```

They can also be used to introduce functions in a local scope:

```elm
toPrint = [let square x = x * x in (square 5, square 3, square 2)]
[(25,9,4)]
```

Like we said before, you can pattern match with *let*
bindings. They're very useful for quickly dismantling a tuple into
components and binding them to names and such.

```elm
toPrint = (let (a,b,c) = (1,2,3) in a+b+c) * 100
600
```
