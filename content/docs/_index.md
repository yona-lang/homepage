---
title: "Documentation"
---

To get started with yatta, follow **[installation instructions]({{< ref "installation" >}})**. Following sections explain language features, syntax, semantics and evaluation model.

Yatta provides a standard library which is documented in the **[standard library]({{< ref "stdlib/overview" >}})** section.
Plenty of demos can be found in [tests](https://github.com/yatta-lang/yatta/tree/master/language/tests).

Yatta takes advantage of the **[polyglot]({{< ref "/polyglot" >}})** features of GraalVM and allows being used in combination with other languages on this platform, including Java.

## Execution Model
Yatta provides fully transparent runtime system that integrates asynchronous non-blocking IO features with concurrent execution of the code. This means, there is no special syntax or special data types representing asynchronous computations. Everything related to non-blocking IO is hidden within the runtime and exposed via the standard library, and all expressions consisting of asynchronous expressions(an asynchronous expression is usually obtained from the standard library or created by function `async`) are evaluated in asynchronous, non-blocking matter.

This approach provides several benefits to languages as opposed to having these features provided via external libaries, mainly that such library would have to be adopted by other libraries/frameworks in order to be usable and it would still impose additional boilerplate simply because libraries cannot typically change language syntax/semantics. This is why Yatta provides these features from day one, built into language syntax and semantics and therefore it is always available to any program without any external dependencies. At the same time, putting these features directly on the language/runtime level allows for additional optimizations that could otherwise be tricky or impossible.

### Example
Following example will be used as a case study into Yatta's execution model.

```haskell
let
    keys_file = File::open "tests/Keys.txt" {:read}
    values_file = File::open "tests/Values.txt" {:read}

    keys = File::read_lines keys_file
    values = File::read_lines values_file

    () = File::close keys_file
    () = File::close values_file
in
    Seq::zip keys values |> Dict::from_seq
```

In this example, both files are read concurrently, without having to write any additional boiler-plate.

How does Yatta do this? A couple of things: first, check the difference between the [`do`]({{< relref "syntax#do-expression" >}}) and [`let`]({{< relref "syntax#let-expression" >}}) expressions on the [syntax]({{< ref "syntax" >}}) page. They both are used to evaluate multiple steps of computation, however `do` ensures that the steps take place in the same sequence as they are defined, `let` tries to parallelize non-blocking tasks.

Yatta will first perform a **static analysis** of the `let` expression in order to determine **dependencies between aliases/steps**. It knows that `keys_file` and `values_file` are used in the `File::read_lines` function and also knows that `keys` and `values` **do not depend on each other**, and so they **may be ran concurrently**. Yatta doesn't parallelize `keys_file` and `values_file`, nor does it parallelize closing files - even though it seemingly should -  there are no dependencies between these lines either. The trick is that `File::read_lines` is a function that returns a runtime level Promise (hidden from the user), and that means that reading two files in parallel is actually fine, since otherwise Yatta would need to block the execution there, so it can just block on both being read at once.

Another important concept here is that the **order of aliases defined in the `let` expression does matter**. Yatta **doesn't just randomly re-arrange** them based on dependencies alone. If it did, it could for example close those files before they are ever read. That would be incorrect. Yatta just uses static analysis of this expression to determine which aliases can be "batched" and actually batches the execution if they provide underlying Promises. Then the whole expression is transformed into something like this:

1. Execute batch 1 (sequentially, since File::open does not return a Promise):
```haskell
keys_file = File::open "tests/Keys.txt" {:read}
values_file = File::open "tests/Values.txt" {:read}
```

2. Execute batch 2 (in parallel, since File::read_lines does return a Promise):
```haskell
keys = File::read_lines keys_file
values = File::read_lines values_file
```

3. Result from batch 2 is a Promise aggregating both keys and values, which when complete, executes batch 3:
```haskell
() = File::close keys_file
() = File::close values_file
```

4. Finally, the whole let expression is now a Promise, so run the final expression whenever it is ready:
```haskell
Seq::zip keys values |> Dict::from_seq |> println
```
Yatta automatically chains runtime Promises as needed and, more than that, it "unwraps" them whenever they are complete, so the runtime doesn't actually get bloated with propagating Promises all over the place. As soon as a Promise is computed, it becomes just a regular value again.

These concepts allows programmers to focus on expressing concurrent programs much more easily and not having to deal with the details of the actual execution order. Additionally, when code must be executed sequentially, without explicit dependencies, a special expression `do` is available.

### Implementation
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

## Module Loading
See [module loader]({{< ref "module-loader" >}}) page for more details about module loading.
