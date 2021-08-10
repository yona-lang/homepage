---
title: "Yona Language"
description: Yona is a modern take on a dynamic and polyglot general-purpose programming language with advanced functional programming, automatic concurrency, minimalistic ML-like syntax, strict evaluation, for GraalVM polyglot virtual machine (VM).
---

[![Build Status](https://travis-ci.org/yona-lang/yona.svg?branch=master)](https://travis-ci.org/yona-lang/yona)
[![Latest Release](https://img.shields.io/github/v/release/yona-lang/yona)](https://github.com/yona-lang/yona/releases/latest/)
[![Docker Pulls](https://img.shields.io/docker/pulls/akovari/yona)](https://hub.docker.com/r/akovari/yona)
[![IDEA Plugin](https://img.shields.io/jetbrains/plugin/d/14917-yona-language?label=IDEA%20Plugin)](https://plugins.jetbrains.com/plugin/14917-yona-language)
![GitHub](https://img.shields.io/github/license/yona-lang/yona)
[![Twitter](https://img.shields.io/twitter/url/https/twitter.com/kovariadam.svg?style=social&label=Follow%20%40kovariadam)](https://twitter.com/kovariadam)

<iframe frameborder="none" width="245px" height="48px" src="https://plugins.jetbrains.com/embeddable/install/14917"></iframe>

[![Gitter](https://badges.gitter.im/yona/community.svg)](https://gitter.im/yonalang/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![Google group : yona-lang](https://img.shields.io/badge/yona--lang-Google%20group-blue)](https://groups.google.com/forum/#!forum/yona-lang)

Yona is a **minimalistic**, **(strongly) dynamically** typed, **parallel** and **non-blocking**, **polyglot**, **strict**, **functional** programming language, with **ML**-like syntax, for the [GraalVM](https://www.graalvm.org/) virtual machine (VM). Yona puts a strong focus on code readability.

Yona abstract users from dealing with non-blocking asynchronous computations and parallelism. While these features are commonly available in other languages nowadays, they are almost exclusively non-native solutions that come in forms of libraries or frameworks and are difficult to integrate with existing codebases. On top of that, dealing with these additional libraries requires a conscious effort of the programmer to choose/learn/integrate these libraries into their mindset when writing new code.

Follow our [**blog**](https://functional.blog) for release announcements and other reading materials.

## Goals & Priorities
- **Excellent readability** - simple syntax, few keywords, virtually no boilerplate.
- **Few types of expressions** - `module`, `import`, function(function does not need a keyword, it is defined by a name and arguments - patterns), `case`, `if`, `let`, `do`, `with` and `try`/`catch` + `raise`.
- **Simple module system** - ability to expose functions for use in other modules, and ability to import them from other modules. [**Modules**](syntax.md#module-expression) are first level values and can be created dynamically.
- **Single expression principle** - program is always one expression - this enables simpler evaluation and syntax, allows writing simple scripts as well as complex applications.
- **Powerful and efficient** built-in **immutable** data structures with full support for [**pattern matching**](features/pattern-matching), including Sequence, Dictionary and Set.
- Custom data types representable as [**records**](syntax#records).
- Built-in runtime level **non-blocking asynchronous IO**.
- [**Simple concurrency**](about#execution-model), built-into runtime, no need for any abstractions such as Futures, Channels or Actors.
- **Advanced concurrency** provided by built-in [**Software Transactional Memory** (STM)](stdlib/stm) module.
- **Polyglot language** - interoperability with other languages via GraalVM.
- **Powerful resource management** - automatically [**manage resources**](features/resource-management) using built-in context manager infrastructure.

!!! tip
    Read more about the philosophy behind Yona [here](about.md).

## Status
The Yona language is currently in active development. The release plan is:

* [![https://img.shields.io/github/milestones/progress/yona-lang/yona/1](https://img.shields.io/github/milestones/progress/yona-lang/yona/1)](https://github.com/yona-lang/yona/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22alpha+release%22) May 25th 2020. The focus of this release was:
    - [x] stabilized syntax, semantics, and runtime
    - [x] automated the release process
    - [x] establish website and documentation
    - [x] spread the word and allow users to "play" with the language and the interpreter
    - [x] collect some feedback from interested users
* [![https://img.shields.io/github/milestones/progress/yona-lang/yona/2](https://img.shields.io/github/milestones/progress/yona-lang/yona/2)](https://github.com/yona-lang/yona/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22beta+release%22) 2021/Q4. Focus of this release is:
    - [x] finished [STM](stdlib/stm) implementation
    - [x] resource management: `with` [expression](features/resource-management)
    - [x] [regular expressions](stdlib/regexp)
    - [x] Intellij [plugin](https://plugins.jetbrains.com/plugin/14917-yona-language)
    - [ ] significantly grow the standard library
    - [ ] focus on optimizations in the interpreter
* [![https://img.shields.io/github/milestones/progress/yona-lang/yona/3](https://img.shields.io/github/milestones/progress/yona-lang/yona/3)](https://github.com/yona-lang/yona/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22beta+release%23) final - 2022. Focus of this release is:
    - [ ] stabilize standard library
    - [ ] focus on tooling, such as package management, editor/IDE support
    - [x] provide a high-quality REPL

See the [release notes](getting_started/release-notes.md) for more details.

<!--
[Change cookie settings](#__consent){ .md-button }
-->

