---
title: "Release Notes"
---

## Yona 0.8.3-SNAPSHOT: {: #yona-0.8.3}
* GraalVM 21.2.0-java-16 is required now, see [installation](/getting_started/installation/) page for details
* [terminal\\Colors](/stdlib/terminal/colors/), [terminal\\colors\\Background](/stdlib/terminal/colors/background/) and [terminal\\colors\\Foreground](/stdlib/terminal/colors/foreground/): new standard library modules for terminal related functionality

## IDEA Plugin 0.0.4 - June 13th 2021 {: #idea-plugin-0.0.4}
- fixed issues with syntax highlighting in string interpolation
- brace matcher
- commenter support

## Yona 0.8.2-SNAPSHOT: {: #yona-0.8.2}
* TCP Socket [Client](/stdlib/socket/tcp/client/), [Server](/stdlib/socket/tcp/server/), [Connection](/stdlib/socket/tcp/connection/) modules
* Updated [Seq](/stdlib/seq/), [Set](/stdlib/set/) and [Dict](/stdlib/dict/) to match Haskell's `fold` and `reduce` interface and also some new functions
* [Stopwatch](/stdlib/stopwatch/) module
* Benchmarks and examples available in the source repository
* Improved REPL experience
* [Deamon context managers](/features/resource-management#daemon-context-management)
* GraalVM 21.1.0-java-16 is required now, see [installation](/getting_started/installation/) page for details
* Added ability to "execute" a module that exports a zero argument function called `main`

## Yona 0.8.1-SNAPSHOT: {: #yona-0.8.1}
* [resource management](/features/resource-management/)
* [STM](/stdlib/stm/) module
* [Regexp](/stdlib/regexp/) module
* [IO](/stdlib/io/) module now containing `println`, `read` and `readln` functions
* REPL, with syntax highlighting, autocomplete, history, etc.
* GraalVM 20.3.0 is required now, see [installation](/getting_started/installation/) page for details

## IDEA Plugin 0.0.1 - June 17th 2020  {: #idea-plugin-0.0.1}
* first public release available from [JetBrains Marketplace](https://plugins.jetbrains.com/plugin/14536-yona-language)
* basic syntax highlighting

## Yona 0.8.0: alpha version - May 25th 2020  {: #yona-0.8.0}
* first public release
* release available from GiHub [releases](https://github.com/yona-lang/yona/releases/tag/0.8.0)
