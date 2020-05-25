---
title: "timeout"
tags: stdlib
---

Function `timeout` will timeout an asynchronous computation after specified amount of time. It requires two arguments, a [time tuple]({{< ref "/docs/stdlib/misc/timetuple" >}}) representing the duration for which the computation is allowed to run and asynchronous computation, which can be obtain by an [async]({{< ref "async" >}}) function for example. If the value is not computed in the specified timeout, a `:timeout` exception is raised.

## Usage
```haskell
timeout (:seconds, 5) (async \-> :something)
```

It is recommended to always wrap a function passed as the second argument using [`async`]({{< ref "async" >}}) function, this way you can ensure that the function is being calculated asynchronously and the timeout can be applied.
