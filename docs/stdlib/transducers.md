---
title: "Transducers"
tags: stdlib
---

Transducers are composable transformations of reducing functions. They provide a way to implement high order operations such as `map`, `filter`, `take` or `drop` in a way that they are independent from the collection they operate on. This way, these operations are defined only once, for all collections in Yatta, specifically for `Seq`, `Set` and `Dict` and in fact, they can be used for custom user-built data collections, provided that it exposes a `reduce` function.

Transducer constructor is a function that takes a transducer as an argument and returns a new transducer.

## Using a transducer
Transducer needs to be used in a combination with `reduce` function, best seen in an example, a sequence can be filtered like this:

```haskell
let
    transducer = Transducers::filter \val -> val < 0 (0, \acc val -> acc + val, \acc -> acc * 2)
in
    Seq::reducer [-2,-1,0,1,2] transducer
```

What this code does, is that it calls function `Seq::reducer` with 2 arguments:
* sequence to reduce
* transducer

And the transducer is custructed by function `Transducers::filter` which takes two arguments:
* predicate - function to filter elements added to the resulting collection
* transducer

The second argument here is another transducer actually. This is how transducers are composed. Transducer can either be created by using a predefined one in the `Transducers` module, or created manually as a tripple, as described in the next section.

## Implementing a transducer
Transducer in Yatta is a tripple, with following elements:
* initial state - is the initial state of the accumulator of the step function
* step function - is a function of two arguments - accumulator and an element, returning a new state of the accumulator
* complete function - is a function of one argument - the final state of accumulator, returning the final value of the `reduce` function

### Example: `filter` transducer
```haskell
# pred: function of one argument (element) returning boolean value
# init: initial state
# step: function of two arguments (accumulator, element) returning new value of the accumulator
# complete: function of one argument (final state of accumulator) returning final return value
filter pred (init, step, complete) = let
    new_step = \acc val -> if pred val then step acc val else acc
in
    (init, new_step, complete)
```

Source code for all transducers available in Yatta is available [here](https://github.com/yatta-lang/yatta/blob/master/language/lib-yatta/Transducers.yatta).

Note that transducers are actually used in [generators](/features/syntax.md#generators) as well, they are just a syntax sugar for them for the built-in collections.
