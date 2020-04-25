---
title: "Simple HTTP Server"
date: 2020-04-25T18:37:33+02:00
draft: false
tags: stdlib
---

This module provides a simple HTTP Server built on top of `com.sun.net.httpserver.HttpServer`.

## Usage
To create a server instance, one must first instantiate it using:

    server = http\Server::create bind_address port backlog

Where the `backlog` is is the maximum number of queued incoming connections to allow on the listening socket. Queued TCP connections exceeding this limit may be rejected by the TCP implementation. If this value is less than or equal to zero, then a system default value is used.

Once instantiated, the route handlers should be added to the server instance:

    server2 = http\Server::handle "/path" handler_function server

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

    handler = \params headers raw_body -> let
        {"i" = i} = JSON::parse raw_body
    in (200, {"content-type" = "application/json"}, JSON::generate {"result" = i * 2})

Note that an uncaught exception thrown in the handler function results in server responding with response code 500 and exception details in the body of the response.

Last function of this module is a function `start` which takes the server instance after all routes have been initialized and starts listening for connections.

    http\Server::start server

Using pipe operators, its possible to conveniently define server for example this way:

    max_connections = 100
    server = http\Server::create "127.0.0.1" port max_connections
          |> http\Server::handle "/path" handler
          |> http\Server::start

A full example showing use of both HTTP server and client in an application for distributed PI computation can be seen in this [test case](https://github.com/yatta-lang/yatta/blob/master/language/tests/DistributedPiCalculation.yatta).
