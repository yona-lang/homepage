---
title: "async"
tags: stdlib
---

Function `async` executes a function asynchronously. It expects a lambda and this lambda will be computed asynchronously, thus not blocking the current thread. Result of this function is the result of the lambda provided as an argument. If `async` functions are nested, then the result of the whole expression is the result of the nested-most function.

## Usage:
```haskell
async (\-> println :hello)
```
