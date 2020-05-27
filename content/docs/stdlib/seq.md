---
title: "Seq"
tags: stdlib
---

This module provides functions for manipulating Sequences. Sequences are bi-directional, immutable data structures with constant time random element lookup. Strings in Yatta are specially optimized sequences of UTF-8 characters.

## Usage
There is a special syntax to create an empty sequence: `[]` or `""`. Sequences support special [operators]({{< ref "/docs/operators" >}}) for adding elements or joining with other sequences.

### Folding a sequence
Folding a sequence is a process of iterating over its values while producing one result value. An example of a folding function could be a sum function, that iterates over all elements and adds them up, producing a single result.

Sequences can be folded from both directions - left or right.

Module `Seq` contains functions `foldl` and `foldr` for folding from the left or right side respectively.

Functions `foldl` and `foldr` take 3 arguments:
* sequence to fold
* 2-argument lambda (accumulator, element) returning a new value of an accumulator
* initial value of the accumulator

This example shows how to sum up a sequence:
```haskell
Seq::foldl [1, 2, 3] (\acc val -> acc + val) 0
```

### Reducing a sequence
Reducing a sequence means applying a [transducer]({{< ref "transducers" >}}) onto a sequence. Transducers are a generic, high-level method for implementing operations over a data structure.

Function `reduce` takes 2 arguments:
* sequence to reduce
* transducer

Example with a filter transducer:
```haskell
Seq::reducel [-2,-1,0,1,2] <| Transducers::filter \val -> val < 0 (0, \acc val -> acc + val, \acc -> acc * 2)
```

This example will produce `-6`, as it sums up all negative numbers in the sequence.

### Obtaining a length of a sequence
Function `len` returns a length of a sequence:
```haskell
length = Seq::len [1, 2, 3]
```

### Splitting a sequence at an index
Function `split` splits the sequence at a given index into a tuple of two sequences:
```haskell
Seq::split [1, 2, 3, 4, 5] 2
```

will yield `([1, 2], [3, 4, 5])`

### Checking whether a sequence is a string
Function `is_string` checks whether a sequence is a string. It can be useful in [guard expressions]({{< ref "/docs/syntax" >}}) for example.
```haskell
Seq::is_string "hello"
```

will result in a `true`.

### Looking up an element by an index
Function `lookup` takes an index and a sequence and returns an element on that index. It throws `:badarg` exception if the index is not found.
```haskell
Seq::lookup 2, [1, 2, 3]
```

### Zipping two sequences into one
Function `zip` takes two sequences and produces one with tuples, one from each sequence.

Example:
```haskell
Seq::zip [1, 2, 3] [4, 5, 6]
```

Will create a new sequence `[(1, 4), (2, 5), (3, 6)]`.