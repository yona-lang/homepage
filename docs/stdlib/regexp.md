---
title: "Regexp"
tags: stdlib
---

This module provides functions for compiling, executing and replacing regular expressions. Note that Yatta supports a subset of ECMAscript regular expressions, with the exception of back-references and arbitrary look-behind assertions.

## Usage
Module `Regexp` provides functions for compiling a regular expression from a string: `compile`, for executing them: `exec`, which finds occurrences of provided regular expression in the input string and `replace` that replaces occrrence of the regular expression match for a replacement string.

# Compiling regular expressions
Before using other functions, regular expression must be first compiled. Yatta provides function `compile` that takes a string describing the regular expression and a set of options:
- `global`: With this flag the search looks for all matches, without it – only the first match is returned.
- `multiline`: Multiline mode.
- `ignore_case`: With this flag the search is case-insensitive: no difference between `A` and `a`.
- `sticky`: "Sticky" mode: searching at the exact position in the text.
- `unicode`: Enables full unicode support. The flag enables correct processing of surrogate pairs.
- `dot_all`: Enables "dotall" mode, that allows a dot `.` to match newline character `\n`.

Example:

```haskell
Regexp::compile "(a|(b))c" {:ignore_case}
```

# Matching strings with regular expressions
Function `exec` "executes" a regular expression on the provided input, returning all matches as a sequence of matched strings, if none found, returning an empty sequence. It expects arguments in this order:
- `input` string
- compiled regular expression

Example:

```haskell
Regexp::compile "(a|(b))c" {:ignore_case} |> Regexp::exec "xacy"
```
will return `["ac", "a"]`

# Replacing strings with regular expressions
Function `replace` replaces occurrences matching the provided regular expression on the input with the provided replacement. It expects arguments in this order:
- `input` string
- `replacement` string
- compiled regular expression

Example:

```haskell
Regexp::compile "we" {:ignore_case, :global} |> Regexp::replace "We will, we will" "she"
```
which will produce `"she will, she will"`

Additionally, the replacement string may contain these special combinations of strings:
- `$$` which are replaced with `$`
- `$&` which are replaced with the whole match
