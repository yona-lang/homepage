---
title: "socket\\tcp\\Connection"
tags: stdlib
---

This module provides non-blocking functions for reading and writing on TCP connections. Connection can be estabilished either by the [Client](../client) or [Server](../server) module.

## Usage

### Reading from a connection
Function `read_until` accepts a connection and a function of one argument, a byte, that returns a boolean representing whether to read more data from the socket or return whatever has been read so far.

Example:

```haskell
socket\tcp\Connection::read_until connection (\b -> b != 10b)
```

### Reading a line from a connection
Function `read_line` accepts a connection and returns a line of input delimited by a LF (ASCII 10) character.

Example:

```haskell
socket\tcp\Connection::read_line connection
```

### Writing to a connection
Function `write` accepts a connection and a sequence of data (string - sequence of characters, or a sequence of bytes) to be sent to the other side on this connection.

Example:

```haskell
socket\tcp\Connection::write connection "hello"
```
