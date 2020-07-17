---
title: "context\\Local"
tags: stdlib
---

This module provides functions for implementing user-level [context managers](/features/resource-management.md#context-managers). It provides necessary functions that allow reading the local context dictionary as well as convenience functions for constructing and deconstructing the context manager tuples.

## Creating a new custom context manager
Function `new` takes four arguments and builds a new context manager tuple:

* `name` is the default name of the alias, used as a key in the local context dictionary.
* `wrapper` wrapper: 2-arg function - the context manager tuple and a callback that is called from within this wrapper function and of which result is returned as a result of this wrapper. The wrapper can perfrom initialization and finalization of resources around calling the callback.
* `data` is the actual data, such as transaction object, file object or whatever other resource data to be managed by the context manager.

This example builds a simple test context manager that multiplies its `data` within its `wrapper` function, provided that `4` is its initial `data`.
```haskell
context\Local::new "test_context" (\ctx_mgr cb -> (cb) * 2) 4
```

## Looking up a value in the local context dictionary
Function `lookup` returns a value in the local context dictionary by the key (string), or `()` if not found.

## Checking whether a key is present in the local context dictionary
Function `contains` returns true if the key (string) is present in the local context dictionary, or not.

## Getting the default name of the context manager
Obtaining the `name` from the context manager tuple can be done using function `get_name`. It takes context manager tuple as its only argument and returns the `name`.

## Getting the initialization function of the context manager
Obtaining the `wrapper` from the context manager tuple can be done using function `get_wrapper`. It takes context manager tuple as its only argument and returns the `wrapper` function.

## Getting the data of the managed resource of the context manager
Obtaining the `data` from the context manager tuple can be done using function `get_data`. It takes context manager tuple as its only argument and returns the `data` function.

Using this context manager in a simple test program coul look like:

```haskell
with context\Local::new "test_context" (\ctx_mgr cb -> (cb) * 2) 4 as test_context
    # name:  "test_context"
    # wrapper: \cm -> (context\Local::get_data cm) * 2
    # data:  4
    let data = context\Local::get_data test_context in data * 2
end
```
