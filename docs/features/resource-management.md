---
title: "Resource Management"
---

Yatta provides specialized infrastructure for managing resources, including their initialization and finalization, with guaranteed ordering and error handling.

This feature is called **context managers** and it allows for generic, extensible management of local contexts. Context managers are used in conjunction with the [`with` expression](/features/syntax#with-expression).

## Context Managers  {: #context-managers}
Context managers provide the necessary gear for dealing with resource management is a structured way, that can be used together with the [`with` expression](/features/syntax#with-expression).
They provide a way for Yatta to initialize and finalize resources. Resources may be explicitly or implicitly named direct access within the scope of the `with` expression.

Module [`context\Local`](/stdlib/context/local) provides functions for implementing custom context managers. Builtin modules, such as [STM](/stdlib/stm) or [File](/stdlib/file) implement context managers as well.

Context managers store their data in a local dictionary, so that they can be used from any threads (this is transparent for programmers, who need not to be aware of this actually).

Context manager is a tripple of `(name, wrapper, data)`, where:

* `name` is the default name of the alias, used as a key in the local context dictionary.
* `wrapper` wrapper is a 2-arg function - taking the context manager tuple and a callback that is called from within this wrapper function and of which result is returned as a result of this wrapper. The wrapper can perfrom initialization and finalization of resources around calling the callback.
* `data` is the actual data, such as transaction object, file object or whatever other resource data to be managed by the context manager.

### Lifecycle of Context Managers
Context manager is executed in this order:

* the context manager tuple is stored to the local context dictionary
* the `wrapper` function is executed, expected to initialize necessary resources, then call the callback passed into this function and finally resources get disposed after they've been used
* if body expression raises an exception, the `leave` function is still called regardless

### Handling errors
Whether an exception is raised or not within the context manager execution is not important for the its lifecycle. Context managers ensure that proper initialization and finalization of resources always takes place and for this reason they are the recommended way for handling this sort of things.
