# Introduction
Embla is a server side framework for the Dart VM. It provides an extendable plugin architecture,
as well as a robust HTTP transport plugin to itself.

## Why use Embla?
Managing server side apps in Dart can be a bit of a hassle. You'll either have to do it all by
yourself, using the built in `dart:io` library. Or you can pick and choose between all the different
packages over at [Pub](https://pub.dartlang.org), to create your own set up.

And there are great libraries out there. One of the most popular packages for servers is
[Shelf](https://pub.dartlang.org/packages/shelf). It is a lightweight library building on the
concept of a middleware pipeline.

## Middleware Pipeline
An HTTP server works by listening to a port on its host machine. Messages sent to that port are
shoved into the application, expecting to be sent a message back. We talk about requests and
responses.

Different requests want different things in response, and there are multiple ways in which HTTP
specifies distinction between requests. The URI is one of them. The method (or verb) is another.

When the request enters the application, it is up to the server to—depending on the
request—delegate to the correct resource or function which will provide the correct response.

Shelf talks about middleware and handlers. A handler is used to convert a Request to a Response.
Middleware are intermediate "pipes" which can interfere with or change the request or response.

Embla proposes a different, but not incompatible, concept. Think of the Embla Middleware Pipeline
as a tree. Each end of a branch is considered a failure to deliver a response. These endpoints
throw a `NoResponseFromPipelineException`.

The branches are made up of middleware. Each middleware receives a request. It can either respond
with a response object, or delegate to the next middleware. But it can also choose to send the
request down a different path, a separate pipeline. This creates a new branch in the tree.

In Embla, everything is a middleware! Consider a "route". No matter what kind of router you
are used to, what it boils down to is simply a layer of indirection. Flow of control.

Something like:

```dart
router.add(method: 'GET', url: '/', handler: someFunction)
```

Can pretty easily be translated to:

```dart
if (request.method == 'GET' && request.url == '/') { someFunction(); }
```

Right?

In Embla, routes can be thought of as forks in the road, with a signpost which says:
"If your request satisfies these demands (like HTTP method or URL), go this way", otherwise,
the route (which is a middleware) passes the request along to the next middleware.
