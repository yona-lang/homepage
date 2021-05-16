---
title: "Seq"
tags: stdlib
---

This module provides functions for manipulating Sequences. Sequences are bi-directional, immutable data structures with constant time random element lookup. Strings in Yona are specially optimized sequences of UTF-8 characters.

## Usage
There is a special syntax to create an empty sequence: `[]` or `""`. Sequences support special [operators](/features/operators.md) for adding elements or joining with other sequences.

### Folding a sequence
Folding a sequence is a process of iterating over its values while producing one result value. An example of a folding function could be a sum function, that iterates over all elements and adds them up, producing a single result.

Sequences can be folded from both directions - left or right.

Module `Seq` contains functions `foldl` and `foldr` for folding from the left or right side respectively.

Functions `foldl` and `foldr` take 3 arguments:
* 2-argument lambda (accumulator, element) returning a new value of an accumulator
* initial value of the accumulator
* sequence to fold

This example shows how to sum up a sequence:
```haskell
Seq::foldl (\acc val -> acc + val) 0 [1, 2, 3]
```

### Reducing a sequence
Reducing a sequence means applying a [transducer](transducers.md) onto a sequence. Transducers are a generic, high-level method for implementing operations over a data structure.

Function `reduce` takes 2 arguments:
* transducer
* sequence to reduce

Example with a filter transducer:
```haskell
Seq::reducel (Transducers::filter \val -> val < 0 (0, \acc val -> acc + val, \acc -> acc * 2)) [-2,-1,0,1,2]
```

This example will produce `-6`, as it sums up all negative numbers in the sequence.

### Obtaining a length of a sequence
Function `len` returns a length of a sequence:
```haskell
length = Seq::len [1, 2, 3]
```

### Splitting a sequence at an index
Function `splitAt` splits the sequence at a given index into a tuple of two sequences:
```haskell
Seq::split_at 2 [1, 2, 3, 4, 5]
```

will yield `([1, 2], [3, 4, 5])`

### Checking whether a sequence is a string
Function `is_string` checks whether a sequence is a string. It can be useful in [guard expressions](/features/syntax.md) for example.
```haskell
Seq::is_string "hello"
```

will result in a `true`.

### Looking up an element by an index
Function `lookup` takes an index and a sequence and returns an element on that index. It throws `:badarg` exception if the index is not found.
```haskell
Seq::lookup 2 [1, 2, 3]
```

### Taking first `n` elements
Function `takes` takes first `n` elements.  It throws `:badarg` exception if the `n` is greater or equal than the length of the sequence.
```haskell
Seq::take 2 [1, 2, 3]  # returns [1, 2]
```

### Dropping first `n` elements
Function `drop` drops first `n` elements.  It throws `:badarg` exception if the `n` is greater or equal than the length of the sequence.
```haskell
Seq::drops 2 [1, 2, 3]  # returns [3]
```

### Zipping two sequences into one
Function `zip` takes two sequences and produces one with tuples, one from each sequence.

Example:
```haskell
Seq::zip [1, 2, 3] [4, 5, 6]
```

Will create a new sequence `[(1, 4), (2, 5), (3, 6)]`.

### Encoding / Decoding strings
Functions `encode` and `decode` convert Strings (UTF-8 character sequences) to and from Sequence of bytes.
```haskell
Seq::encode "hello" |> Seq::decode  # returns "hello"
```

### Trimming whitespace
Returns a string whose value is this string, with all leading and trailing space removed, where space is defined as any character whose codepoint is less than or equal to `U+0020` (the space character).
```haskell
Seq::trim " hello "  # returns "hello"
```
