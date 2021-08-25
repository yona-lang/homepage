---
title: "JSON"
tags: stdlib
---

JSON module is a simple builtin interface for parsing and generating JSON files to and from Yona data types.

## Usage
This module has very simple interface. Just two functions, one to parse a string or a byte sequence, another one taking any Yona value and turning it into a JSON represented as a string.

### Parsing JSON
Function `parse` takes either a string (character sequence), or a byte sequence (can be obtained by reading a file in a binary mode for example), and produces a Yona value.

Example:
```haskell
let
    {"1" = one} = JSON::parse "{{\"1\": 2, \"3\": 4}}"
in one
```

This program parses the JSON string and returns a Yona dictionary, which is then pattern matched on and a key of `"1"` is assigned to name `one`. Thus the result of this expression is `2`.

Note that JSON has fewer data types, for example it has no representation of symbols. Regarding numbers, Yona first tries to parse them as integers, if that's not possible, it will parse them as floats.

If the parser occurs an invalid input, it throws an exception of type `:json_parser_error`.

This function is implemented as a native builtin in Java, mainly for performance reasons.

### Generating JSON {: #generate}
Function `generate` is a function of single argument (any Yona value), returning a string containing JSON representation of the passed argument.

This function is actually very simple to understand just by reading its [source code](https://github.com/yona-lang/yona/blob/master/language/lib-yona/JSON.yona) (it's less than 20 lines of code).

Example usage:
```haskell
JSON::generate {:one = 1, :two = 2}
```

As can be seen in the function implementation, symbols are translated into JSON strings, so this function will produce `{"one": 1, "two": 2}`.
