---
title: "times"
tags: stdlib
---

Function `times` executes the provided zero-argument function `n` number of times.

## Usage
```haskell
times 5 (\-> IO::println "hello")
```

**Note**: this function is typically called using the infix notation.

```haskell
5 `times` (\-> IO::println "hello")
```
