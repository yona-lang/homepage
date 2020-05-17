---
title: "Evaluating code dynamically"
tags: stdlib
---

This function takes two arguments, first is the symbol of a language, such as `:yatta`, `:js` or `:python` and the second argument is a string of the expression to be evaluated in this language.

Example:

```haskell
eval :yatta "5 + 5"
```
