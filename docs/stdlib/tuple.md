---
title: "Tuple"
tags: stdlib
---

This is a simple module providing functions for manipulating tuples.

## Usage
Currently this module provides a single function, for converting a tuple into a sequence.

### Converting a tuple into a sequence
Function `to_seq` takes a tuple and turns into a sequence.

Example:
```haskell
let
    seq = (:one, :two)
in
    seq
```

Will produce `[:one, :two]`.
