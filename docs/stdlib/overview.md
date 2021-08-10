---
title: "Overview"
tags: stdlib
---

Following functions and modules are provided as part of the standard library.

# Functions & Modules

## Data Structures
### Modules
* [Seq](seq.md) - contains functions for manipulating sequences
* [Set](set.md) - contains functions for manipulating sets
* [Dict](dict.md) - contains functions for manipulating dictionaries
* [Tuple](tuple.md) - helper functions for analyzing/manipulating tuples
* [STM](stm.md) - Software Transactional Memory


## Asynchronous Operation
### Functions
* [async](functions/async.md) - Executes the lambda asynchronously
* [drop](functions/drop.md) - Execute the callback without waiting on its result
* [never](functions/never.md) - Function that is never completed
* [sleep](functions/sleep.md) - Suspends computation by specified duration
* [timeout](functions/timeout.md) - Either the provided value is computed by the specified timeout, or a `:timeout` exception is raised


## Type Manipulations
### Modules
* [Types](types.md) - contains functions for checking a type of a value

### Functions
* [str](functions/str.md) - Converts any value to its string representation
* [int](functions/int.md) - Converts any number to to int
* [float](functions/float.md) - Converts any value to float
* [ord](functions/ord.md) - Return byte representation of an ASCII character

## Control Flow
### Functions
* [infi](functions/infi.md) - Invoke functions infinite number of times
* [times](functions/times.md) - Repeat function `n` times


## Input/Output
### Modules
* [IO](io.md) - standard input/output actions
* [File](file.md) - contains functions for manipulating files
* [http\Client](http/client.md) - simple HTTP client
* [http\Server](http/server.md) Java - Java interop functions
* [socket\tcp\Client](socket/tcp/client.md) - TCP Socket Client
* [socket\tcp\Server](socket/tcp/server.md) - TCP Socket Server
* [socket\tcp\Connection](socket/tcp/connection.md) - Socket Connection handling - reading/writing to sockets
* [terminal\Colors](terminal/colors.md) - Terminal colors
* [terminal\colors\Foreground](terminal/colors/foreground.md) - Terminal colors - foreground
* [terminal\colors\Background](terminal/colors/background.md) - Terminal colors - foreground


## Interoperability
### Modules
* [java\Types](java/types.md) - Java types conversions
* [Java](java.md) - Java interoperability interface
* [System](system.md) - execute external system programs

### Functions
* [eval](functions/eval.md) - Dynamically evaluates the string as a Yona express


## Miscellaneous
### Modules
* [Transducers](transducers.md) - contains reducer transformers, used for example by generators
* [context\Local](context/local.md) - utilities for implementing custom context managers, see [resource management](/features/resource-management.md) for details
* [JSON](json.md) - JSON parser and generator functions
* [Regexp](regexp.md) - Regular Expressions
* [Stopwatch](stopwatch.md) - Simple benchmarking utilities

### Functions
* [identity](functions/identity.md) - Returns the value provided to it as an argument, without any modification

