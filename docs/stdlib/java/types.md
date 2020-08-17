---
title: "java\\Types"
tags: stdlib
---

This module provides some helper functions for [interoperability with Java](/features/polyglot.md).

## Usage
These are functions for converting Yona data types into Java types and should be used, when required as described in the polyglot page, [automatic conversions](/polyglot.md#automatic-conversions) section.

### Converting Yona integer to Java integer
Function `to_int` takes Yona integer(64-bit) and converts it into Java integer(32-bit). If it doesn't fit, it raises a `:badarg` exception.

Example:
```haskell
Java\Types::to_int 5
```

### Converting Yona float to Java float
Function `to_float` takes Yona float(64-bit) and converts it into Java float(32-bit). If it doesn't fit, it raises a `:badarg` exception.

Example:
```haskell
Java\Types::to_float 5f
```
