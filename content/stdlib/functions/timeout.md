---
title: "Timeout asynchronous computation after specified amount of time"
tags: stdlib
---

Function `time` requires two arguments, a [time tuple]({{< ref "/stdlib/misc/timetuple" >}}) representing the duration for which the computation is allowed to run and asynchronous computation, which can be obtain by an [async]({{< ref "async" >}}) function for example. If the value is not computed in the specified timeout, a `:timeout` exception is raised.

Usage:

```haskell
timeout (:seconds, 5) (async \-> :something)
```
