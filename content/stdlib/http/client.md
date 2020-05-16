---
title: "Simple HTTP Client"
tags: stdlib
---

This module provides a simple HTTP Client built on top of [java.net.http.HttpClient](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpClient.html).
## Usage

First you need to create an HTTP session instance, that takes a dictionary with optional settings, such as whether redirects should be followed or an HTTP authentication should be used. This session instance is then used to make HTTP requests.

Example:

```haskell
session = http\Client::session {}  # this will create a session without any additional configuration
```

Another example:

```haskell
session = http\Client::session {:authenticator = (:password, "username", "password"), :follow_redirects = true}  # this will initiate a new session, with additional `authenticator` and `follow_redirects` settings
```


To make HTTP requests, following functions are available:

```haskell
(status, headers, body) = http\Client::get session "<url>" {}  # where the dictionary can be used to pass HTTP headers
(status, headers, body) = http\Client::delete session "<url>" {}  # where the dictionary can be used to pass HTTP headers
(status, headers, body) = http\Client::post session "<url>" {} []  # where the dictionary can be used to pass HTTP headers, and the last argument is request body (empty string in this case)
(status, headers, body) = http\Client::put session "<url>" {} []  # where the dictionary can be used to pass HTTP headers, and the last argument is request body (empty string in this case)
```


A complete example using HTTP authorization:

```haskell
let
    session = http\Client::session {:authenticator = (:password, "test", "test")}
    (200, headers, body) = http\Client::get session "https://httpbin.org/basic-auth/test/test" {}
    {"user" = user, "authenticated" = true} = JSON::parse body
in
    user
```

which uses [httpbin](https://httpbin.org/) testing service to test HTTP authentication.

Another example to test request headers, using the same service:

```haskell
let
    session = http\Client::session {}
    (200, headers, body) = http\Client::get session "https://httpbin.org/headers" {:accept = "application/json"}
    {"headers" = response_headers} = JSON::parse body
in
    Dict::lookup response_headers "Accept"
```
