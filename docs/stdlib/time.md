---
title: "Time"
tags: stdlib
---

This module provides functions for working with time.

### Obtaining current time since epoch, in seconds
Function `unix` returns current time since the epoch in seconds.

Example:
```
Time::unix |> IO::println
```

