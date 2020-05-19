---
title: "never"
tags: stdlib
---

Function `never` is a function that is never completed.

## Usage
Because of Yatta's non-blocking and asynchronous nature, it may not be obvious how to write a program that never completes. One common usecase here would be when writing an HTTP server, then a program should never stop. Since the HTTP server itself is a non-blocking function, after it is created, Yatta program would just end, without using this function. Additionally, this function doesn't take any CPU time.

```haskell
do
    port = 3000
    server = do
        double_handler = \params headers raw_body -> let
            {"i" = i} = JSON::parse raw_body
        in (200, {"content-type" = "application/json"}, JSON::generate {"result" = 2 * i})

        max_connections = 100
        http\Server::create "127.0.0.1" port max_connections
        |> http\Server::handle "/double" iteration_handler
        |> http\Server::start
    end
    never
end
```
