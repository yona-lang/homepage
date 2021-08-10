---
title: "terminal\\Colors"
tags: stdlib
---

This module contains basic terminal related functionality.

## Setting a terminal flag
Function `set` accepts a type of a flag to set. Possible types are:

* `:bold` / `:bright` - synonyms
* `:dim`
* `:underlined`
* `:blink`
* `:inverted` - invert the foreground and background colors
* `:hidden` - useful for passwords

Example:
```haskell
terminal\Colors::set :bold
```

## Resetting a terminal flag
Function `reset` resets flags set by function `set`. It accepts the same type of flags, with an additional option `:all` that resets all the flags.
