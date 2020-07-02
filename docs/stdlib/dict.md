# Dict

This module provides functions for manipulating Dictionaries. Dictionaries are immutable data structure containing unique pairs of key and value.

## Usage
There is a special syntax to create an empty dictionary: `{}`. Dictionaries support special [operators](/features/operators) for adding elements or operations typical for dictionaries, such as unions or intersections etc.

### Folding a dictionary
Folding a dictionary is a process of iterating over its key-value pairs while producing one result value. An example of a folding function could be a sum function, that iterates over all elements and adds all the values up, producing a single result.

Functions `foldl` take 3 arguments:
* dictionary to fold
* 2-argument lambda (accumulator, (key, value)) returning a new value of an accumulator
* initial value of the accumulator

This example shows how to sum up a dictionary:
```haskell
Dict::fold {'a' = 1, 'b' = 2, 'c' = 3} (\acc (key, value) -> acc + value) 0
```

### Reducing a dictionary
Reducing a dictionary means applying a [transducer](transducers.md) onto a dictionary. Transducers are a generic, high-level method for implementing operations over a data structure.

Function `reduce` takes 2 arguments:
* dictionary to reduce
* transducer

Example with a map transducer:
```haskell
Dict::reduce {'a' = 1, 'b' = 2, 'c' = 3} <| Transducers::map \val -> val * 2 (0, \state val -> state + 1, identity)
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
