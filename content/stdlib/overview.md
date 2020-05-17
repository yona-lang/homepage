---
title: "Standard Library"
tags: stdlib
---

Yatta provides following functions and modules as part of the standard library.

# Functions
* [println]({{< ref "functions/println" >}}) - Prints a value to the standard output
* [sleep]({{< ref "functions/sleep" >}}) - Suspends computation by specified duration
* [timeout]({{< ref "functions/timeout" >}}) - Either the provided value is computed by the specified timeout, or a `:timeout` exception is raised
* [async]({{< ref "functions/async" >}}) - Executes the lambda asynchronously
* [identity]({{< ref "functions/identity" >}}) - Returns the value provided to it as an argument, without any modification
* [str]({{< ref "functions/str" >}}) - Converts any value to its string representation
* [int]({{< ref "functions/int" >}}) - Converts any number to to int
* [float]({{< ref "functions/float" >}}) - Converts any value to float
* [eval]({{< ref "functions/eval" >}}) - Dynamically evaluates the string as a Yatta expression
* [never]({{< ref "functions/never" >}}) - Function that is never completed
* [read]({{< ref "functions/read" >}}) - Reads a single character from standard input
* [readln]({{< ref "functions/readln" >}}) - Reads a line from the standard input


# Modules
* Types - contains functions for checking a type of a value
* Seq - contains functions for manipulating sequences
* Set - contains functions for manipulating sets
* Dict - contains functions for manipulating dictionaries
* [File]({{< ref "file" >}}) - contains functions for manipulating files
* Transducers - contains reducer transformers, used for example by generators
* Time - contains functions for reading current time
* JSON - JSON parser and generator functions
* STM - Software Transactional Memory
* Tuple - helper functions for analyzing/manipulating tuples
* [http\Client]({{< ref "http/client" >}}) - simple HTTP client
* [http\Server]({{< ref "http/server" >}}) Java - Java interop functions
* java\Types - Java types conversions
* [System]({{< ref "system" >}}) - execute external system programs
