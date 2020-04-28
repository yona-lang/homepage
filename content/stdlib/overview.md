---
title: "Standard Library"
date: 2020-04-25T18:32:16+02:00
draft: false
tags: stdlib
---

Yatta provides following functions and modules as part of the standard library.

# Functions
* `println <value>` - Prints a value to the standard output
* `sleep <time unit tuple>` - Suspends computation by `<time unit tuple>`
* `timeout <time unit tuple> <value>` - Either the `<value>` is computed by the specified timeout, or a `:timeout` exception is raised
* `async <0-arg lambda>` - Executes the lambda asynchronously
* `identity <value>` - Returns the value provided to it as an argument, without any modification
* `str <value>` - Converts any value to its string representation
* `int <value>` - Converts any number to to int
* `float <value>` - Converts any value to float
* `system <seq of args>` - Executes a system command, arguments can be passed as additional elements of the sequence
* `eval <string>` - Dynamically evaluates the string as a Yatta expression
* `never` - Function that is never completed
* `read` - Reads a single character from standard input
* `readln` - Reads a line from the standard input


# Modules
* Types - contains functions for checking a type of a value
* Seq - contains functions for manipulating sequences
* Set - contains functions for manipulating sets
* Dict - contains functions for manipulating dictionaries
* File - contains functions for manipulating files
* Transducers - contains reducer transformers, used for example by generators
* Time - contains functions for reading current time
* JSON - JSON parser and generator functions
* STM - Software Transactional Memory
* Tuple - helper functions for analyzing/manipulating tuples
* [http\Client]({{< ref "http/client" >}}) - simple HTTP client
* [http\Server]({{< ref "http/server" >}}) Java - Java interop functions
* java\Types - Java types conversions
