---
title: "Java"
tags: stdlib
---

This module provides important functions for [interoperability with Java](/docs/polyglot.md).

## Usage
This modules allows Java executing specific code, such as instantiating a class, casting to another type, checking whether a value is an instance of a class, or throwing Java exceptions.

### Representing a Java type
Function `type` takes a string - a fully qualified name of a Java class and creates a Yatta representation of given class. This value can be used to create new instances of this type.

Example:

```haskell
    big_integer = Java::type "java.math.BigInteger"
```

### Creating a new instance of a Java class
Function `new` takes a Java type as the first argument and a sequence of arguments that are passed to the first matching constructor of the given type. Matching in this case means having same number of arguments, types are not checked.

Example:

```haskell
    big_integer_two = Java::new big_integer ["2"]
```

### Throwing Java exceptions
Function `throw` may be used to throw a Java exception. It takes an object as an argument and this object must be a Java `Throwable`.

Example:

```haskell
let
    type = Java::type "java.lang.ArithmeticException"
    error = Java::new type ["testing error"]
in Java::throw error
```

### Checking whether an object is instance of
Function `instanceof` takes an object and a type as arguments and returns a boolean if the object is instance of the provided type.

Example:

```haskell
let
    type = Java::type "java.math.BigInteger"
    instance = Java::new type ["50"]
in Java::instanceof instance type
```

### Casting an object to another type
Function `case` takes an object and a type to which the object should be casted to.

Example:

```haskell
let
    type = Java::type "java.math.BigInteger"
    instance = Java::new type ["50"]
    cast_to_type = Java::type "java.lang.Number"
in Java::cast instance cast_to_type
```
