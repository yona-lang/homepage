---
title: "socket\\tcp\\Client"
tags: stdlib
---

This module provides non-blocking TCP client.

## Usage
Use function `connect` to obtain a context manager that represents an estabilished TCP connection to the server. Use the [Connection] module from this package to perform `read` and `write` operations on this connection.

Example:

```haskell
with socket\tcp\Client::connect "localhost" 5555 as connection
    socket\tcp\Connection::write connection "hello"
end
```
