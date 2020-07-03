---
title: "Overview"
tags: stdlib
---

Following functions and modules are provided as part of the standard library.

# Functions
* [sleep](functions/sleep.md) - Suspends computation by specified duration
* [timeout](functions/timeout.md) - Either the provided value is computed by the specified timeout, or a `:timeout` exception is raised
* [async](functions/async.md) - Executes the lambda asynchronously
* [identity](functions/identity.md) - Returns the value provided to it as an argument, without any modification
* [str](functions/str.md) - Converts any value to its string representation
* [int](functions/int.md) - Converts any number to to int
* [float](functions/float.md) - Converts any value to float
* [eval](functions/eval.md) - Dynamically evaluates the string as a Yatta expression
* [never](functions/never.md) - Function that is never completed


# Modules
* [Types](types.md) - contains functions for checking a type of a value
* [IO](io.md) - standard input/output actions
* [Seq](seq.md) - contains functions for manipulating sequences
* [Set](set.md) - contains functions for manipulating sets
* [Dict](dict.md) - contains functions for manipulating dictionaries
* [File](file.md) - contains functions for manipulating files
* [Transducers](transducers.md) - contains reducer transformers, used for example by generators
* [JSON](json.md) - JSON parser and generator functions
* [Tuple](tuple.md) - helper functions for analyzing/manipulating tuples
* [http\Client](http/client.md) - simple HTTP client
* [http\Server](http/server.md) Java - Java interop functions
* [java\Types](java/types.md) - Java types conversions
* [Java](java.md) - Java interoperability interface
* [System](system.md) - execute external system programs
* [context\Local](context/local.md) - utilities for implementing custom context managers, see [resource management](/features/resource-management.md) for details
* [STM](stm.md) - Software Transactional Memory
* [Regexp](regexp.md) - Regular Expressions
