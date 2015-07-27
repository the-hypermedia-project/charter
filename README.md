# The Hypermedia Project
Making the generation and consumption of Hypermedia messages for multiple media types in multiple programming languages
accessible to the masses.

Checkout the [genesis][] of the project if you are interested.

## Hypermedia Libraries
Libraries in the following languages are currently planned:

- representor-dotnet-csharp - Planned. Contributors Welcome.
- representor-java - Planned. Contributors welcome.
- representor-scala - Planned. Contributors welcome.
- representor-js - Planned. Contributors welcome.
- [representor-python](http://github.com/the-hypermedia-project/representor-python) - WIP
- [representor-ruby](http://github.com/the-hypermedia-project/representor-ruby) - WIP
- [representor-swift](http://github.com/the-hypermedia-project/representor-swift) - WIP

## Representor Pattern
Hypermedia is about self-describing, runtime messages that _represent_ a _resource_ in a client-server system.
These messages are not the resource itself. Rather, they are a local expression of some underlying data and the
possible state transitions associated with that data, as defined by the related remote _resource_.

Practically, a _resource_ can be described as a recursive Finite State Machine (FSM). It may contain data, application
and resource state transitions available based on the current state of the data (and possibly the context of a request 
for the resource) and possibly other recursively embedded resources (finite state machines).

Formally, the representation of a resource is tied to a particular media type that has a strictly specified way of
describing a resource in a message. Servers must generate messages in the format of these media types and programmatic
clients must be written to understand these media types. But, irrespective of the media type, the definition of a
resource exists completely independent of its possible representations.

The _Representor Pattern_ is based on this idea that a canonical resource exists, independent of media type and
domain data models. By programmatically building a _Representor_ instance associated with a resource,
generating hypermedia messages server-side or consuming them client-side can be significantly simplified.

![Representor Pattern Diagram](assets/representor_pattern_diagram.png?raw=true)

The heart of _The Hypermedia Project_ is developing a suite of libraries in different languages that provide
tooling for implementing the _Representor Pattern_. For details on how this pattern can be applied to full-stack
development of Hypermedia applications, checkout the [Components][] discussion.

### Server-side Representor
On the server, a _Representor_ abstracts the underlying data model implementation so that the definition of a resource
is completely decoupled from the details of how data is persisted or organized in models. A server need only, based on
the context of a client request, build a _Representor_ instance associated with the resource and it's current state. It
does this through a simple state-machine related builder interface that adds attributes, optional meta data,
transitions, and possibly embedded resources.

Since the _Representor_ knows how to render itself as a Hypermedia message in different media types, the server can
simply request a representation from the _Representor_ based on a media type negotiated by a client and return it.
Because of this, service developers can focus on the domain of their application, the definition of resources and the
state-machine interface of their API vs. the details of generating Hypermedia messages.

### Client-side Representor
On the client, a _Representor_ abstracts away the underlying protocol and media-type the message was transported by. The
simple state machine interface of the _Representor_ allows client applications to appropriately introspect and interact
with a Hypermedia message.

One of the reasons for the generic state-machine interface is that client applications should not interact with a
_Representor_ as if they are interacting with some remote object. Rather, they should write client code that
"assumes nothing" and checks what is in the message received and respond accordingly. This subtle, but significant,
difference is at the heart of the loose coupling and evolvability associated with Hypermedia APIs.

That being said, a client application could interact with a _Representor_ instance of a message directly, or it could
define an optional "Semantic Presentor" that translates the message into a loosely-coupled local interface that can be
used to present information in a UI based on mapping the elements that it "semantically" understands. Alternately, an
optional "Semantic Model" could be developed as a loosely-coupled local model for interacting with the message.

The primary point is that how a client application decides to interact with a particular resource is not associated with
some remote data/object model that is tunneled across the wire, but rather is decided by locally the client for its own
convenience keeping it de-coupled and free to evolve on its own terms as an underlying Hypermedia API evolves.

## Project Manifesto
Architecture is about constraints that produce a desired result. Project Members have agreed to develop a common
set of constraints and designs to guide implementation of tooling in their respective languages in order to:

1. Maintain commonality of ideas for developers learning and using these tools.
2. Facilitate open-source participation in developing the various language-specific libraries.
3. Avoid RPC, OOP and/or Remote Type Marshalling anti-patterns.
4. Abstract the complexity of media types and protocols when interacting with Hypermedia APIs to focus on the
functionality the APIs support.

As such, The Hypermedia Project is about a non-exclusive, opinionated approach to Hypermedia tooling. That is, we are
working on Hypermedia tools in an agreed upon fashion without making any universal value statement as to how others MAY
or SHOULD ([RFC2119][]) develop tooling of their own.

[RFC2119]: https://www.ietf.org/rfc/rfc2119
[genesis]: reference/genesis.md
[Components]: reference/components.md
