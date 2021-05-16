---
title: "drop"
tags: stdlib
---

Function `drop` executes a function asynchronously. It expects a lambda and this lambda will be computed asynchronously, thus not blocking the current thread. Unlike `async`, this function does not wait for the result of the computation and it returns immediately. The return value is `()`.

## Usage:
```haskell
drop (\-> IO::println :hello)
```
This function is particalarly useful when writing server applications. In these situations it is not desirable to wait for the result of individual connection handlers, and it only needs to wait for the connection and return immediately, while hanndling the connection in the background. This functionality is similar to **daemon** context managers, see [resource management](/features/resource-management.md) for more details.
