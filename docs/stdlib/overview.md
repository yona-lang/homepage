---
title: "Overview"
tags: stdlib
---

Following functions and modules are provided as part of the standard library.

# Functions
* [identity](functions/identity.md) - Returns the value provided to it as an argument, without any modification
* [eval](functions/eval.md) - Dynamically evaluates the string as a Yona expression

## Asynchronous Operation
* [async](functions/async.md) - Executes the lambda asynchronously
* [drop](functions/drop.md) - Execute the callback without waiting on its result
* [never](functions/never.md) - Function that is never completed
* [sleep](functions/sleep.md) - Suspends computation by specified duration
* [timeout](functions/timeout.md) - Either the provided value is computed by the specified timeout, or a `:timeout` exception is raised

## Type Manipulations
* [str](functions/str.md) - Converts any value to its string representation
* [int](functions/int.md) - Converts any number to to int
* [float](functions/float.md) - Converts any value to float
* [ord](functions/ord.md) - Return integer representation of an ASCII character

## Control Flow
* [infi](functions/infi.md) - Invoke functions infinite number of times
* [times](functions/times.md) - Repeat function `n` times


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
* [Socket\Client](socket/client.md) - TCP Socket Client
* [Socket\Server](socket/server.md) - TCP Socket Server
* [Socket\Connection](socket/connection.md) - Socket Connection handling - reading/writing to sockets
