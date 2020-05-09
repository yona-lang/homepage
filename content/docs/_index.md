---
title: "Documentation"
draft: false
---

To get started with yatta, follow installation [instructions]({{< ref "installation" >}}). Following sections explain language features, syntax, semantics and evaluation model.

Yatta provides a standard library which is documented in the [standard library]({{< ref "/stdlib/overview" >}}) section.
Plenty of demos can be found in [tests](https://github.com/yatta-lang/yatta/tree/master/language/tests).

Polyglot usage is explained in this [section]({{< ref "/polyglot" >}}).

## Execution Model and asynchronous non-blocking IO & Concurrency
Yatta provides fully transparent runtime system that integrates asynchronous non-blocking IO features with concurrent execution of the code. This means, there is no special syntax or special data types representing asynchronous computations. Everything related to non-blocking IO is hidden within the runtime and exposed via the standard library, and all expressions consisting of asynchronous expressions(Asynchronous expression is usually obtained from the standard library or created by function `async`) are evaluated in asynchronous, non-blocking matter.

This approach has provides several benefits to languages that provide these features via libaries, mainly that such library would have to be adopted by other libraries/frameworks in order to be usable and it would still impose additional boilerplate simply because libraries cannot typically change language syntax/semantics. This is why Yatta provides these features from day one, built into language syntax and semantics and therefore it is always available to any program without any external dependencies. At the same time, putting these features directly on the language/runtime level allows for additional optimizations that could otherwise be tricky or impossible.

The example below shows a simple program that reads line from two different files and writes a combined line to the third line. The execution order is as follows:

1. Read line from file 1, at the same time, read line from file 2
2. After both lines have been read, write file to file 3 and return it as a result of the \verb|let| expression

The important point of this rather simple example is to demonstrate how easy it is to write asynchronous concurrent code in Yatta.

```haskell
let
    (:ok, line1) = File::read_line f1
    (:ok, line2) = File::read_line f2
in
    File::write_line f3 (line1 ++ line2)
```

This allows programmers to focus on expressing concurrent programs much more easily and not having to deal with the details of the actual execution order. Additionally, when code must be executed sequentially, without explicit dependencies, a special expression `do` is available.

In terms of implementation, the runtime system of Yatta can be viewed in terms of promise pipelineing or call-streams. The difference is that this pipelining and promise abstraction as such is completely transparent to the programmer and exists solely on the runtime level.

## Evaluation
Evaluation of an Yatta program consists of evaluating a single expression. This is important, because everything, including module definitions are simple expressions in Yatta.

Module loader then takes advantage of this principle, knowing that an imported module will be a file defining a module expression. It can simply evaluate it and retrieve the module itself.

## Data Types
See list of supported [data types]({{< ref "data-types" >}}) for more details.

## Syntax
See [syntax]({{< ref "syntax" >}}) guide.

## Pattern Matching
See [pattern matching]({{< ref "pattern-matching" >}}) guide.
