---
title: "Data Types"
---

Yatta is dynamically typed language, meaning that types are checked during runtime. Values in Yatta are represented using following data types:

* boolean - `true` or `false`
* integer - 64 bit number
* double - double-precision 64-bit IEEE 754 floating point, written either using a floating point number or by an integer with an `f` suffix
* byte - single byte, written as an integer literal with a `b` suffix
* identifier: small letter followed by any number of letters or numbers or underscores
* character - a single UTF-8 character in single quotes: `'c'`
* string - UTF-8 characters in quotes: `"hello world"`. String is technically a sequence of characters (highly optimized internally, but for a programmer it looks and feels as a sequence).
* symbols - preceded by a colon: `:error`
* tuple - in parenthesis: `(1, 2, 3)`
* sequence - biderctional list like structure with constant time random access in brackets: `[1, 2, 3]`
* dictionary - in curly braces: `{:one = 1, :two = 2}`
* set - in curly braces: `{1, 2, 3}`
* anonymous function(lambda): `\first_arg second_arg -> first_arg + second_arg`
* unit, or `()` - representing no value
* native object: underlying Java object that is used by some stdlib functions, such as File descriptor
* stm - [software transactional memory](stdlib/stm.md)
* var - reference to entry in the [software transactional memory](stdlib/stm.md)
