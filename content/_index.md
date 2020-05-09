---
title: "yatta-lang"
date: 2020-04-25T18:32:16+02:00
draft: false
tldr: Yatta is a modern take on a dynamic general-purpose programming language with advanced functional programming, minimalistic ML-like syntax, strict evaluation, for GraalVM polyglot virtual machine (VM).
---

**Mailing list**: yatta-lang@googlegroups.com

Yatta is a minimalistic, opiniated, (strongly) dynamically typed, strict, functional programming language, with ML-like syntax, for the [GraalVM](https://www.graalvm.org/) polyglot virtual machine (VM). Yatta puts a strong focus on code readability.

Yatta abstract users from dealing with non-blocking asynchronous computations and parallelism. While these features are commonly available in other languages nowadays, they are almost exclusively non-native solutions that come in forms of libraries or frameworks and are difficult to integrate with existing codebases. On top of that, dealing with these additional libraries requires conscious effort of the programmer to choose/learn/integrate these libraries into their mindset when writing new code.

## Goals & Priorities
- **Excellent readability** - simple syntax, few keywords, virtually no boilerplate.
- **Few types of expressions** - `module`, `import`, function(function does not need a keyword, it is defined by a name and arguments - patterns), `case`, `if`, `let`, `do` and `try`/`catch` + `raise`.
- **Simple module system** - ability to expose functions for use in other modules, and ability to import them from other modules. Modules are first level values and can be created dynamically.
- **Single expression principle** - program is always one expression - this enables simpler evaluation and syntax, allows writing simple scripts as well as complex applications.
- **Powerful and efficient** built-in data structures with full support for pattern matching, including Sequence, Dictionary and Set.
- Custom data types representable as **records**.
- Built-in runtime level **non-blocking asynchronous IO**.
- **Simple concurrency**, built-into runtime, no need for any abstractions such as Futures, Channels or Actors.
- **Advanced concurrency** provided by built-in **Software Transactional Memory** (STM) module.
- **Polyglot language** - interoperability with other languages via GraalVM.

## Documentation
Documentation regarding language syntax, execution model, standard library and polyglot usage is available via the [documentation]({{< ref "/docs" >}}) section.

To get quickly started, please follow the installation [instructions]({{< ref "/docs/installation" >}}) and follow the the [blog](https://functional.blog) to get updates regarding learning materials.

## Status
The Yatta language is currently in active development. The release plan is:
* [alpha version](https://github.com/yatta-lang/yatta/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22alpha+release%22) - 2020/Q2
* [beta version](https://github.com/yatta-lang/yatta/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22beta+release%22) - 2020/Q3-Q4
* version 1 - final - 2021

## Contributors

* [Adam Kovari](https://github.com/akovari)
* [Fedor Gavrilov](https://github.com/kurobako)
* [Daniele Consoli](https://github.com/ktzee)

## Getting Started
Follow instructions on [Github](https://github.com/yatta-lang/yatta).

## Examples

### Distributed calculation of PI using builtin HTTP Server and Client

```haskell
    do
        port = 3000
        server = do
                iteration_handler = \params headers raw_body -> let
                    {"i" = i} = JSON::parse raw_body
                in (200, {"content-type" = "application/json"}, JSON::generate {"result" = (-1f ** (i + 1f)) / ((2f * i) - 1f)})

                max_connections = 100
                http\Server::create "127.0.0.1" port max_connections
                |> http\Server::handle "/iteration" iteration_handler
                |> http\Server::start
            end

        client_session = http\Client::session {}
        client = module Client exports run as
            send_request i = do
                    (200, _, raw_body) = http\Client::post client_session "http://localhost:{port}/iteration" {} <| JSON::generate {"i" = i}
                    {"result" = result} = JSON::parse raw_body
                    result
                end

            max_iterations = 1000
            run i acc
            | i >= max_iterations = acc
            | true = run (i + 1) (acc + (send_request <| float i))
        end

        4f * client::run 1 0f
    end
```

### Interoperability with Java

```haskell
    let
        big_integer = Java::type "java.math.BigInteger"
        big_two = Java::new big_integer ["2"]
        big_three = Java::new big_integer ["3"]
        result = big_two::multiply big_three  # calling a method `multiply` on object of type BigInteger.
    in result::longValue
```
