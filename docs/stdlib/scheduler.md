---
title: "Scheduler"
tags: stdlib
---

Simple scheduling of tasks.

## Usage
Use this module to schedule task execution.

### Scheduling a repeating task
Function `run` accepts two argument:

- `how_often` a tuple representing how often the task should be repeated
- `what` a 0-argument function to be run in the provided period

Depending on the `how_often`, the first execution is delayed until the next occurrence of this period. In other words, the first invocation of `(:every, :minute)` will happen in the next exact minute, if it's 13:32:40 now, then the first invocation is `13:33:00`. If it's `(:every, :day)`, then the first run will be scheduled at `00:00:00` the next day.

#### Specification of `how_often` options
Frequency of the execution is specified as a tuple like `(:every, time_unit)`, where `time_unit` is one of:

- `:second`
- `:minute`
- `:hour`
- `:day`
- `:week`

Examples:
```haskell
Scheduler::run (:every, :minute) (\-> IO::println "hello")  # print hello every minute at 0 seconds
```
