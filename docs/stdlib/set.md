---
title: "Set"
tags: stdlib
---

This module provides functions for manipulating Sets. Sets are immutable data structure containing unique elements.

## Usage
There is function `Set::empty` to create a new empty set. Sets support special [operators](/features/operators.md) for adding elements or operations typical for sets, such as unions or intersections etc.

### Folding a set
Folding a set is a process of iterating over its values while producing one result value. An example of a folding function could be a sum function, that iterates over all elements and adds them up, producing a single result.

Functions `foldl` take 3 arguments:
* 2-argument lambda (accumulator, element) returning a new value of an accumulator
* initial value of the accumulator
* set to fold

This example shows how to sum up a set:
```haskell
Set::fold (\acc val -> acc + val) 0 {1, 2, 3}
```

### Reducing a set
Reducing a set means applying a [transducer](transducers.md) onto a set. Transducers are a generic, high-level method for implementing operations over a data structure.

Function `reduce` takes 2 arguments:
* transducer
* set to reduce

Example with a filter transducer:
```haskell
Set::reduce (Transducers::filter \val -> val < 0 (0, \state val -> state + val, \state -> state * 2)) {-2,-1,0,1,2}
```

This example will produce `-6`, as it sums up all negative numbers in the set.

### Obtaining a length of a set
Function `len` returns a length of a set:
```haskell
length = Set::len {1, 2, 3}
```


### Turning a set to a sequence
Function `to_seq` constructs a sequence containing elemtents of the given set. Elements in this sequence are in no particular order.

Example:
```haskell
Set::to_seq {1, 2, 3}
```

Will return `[1, 2, 3]`.
