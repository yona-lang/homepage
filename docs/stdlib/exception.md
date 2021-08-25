---
title: "Exception"
tags: stdlib
---

Exception handling related functionality.

## Usage


### Pretty printing exception stack trace
Function `pretty_print_stacktrace` accepts a stacktrace (third element of a caught exception tuple, see [exceptions syntax](/syntax/#exceptions)). It produces a string, with lines, each representing one stack frame, first being the most recent one.

Example:
```haskell
try
    expression
catch
    (:badarg, error_msg, stacktrace)  -> Exception::pretty_print_stacktrace stacktrace |> IO::println_err
end
```

