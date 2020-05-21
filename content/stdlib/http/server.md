---
title: "http\\Server"
tags: stdlib
---

This module provides a simple HTTP Server built on top of `com.sun.net.httpserver.HttpServer`.

## Usage
To create a server instance, one must first instantiate it using:

```haskell
server = http\Server::create bind_address port backlog
```

Where the `backlog` is is the maximum number of queued incoming connections to allow on the listening socket. Queued TCP connections exceeding this limit may be rejected by the TCP implementation. If this value is less than or equal to zero, then a system default value is used.

### Handling a client request
Once instantiated, the route handlers should be added to the server instance:

```haskell
server2 = http\Server::handle "/path" :binary handler_function server
```

Function `handle` expects the server path handled by the handler function, then it expects a body encoding symbol (either `:binary` or `:text` - default, utf-8 encoded), which is used to convert the client request body into either a sequence of bytes or a string, then the handler function as specified in the following part and the server object.

Handler function is a function of three arguments, specifically connection params, request headers and request body:
Connection params is a dictionary containing:
* `local_address`
* `protocol`
* `remote_address`
* `method`
* `uri`

Headers is a dictionary containing HTTP request headers and body is a sequence with the request body.
The handler function returns a triple of response code, response headers and response body.

Example handler function:

```haskell
handler = \params headers raw_body -> let
    {"i" = i} = JSON::parse raw_body
in (200, {"content-type" = "application/json"}, JSON::generate {"result" = i * 2})
```

Note that an uncaught exception thrown in the handler function results in server responding with response code 500 and exception details in the body of the response.

### Wrapping it all up and starting a server
Last function of this module is a function `start` which takes the server instance after all routes have been initialized and starts listening for connections.

```haskell
http\Server::start server
```

Using pipe operators, its possible to conveniently define server for example this way:

```haskell
max_connections = 100
server = http\Server::create "127.0.0.1" port max_connections
        |> http\Server::handle "/path" handler
        |> http\Server::start
```

A full example showing use of both HTTP server and client in an application for distributed PI computation can be seen in this [test case](https://github.com/yatta-lang/yatta/blob/master/language/tests/DistributedPiCalculation.yatta).
