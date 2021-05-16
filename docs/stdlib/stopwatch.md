---
title: "Stopwatch"
tags: stdlib
---

This module provides some basic stopwatch or timer ability.

## Usage
Function `nanos` will accept a function of zero arguments and return a tuple, where the first element represent time spent executing this function and the second element is the original result of the function passed.

Example:
```haskell
Stopwatch::nanos (\-> sleep 5)
```
