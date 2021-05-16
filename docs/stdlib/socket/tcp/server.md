---
title: "socket\\tcp\\Server"
tags: stdlib
---

This module provides non-blocking TCP server.

## Usage
This module provides functions for opening a TCP channel and accepting connections.

### Creating a TCP channel
Function `channel` accepts a tuple of configuration arguments and returns a context manager representing this channel.

The tuple format for a TCP channel is: `(:tcp, address, port)` where the `address` and `port` is the address and port to bind on.

Example:

```haskell
with socket\tcp\Server::channel (:tcp, addr port) as channel
    do
        IO::println "listening on {addr}:{port}"
        infi (\-> accept channel)
    end
end
```

### Accepting a client connection
Function `accept` requires a channel context and returns a context manager representing a connection as soon as it has been estabilished.

Example:
```haskell
with daemon socket\tcp\Server::accept channel as connection
    do
       socket\tcp\Connection::write connection "welcome: "
       request = socket\Connection::read_line connection |> Seq::decode
       IO::println request
       request |> socket\Connection::write connection
    end
end
```

!!! tip
    Note the use of the `daemon` in the example above. In this way the handling of the client is not delaying the return of this [`with`](/features/resource-management#daemon-context-management) expression. This is a useful to handle multiple clients concurrently.
