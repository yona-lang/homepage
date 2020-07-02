---
title: "Overview"
description: Yatta is a modern take on a dynamic and polyglot general-purpose programming language with advanced functional programming, automatic concurrency, minimalistic ML-like syntax, strict evaluation, for GraalVM polyglot virtual machine (VM).
---

[![Actions Status](https://github.com/yatta-lang/yatta/workflows/Release/badge.svg)](https://github.com/yatta-lang/yatta/actions)
![Test](https://github.com/yatta-lang/yatta/workflows/Test/badge.svg)
![Publish Docker Image](https://github.com/yatta-lang/yatta/workflows/Publish%20Docker%20Image/badge.svg)

[![Gitter](https://badges.gitter.im/yattalang/community.svg)](https://gitter.im/yattalang/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![Google group : yatta-lang](https://img.shields.io/badge/yatta--lang-Google%20group-blue)](https://groups.google.com/forum/#!forum/yatta-lang)

[![Docker Hub](https://img.shields.io/docker/pulls/akovari/yatta)](https://hub.docker.com/r/akovari/yatta)
[![IDEA Plugin](https://img.shields.io/jetbrains/plugin/d/14536-yatta-language?label=IDEA%20Plugin)](https://plugins.jetbrains.com/plugin/14536-yatta-language)

Yatta is a **minimalistic**, **(strongly) dynamically** typed, **parallel** and **non-blocking**, **polyglot**, **strict**, **functional** programming language, with **ML**-like syntax, for the [GraalVM](https://www.graalvm.org/) virtual machine (VM). Yatta puts a strong focus on code readability.

Yatta abstract users from dealing with non-blocking asynchronous computations and parallelism. While these features are commonly available in other languages nowadays, they are almost exclusively non-native solutions that come in forms of libraries or frameworks and are difficult to integrate with existing codebases. On top of that, dealing with these additional libraries requires conscious effort of the programmer to choose/learn/integrate these libraries into their mindset when writing new code.

Follow our [**blog**](https://functional.blog) for release announcements and other reading materials.

## Goals & Priorities
- **Excellent readability** - simple syntax, few keywords, virtually no boilerplate.
- **Few types of expressions** - `module`, `import`, function(function does not need a keyword, it is defined by a name and arguments - patterns), `case`, `if`, `let`, `do`, `with` and `try`/`catch` + `raise`.
- **Simple module system** - ability to expose functions for use in other modules, and ability to import them from other modules. [**Modules**](features/syntax.md#module-expression) are first level values and can be created dynamically.
- **Single expression principle** - program is always one expression - this enables simpler evaluation and syntax, allows writing simple scripts as well as complex applications.
- **Powerful and efficient** built-in **immutable** data structures with full support for [**pattern matching**](features/pattern-matching), including Sequence, Dictionary and Set.
- Custom data types representable as [**records**](features/syntax#records).
- Built-in runtime level **non-blocking asynchronous IO**.
- [**Simple concurrency**](usecase#execution-model), built-into runtime, no need for any abstractions such as Futures, Channels or Actors.
- **Advanced concurrency** provided by built-in [**Software Transactional Memory** (STM)](stdlib/stm) module.
- **Polyglot language** - interoperability with other languages via GraalVM.
- **Powerful resource management** - automatically [**manage resources**](features/resource-management) using built-in context manager infrastructure.

!!! tip
    Read more about the philosophy behind Yatta [here](usecase.md).

## Status
The Yatta language is currently in active development. The release plan is:

* [alpha version](https://github.com/yatta-lang/yatta/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22alpha+release%22) - May 25th 2020. Focus of this release was:
    - [x] stabilized syntax, semantics and runtime
    - [x] automated the release process
    - [x] estabilish website and documentation
    - [x] spread the word and allow users to "play" with the language and the interpreter
    - [x] collect some feedback from interested users
* [beta version](https://github.com/yatta-lang/yatta/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22beta+release%22) - 2020/Q3-Q4. Focus of this release is:
    - [x] finished STM implementation together with a generic `with` [expression](https://github.com/yatta-lang/yatta/issues/33)
    - [ ] significantly grow the standard library
    - [ ] focus on optimizations in the interpreter
* version 1 - final - 2021. Focus of this release is:
    - [ ] stabilize standard library
    - [ ] focus on tooling, such as package management, editor/IDE support
    - [ ] provide a high-quality REPL

See the [release notes](getting_started/release-notes.md) for more details.
