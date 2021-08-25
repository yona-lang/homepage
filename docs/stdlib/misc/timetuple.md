---
title: "Duration specified as a tuple"
tags: stdlib misc
---

Duration in Yona is specified as a tuple, of a time unit symbol and a number representing the amount of that time unit.

Available time unit symbols:

* `:milli` / `:millis`
* `:second` / `:seconds`
* `:minute` / `:minutes`
* `:hour` / `:hours`
* `:day` / `:days`
* `:week` / `:weeks`

Constructing a tuple representing a duration is as simple as:

```haskell
(:minutes, 5)
```

which would represent a duration of 5 minutes.
