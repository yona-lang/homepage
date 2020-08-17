---
title: "STM"
tags: stdlib
---

Module `STM` provides access to a built-in Software Transactional Memory data structure, which can be thought of as a mutable map, safe from concurrent access.

## Usage
There may be multiple instances of STM created in Yona, and they live completely independently to each other. Any error raised by functions in this module raise an exception of type `:stm`.
Any expression inside the body of a [context manager](/features/resource-management.md#context-managers) will have implicit access to the transaction from its scope.

### Instantiating a new STM
In order to use an STM for read/write operations, one must first create the STM itself. Reference to the STM instance will be used by all other functions. STM itself is of a special [type](/features/data-types.md) `stm`.

Example:
```haskell
stm = STM::new
```

### Creating a var - key for some value in the STM
STM uses concepts of vars, which are references to the STM, and they are actually their own special type in Yona.

Creating a new var:
```haskell
var = STM::var stm initial_value
```

Creating a new var requires the STM instance and an initial value this var holds.

### Starting a read-only transaction
Function `read_tx` creates a read-only transaction wrapped in a [context manager](/features/resource-management.md#context-managers). It takes a reference to the STM system and returns the context manager.

Access the STM in a transaction:
```haskell
with STM::read_tx stm
    STM::run (\-> check_balance)
end
```

### Starting a read-write transaction
Function `write_tx` creates a read-write transaction wrapped in a [context manager](/features/resource-management.md#context-managers). It takes a reference to the STM system and returns the context manager.

Access the STM in a transaction:
```haskell
with STM::write_tx stm
    STM::run (\-> withdraw)
end
```

Note that passing a zero argument lambda [requires](/features/syntax.md#anonymous-functions-aka-lambdas) it to be wrapped in another lambda, this is to prevent its eager evaluation.

### Reading a value of a var
A var may be read either inside or outside of a transaction. Reading a var outside of a transaction, aka a dirty read, may provide a value that has been modified by another running transaction and not yet committed. Funciton `read` expects one argument, the var and returns a value this var contains.

Example:
```haskell
value = STM::read var
```

### Writing to a var
Function `var` must be called within a transaction. It expects var and the new value to be written. This function returns a `()`.

Example:
```haskell
STM::write var new_value
```

### Protecting a var from a concurrent write in another transaction
Function `protect` ensures that a var cannot be overriden from another transaction. If such change did occur, the current transaction would fail. This function expects a var and returns a `()`.

Example:
```haskell
STM::protect var
```

## Complete example
The following example shows a simple program for withdrawing money from an account. 10 currency is withdrawn 100 times:

```haskell
let
    stm = STM::new
    balance = STM::var stm 1000f

    max_iterations = 100

    run = \i -> case i of
      x | x >= max_iterations -> STM::read balance
      x -> do
        with STM::write_tx stm
            let
                old_balance = STM::read balance
            in
                STM::write balance (old_balance - 10f)
        end
        
        run (i + 1)
      end
    end
in
    run 0
```
