---
title: "System"
tags: stdlib
---

This module provides functions for interacting with the system environment, such as external processes, or environment variables.
## Usage

### Accessing program arguments
Function `args` allows the program to access the process arguments, so for example running a script such as:

```bash
yatta ./script.yatta -h
```

would result in:

```haskell
["-h"] = System::args
```

### Getting PID
Function `pid` returns the PID of the running process, as an integer number.

```haskell
pid = System::pid
```

### Getting environment variable
Function `get_env` returns an environment variable as a string or a `()` if such environment variable is not present.

```haskell
yatta_path = System::get_env "YATTA_PATH"
```

### Running an external process
Function `run` is used to run an external process. It takes a sequence of strings, of which the first is the process to run and following strings are arguments for that process. The process will be executed in a non-blocking matter, and the result of this function is a triple of `(exit_code, std_out, std_err)`, where
* `exit_code` is the exit code of the executed process
* `std_out` is captured standard output of the executed process
* `std_err` is captured standard error output of the executed process

Example:
```haskell
System::run ["echo", "hello world"]
```

### Running a pipeline of external process
Function `pipeline` is used to run a pipeline of external processes. The process will be executed in a non-blocking matter, and the result of this function is a triple of `(exit_code, std_out, std_err)`, where
* `exit_code` is the exit code of the last executed process
* `std_out` is captured standard output of the last executed process
* `std_err` is captured standard error output of the last executed process

Processes are linked by their standard input and output streams.

Example which will reverse a string and produce `"olleh"` as a result:
```haskell
System::pipeline [
    ["echo", "hello"],
    ["rev"]
]
```
