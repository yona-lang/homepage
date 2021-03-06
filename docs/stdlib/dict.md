---
title: "Dict"
tags: stdlib
---

This module provides functions for manipulating Dictionaries. Dictionaries are immutable data structure containing unique pairs of key and value.

## Usage
There is a special syntax to create an empty dictionary: `{}`. Dictionaries support special [operators](/features/operators) for adding elements or operations typical for dictionaries, such as unions or intersections etc.

### Folding a dictionary
Folding a dictionary is a process of iterating over its key-value pairs while producing one result value. An example of a folding function could be a sum function, that iterates over all elements and adds all the values up, producing a single result.

Functions `foldl` take 3 arguments:
* 2-argument lambda (accumulator, (key, value)) returning a new value of an accumulator
* initial value of the accumulator
* dictionary to fold

This example shows how to sum up a dictionary:
```haskell
Dict::fold (\acc (key, value) -> acc + value) 0 {'a' = 1, 'b' = 2, 'c' = 3}
```

### Reducing a dictionary
Reducing a dictionary means applying a [transducer](transducers.md) onto a dictionary. Transducers are a generic, high-level method for implementing operations over a data structure.

Function `reduce` takes 2 arguments:
* transducer
* dictionary to reduce

Example with a map transducer:
```haskell
Dict::reduce (Transducers::map \val -> val * 2 (0, \state val -> state + 1, identity)) {'a' = 1, 'b' = 2, 'c' = 3}
```

This example will produce `12`, as it sums up all the values doubled.

### Obtaining a length of a dictionary
Function `len` returns a length of a dictionary:
```haskell
length = Dict::len {'a' = 1, 'b' = 2, 'c' = 3}
```

### Looking up a value by a key
Function `lookup` returns a value by the key, or `()` if not found.

First argument is the second is the key and the second one is the dictionary.

```haskell
length = Dict::lookup 'a' {'a' = 1, 'b' = 2, 'c' = 3}
```

### Building a dictionary from a sequence of tuples
Function `from_seq` builds a dictionary from a sequence of tuples.

Example:
```haskell
Dict::from_seq [(1, 4), (2, 5), (3, 6)]
```

Will return `{1 = 4, 2 = 5, 3 = 6}`.


### Turning a dictionary to a sequence of tuples
Function `entries` is the inverse operation of the `from_seq` function and it returns a sequence of tuples `(key, value)` for the provided dictionary.

Example:
```haskell
Dict::entries {1 = 4, 2 = 5, 3 = 6}
```

Will return `[(1, 4), (2, 5), (3, 6)]`.


### Getting keys from the dictionary
Function `keys` returns a set of all keys for the provided dictionary.

Example:
```haskell
Dict::keys {1 = 4, 2 = 5, 3 = 6}
```

Will return `{1, 2, 3}`.
