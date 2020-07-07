---
title: "About Yatta"
---

## Philosophy
For those familiar with other functional programming languages, such as Lisp, Haskell or Erlang, most of the concepts mentioned in this documentation are not totally alien. This section would nevertheless focus on why particular features are present in Yatta while others are not. For readers more familiar with other paradigms, such as object oriented or imperative programming, this section should be about eliminating some concerns users of these languages might have.

Yatta does not support **imperative programming**, as that requires explicit control flow and state manipulation. An example for that is a loop with a counter, that gets increased at each step. Yatta does not have variables that could be modified, nor does it have "loops" in the imperative sense. Instead [recursion or generators](syntax.md#loops-recursion-and-generators) may be used for iteration.

When it comes to **object oriented** features, it's important to separate two things. One is that most common feature of object oriented languages, encapsulation, in the traditional sense is about hiding mutable state. As previously stated, Yatta doesn't have mutable state, so hiding it in an object doesn't provide much of a value. Yatta is a lexically scoped language though and fully supports hiding internal implementation details either via scoping expressions (`let`, `do` or `case`), or using closures as well (lambdas). This means that a particular alias or a name is only available within the scope in which it is defined. Moreover, [modules](syntax.md#module-expression) allow user to expose only certain functions, and not others. This is a form of interface concept, as it is known in the object oriented world.

Polymorophism can take various shapes in Yatta. First obvious option that comes to mind, is using pattern matching. A function can be defined for different patterns and in that way replicate polymorphic methods from object oriented languages. Another possibility could be having multiple modules defined, with the same interface (exposed functions) and switch them dynamically based on any arbitrary conditions the user needs. There is a lot of flexibility in Yatta due to its dynamic nature.

When it comes to **statically typed functional** languages, such as Haskell or Elm, one might wonder about things like purity, type classes or Algebraic Data Types (ADTs). Yatta is not a pure language, in the sense that it does not enforce separation of side effects and pure computations (side-effect free). Side effects represent operations modifying an external state, for example some kind of a global state, input/output, etc. 

Type classes have no real counterpart in Yatta since Yatta is dynamically typed. What would otherwise be achieved using type classes, would have to be achieved by a convention of sorts in Yatta, for example that there would be an expectation of certain modules to export certain interface.

ADTs in Yatta do not need to be defined upfront, as there is no compilation phase to check their validity or perform exhaustivness checks (where a compiler ensures that all possible cases are being tested in a `case` expression). That doesn't mean that ADTs cannot be modeled in Yatta, they can, they just won't have a static type check performed on them before running the program. They may be represented using [symbols](features/data-types.md), [tuples](features/data-types.md) or [records](syntax.md#records), or any combination of those.

## Execution Model
Yatta provides fully transparent runtime system that integrates asynchronous non-blocking IO features with concurrent execution of the code. This means, there is no special syntax or special data types representing asynchronous computations. Everything related to non-blocking IO is hidden within the runtime and exposed via the standard library, and all expressions consisting of asynchronous expressions(an asynchronous expression is usually obtained from the standard library or created by function [`async`](stdlib/functions/async.md)) are evaluated in asynchronous, non-blocking matter.

This approach provides several benefits to languages as opposed to having these features provided via external libaries, mainly that such library would have to be adopted by other libraries/frameworks in order to be usable and it would still impose additional boilerplate simply because libraries cannot typically change language syntax/semantics. This is why Yatta provides these features from day one, built into language syntax and semantics and therefore it is always available to any program without any external dependencies. At the same time, putting these features directly on the language/runtime level allows for additional optimizations that could otherwise be tricky or impossible.

### Example
Following example will be used as a case study into Yatta's execution model. Note that this example code actually will not work from Yatta [0.8.1](/getting_started/release-notes#yatta-0.8.1), because of the [resource management](features/resource-management.md) feature, but it is still a good example for understanding the underlying evaluation model. Make sure to read the next section after this one to see how this program needs to be written in more recent Yatta versions. The same properties of the evaluation still apply!

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

How does Yatta do this? A couple of things: first, check the difference between the [`do`](syntax.md#do-expression) and [`let`](syntax.md#let-expression) expressions on the [syntax](syntax.md) page. They both are used to evaluate multiple steps of computation, however `do` ensures that the steps take place in the same sequence as they are defined, `let` tries to parallelize non-blocking tasks.

Yatta will first perform a **static analysis** of the `let` expression in order to determine **dependencies between aliases/steps**. It knows that `keys_file` and `values_file` are used in the [`File::read_lines`](stdlib/file.md#read-lines) function and also knows that `keys` and `values` **do not depend on each other**, and so they **may be ran concurrently**. Yatta doesn't parallelize `keys_file` and `values_file`, nor does it parallelize closing files - even though it seemingly should -  there are no dependencies between these lines either. The trick is that [`File::read_lines`](stdlib/file.md#read-lines) is a function that returns a runtime level Promise (hidden from the user), and that means that reading two files in parallel is actually fine, since otherwise Yatta would need to block the execution there, so it can just block on both being read at once.

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
Seq::zip keys values |> Dict::from_seq |> IO::println
```
Yatta automatically chains runtime Promises as needed and, more than that, it "unwraps" them whenever they are complete, so the runtime doesn't actually get bloated with propagating Promises all over the place. As soon as a Promise is computed, it becomes just a regular value again.

These concepts allows programmers to focus on expressing concurrent programs much more easily and not having to deal with the details of the actual execution order. Additionally, when code must be executed sequentially, without explicit dependencies, a special expression `do` is available.

#### Simplifying the example by using resource managemenet
This example that concurrenly reads two files, line by line, and produces pairs of them as a dictionary can be implemented in Yatta using its [resource management](features/resource-management.md) features, as of Yatta 0.8.1.

Shorter version, equivalent in terms of features, but actually properly handling errors would look like:

```haskell
let
    keys   = with File::open "tests/Keys.txt" {:read}   as keys_file    File::read_lines keys_file   end
    values = with File::open "tests/Values.txt" {:read} as values_file  File::read_lines values_file end
in
    Seq::zip keys values |> Dict::from_seq
```

Note that if error arises in the original example, files would not be closed. In this improved example they would. Otherwise, everything else regarding the non-blocking and parallel nature of this code stands.

### Implementation
In terms of implementation, the runtime system of Yatta can be viewed in terms of promise pipelineing or call-streams. The difference is that this pipelining and promise abstraction as such is completely transparent to the programmer and exists solely on the runtime level.

## Evaluation
Evaluation of an Yatta program consists of evaluating a single expression. This is important, because everything, including module definitions are simple expressions in Yatta.

Module loader then takes advantage of this principle, knowing that an imported module will be a file defining a module expression. It can simply evaluate it and retrieve the module itself.
