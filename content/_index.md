---
title: "yatta-lang"
date: 2020-04-25T18:32:16+02:00
draft: false
tldr: Yatta is a modern take on a dynamic general-purpose programming language with advanced functional programming, minimalistic ML-like syntax, strict evaluation, for GraalVM polyglot virtual machine (VM).
---

**Mailing list**: yatta-lang@googlegroups.com

**Chat**: [gitter](https://gitter.im/yattalang/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)


Yatta is a minimalistic, opiniated, (strongly) dynamically typed, strict, functional programming language, with ML-like syntax, for [GraalVM](https://www.graalvm.org/) polyglot virtual machine (VM). Yatta puts a strong focus on readability of the code.

Yatta abstract users from dealing with non-blocking asynchronous computations and parallelism. While these features are commonly available in other languages nowadays, they are almost exclusively non-native solutions that come in forms of libraries or frameworks and are difficult to integrate with existing codebases. On top of that, dealing with these additional libraries requires conscious effort of the programmer to choose/learn/integrate these libraries into their mindset when writing new code.

# Documentation
Please see documentation Yatta's regarding the [standard library]({{< ref "/stdlib/overview" >}}). Plenty of demos can be found in [tests](https://github.com/yatta-lang/yatta/tree/master/language/tests).

Polyglot usage is explained in this [section]({{< ref "/polyglot" >}}).

# Status
Yatta language is currently in active development. The release plan is:
* [alpha version](https://github.com/yatta-lang/yatta/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22alpha+release%22) - 2020/Q2
* [beta version](https://github.com/yatta-lang/yatta/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22beta+release%22) - 2020/Q3-Q4
* version 1 - final - 2021

# Contributors

* [Adam Kovari](https://github.com/akovari)
* [Fedor Gavrilov](https://github.com/kurobako)

# Getting Started
Follow instructions on [Github](https://github.com/yatta-lang/yatta).

# Examples

## Distributed calculation of PI using builtin HTTP Server and Client

    do
        port = 3000
        server = do
                iteration_handler = \params headers raw_body -> let
                    {"i" = i} = JSON::parse raw_body
                in (200, {"content-type" = "application/json"}, JSON::generate {"result" = (-1f ** (i + 1f)) / ((2f * i) - 1f)})

                max_connections = 100
                server = http\Server::create "127.0.0.1" port max_connections
                    |> http\Server::handle "/iteration" iteration_handler
                    |> http\Server::start
            end

        client_session = http\Client::session {}
        client_module = module Client exports run as
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

        4f * client_module::run 1 0f
    end

## Interoperability with Java

    let
        big_integer = Java::type "java.math.BigInteger"
        big_two = Java::new big_integer ["2"]
        big_three = Java::new big_integer ["3"]
        result = big_two::multiply big_three  # calling a method `multiply` on object of type BigInteger.
    in result::longValue
