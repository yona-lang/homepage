---
title: "Pattern Matching"
---

Pattern matching is the most important feature for control flow in Yatta. It allows simple, short way of specifying patterns for the built in types, specifically:

Pattern matching, in combination with recursion are the basis of the control flow in Yatta.

## Scalar values - integer, float, byte, character, boolean, symbol
Pattern matching for scalar values is super simple. The pattern looks is the value itself, for example:

```haskell
case expr of
    5 -> do_something
    6 -> do_something_else
end
```

## Tuples
Pattern matching on tuples:

```haskell
case expr of
    (:ok, value)      -> do_something value  # value contains the second element of the tuple
    (:error, message) -> println message
end
```

## Records
Pattern matching on records is described in the section about Records.

## Underscore pattern
The underscore pattern `_` will match any value.

## Sequence & reverse sequence, multiple head & tails & their combinations in patterns
Sequence is a biderctional structure and can be easily pattern matched from either left or right side. Yatta allows pattern matching on more than a single element as well:

### Matching sequence on the beginning
```haskell
case [1] of
    1 -| [] -> 2
    []      -> 3
    _       -> 9
end
```
This code will result in `2`.

Yatta allows matching on more than just one element in the beginning:
```haskell
case [1, 2, 3, 4] of
    1 -| 2 -| []   -> 2
    1 -| 2 -| tail -> tail
    []             -> 3
    _              -> 9
end
```
Will produce `[3, 4]`.

### Matching sequence on the end
```haskell
case [1] of
    [] |- 1 -> 2
    []      -> 3
    _       -> 9
end
```
This code will result in `2`.

### Obtaining a remainer of a sequnce
```haskell
case [1, 2, 3] of
    1 -| []   -> 2
    1 -| tail -> tail
    []        -> 3
    _ -> 9
end
```
This code will result in `[2, 3]`. This could also be done from the other end of the sequence, using `|-` operator instead (and reversing head/tails sections).

### Matching a sequence elements
```haskell
case arg of
    []             -> 3
    [1, second, 3] -> second
    _               -> 9
end
```
Will produce `2`.

## Matching strings
Since strings are sequences of characters, they can be pattern matched on as such. Alternatively string literals may be used as well, or any combination of the two. For example:

```haskell
case "hello there" of
    'h' -| 'e' -| _ |- 'r' |- 'e' -> 0
    _                             -> 1
end
```
Which will produce a `0`.

Another example:
```haskell
case ['a', 'b', 'c'] of
    h -| "bc" -> h
    _         -> 1
end
```
Which produces an `'a'`.

## Dictionary patterns
Dictionaries can be matched on both keys and values. Example:
```haskell
case {"a" = 1, "b" = 2} of
    {"b" = 3}           -> 3
    {"a" = 1, "b" = bb} -> bb
    _                   -> 9
end
```
This will result in `2`.

## "As" patterns
Sometimes it can be useful to name a collection (sequence, set or dictionary) or a record in a pattern for later use. For example when matching on a record type, but ignoring all the fields. This can be done using `@` syntax in Yatta:

```haskell
let mod = module TestMod exports testfun as
        record TestRecord=(argone, argtwo)
        
        testfun = case TestRecord(argone = 1, argtwo = 2) of
            2            -> 0
            x@TestRecord -> x.argone
            _            -> 2
        end
    end
in mod::testfun
```
This slightly with an inlined module definition more complex example will produce `1`. Remember that records exist on a module level only.

## `let` expression patterns
`let` expression allows using patterns on the left side of its assignments. In that way it is not necessary to use `case` expression explicitly.

## * `do` expression patterns
`do` expression similarly to `let` expressions allows defining aliases, and they can be patterns on the left side. Unlike `let` expression, `do` expression does not require value assignment, but still allows it. For example:

```ruby
do
    (one, _) = (1, :unused)
    println one
    two = 2
    one + two
end
```
Which will result in `3` after `1` is printed.

## `case` patterns
This is THE pattern matching expression. It maches a value against a list of patterns and executes the expression of the matching pattern.

## Function & lambda patterns
Functions and lambdas may be defined using patterns as explained in the section about functions. The only limitation of the lambda definitions, is that they may only contain one pattern. This is not used much for control flow, but still useful for deconstructing some data structures. If lambda needs to pattern match multiple patterns, either define it as a named function, or use `case` expression.

## Guard expressions
Patterns may be enhanced further with guard expressions. Guard expressions are initiated by a `|` character followed by an expression that must evaluate into a boolean.
Each pattern may have multiple guard expressions, each on a new line.

For example, a function calculating body mass index could be defined like this:

```haskell
bmiTell bmi
    | bmi <= 18.5 = "You're underweight, you emo, you!"
    | bmi <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"
    | bmi <= 30.0 = "You're fat! Lose some weight, fatty!"
    | true        = "You're a whale, congratulations!"
```
As can be seen, guards allow checking additional conditions that can't be captured by pattern itself, such as checking whether a number is in a range.

## Non-linear patterns - ability to use same variable in pattern multiple times
Non-linear patterns are those that contain the same name in multiple places, for example:

```haskell
nonLinearHeadTailsTest head    -| _    head -| _ = head
nonLinearHeadTailsTest headOne -| _ headTwo -| _ = headOne + headTwo
```
For example, calling this function:
```haskell
nonLinearHeadTailsTest [2, 0] [3, 0]
```
Will yield `5`, because the first element in first and second argument must be the same.
