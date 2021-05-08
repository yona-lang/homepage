---
title: "Resource Management"
---

Yona provides a specialized infrastructure for managing resources, including their initialization and finalization, with guaranteed ordering and error handling.

This feature is called **context managers** and it allows for generic, extensible management of local contexts. Context managers are used in conjunction with the [`with` expression](/syntax#with-expression).

## Context Managers  {: #context-managers}
Context managers provide the necessary gear for dealing with resource management is a structured way, that can be used together with the [`with` expression](/syntax#with-expression).
They provide a way for Yona to initialize and finalize resources. Resources may be explicitly or implicitly named direct access within the scope of the `with` expression.

Module [`context\Local`](/stdlib/context/local) provides functions for implementing custom context managers. Builtin modules, such as [STM](/stdlib/stm) or [File](/stdlib/file) implement context managers as well.

Context managers store their data in a local dictionary, so that they can be used from any threads (this is transparent for programmers, who need not to be aware of this actually).

Context manager is a triple of `(name, wrapper, data)`, where:

* `name` is the default name of the alias, used as a key in the local context dictionary.
* `wrapper` wrapper is a 2-arg function - taking the context manager tuple and a callback that is called from within this wrapper function and of which result is returned as a result of this wrapper. The wrapper can perform initialization and finalization of resources around calling the callback.
* `data` is the actual data, such as transaction object, file object or whatever other resource data to be managed by the context manager.

### Lifecycle of Context Managers
Context manager is executed in this order:

* the context manager tuple is stored to the local context dictionary
* the `wrapper` function is executed, expected to initialize necessary resources, then call the callback passed into this function and finally resources get disposed after they've been used
* if body expression raises an exception, the `leave` function is still called regardless

### Handling errors
Whether an exception is raised or not within the context manager execution is not important for its lifecycle. Context managers ensure that proper initialization and finalization of resources always takes place and for this reason they are the recommended way for handling this sort of thing

### Daemon Context Management
Context managers provide means to manage resources in a consistent and safe way. Just like any other constructions in Yona, the `with` expression produces a return value, that of its body. Sometimes it may be desired not to await this result but still take advantage of the automatic resource management provided by the `with` expression. This is typically the case when the context manager is provided per connection. In this case the calling expression would be limited to handling only one connection at a time.

In these situations it is useful to move the body expression processing to the background and "forget" about it. This can be achieved by putting a `daemon` keyword after the `with` keyword. For more information about the syntax, see the syntax guide for the [`with` expression](/syntax#with-expression).

The result of the `with daemon` expression is therefore `()`, resources will be properly released once the body expression evaluation finishes, nevertheless.
