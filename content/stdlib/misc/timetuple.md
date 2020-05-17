---
title: "Duration specified as a tuple"
tags: stdlib misc
---

Duration in Yatta is specified as a tuple, of a time unit symbol and a number representing the amount of that time unit.

Available time unit symbols:
* `:millis`
* `:seconds`
* `:minutes`
* `:hours`
* `:days`
* `:weeks`

Constructing a tuple representing a duration is as simple as:

```haskell
(:minutes, 5)
```

which would represent a duration of 5 minutes.
