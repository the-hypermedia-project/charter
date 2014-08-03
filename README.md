# The Hypermedia Project
Making the generation and consumption of Hypermedia messages for multiple media types in multiple
programming languages accessible to the masses.

## Genesis
During a number of sessions during the [2014 API-Craft Conference][], it became apparent that a group of
like-minded developers were grappling with similar design patterns and architectures in the Hypermedia
tooling they were creating. A conversation ensued discussing these similarities centered around a brain-dead
simple, state-machine interface for introspecting, interacting with and generating diverse Hypermedia messages:

![whiteboard sketch](assets/whiteboard.png?raw=true) ![representors diagram](assets/representors.png?raw=true)

Given that each were working in different programming languages, it was proposed to join together to
refine a common architecture and interface and then implement developer-friendly tooling in their respective
languages.

The Hypermedia Project was formed to leverage each others expertise to develop tooling for our own
cross-platform Hypermedia applications and to help lower the barrier to entry for interested developers
to explore and create their own Hypermedia APIs in simple, yet powerful ways.

## Architecture and Design
Architecture is about constraints that produce a desired result. Project Members have agreed to develop a common
set of constraints and designs to guide implementation of tooling in their respective languages in order to:

1. Maintain commonality of ideas for developers learning and using these tools.
2. Facilitate open-source participation in developing the various language-specific libraries.
3. Avoid RPC, OOP and/or Remote Type Marshalling anti-patterns.
4. Abstract the complexity of media types and protocols when interacting with Hypermedia APIs to focus on the tasks the
APIs support.

As such, The Hypermedia Project is about a non-exclusive, opinionated approach to Hypermedia tooling. That is, we are
working on Hypermedia tools in an agreed upon fashion without making any universal value statement as to how others MAY or
SHOULD ([RFC2119][]) develop tooling of their own.

## Components
The following sections provide conceptual overviews of various components and links to their evolving design documents.

### Representors
[Representor][] instances will provide a simple state-machine interface for interacting with hypermedia
messages in a consistent fashion.

Because these objects are designed to be a canonical representation of remote Hypermedia resources, they are effectively
a "rosetta stone" of Hypermedia. That is, different media types may be mapped to and from the structure for use in
consuming and generating Hypermedia messages in multiple formats.

The Representor interface includes:

1. Miscellaneous metadata about the message.
2. The set of Transition objects available at runtime in the message.
3. Data attributes of the message.
4. Nested representors in the message associated with embedded resource representations.

#### Transitions
[Transition][] instances encapsulate information about interacting with links and forms (or their equivalents in a
particular generic hypermedia type) in a Hypermedia message. For media types that support embedding protocol specific
information in the message, Transition instances will abstract protocol specifics and allow interacting with the
state-machine transition in a protocol and media type independent fashion.

The Transition interface includes:

1. Miscellaneous metadata about the transition including protocol details.
2. The URI of the transition.
3. Parameters associated with a URI template, if any, including any validators on the values.
4. Attributes associated with request bodies, if any, including any validators on the values.

#### Representor Builder
Internally, Representors will have a robust structure to accommodate mapping diverse Hypermedia media types. In order
to not pollute the interface of Representor instances with this structure, a builder pattern will be scoped that allows
construction of diverse messages via a [RepresentorBuilder][] class.

The interface includes:

1. Methods to add metadata.
2. Methods to build and add transitions.
3. Methods to add attributes.
4. Methods to nest Representors.

#### Serialization
Using simple factory patterns and writing serializer/deserializer pairs for different media types will allow both
translating server-side data into hypermedia representations and server responses into client-side Representor
instances.

Server-side, a representation can be constructed using the RepresentorBuilder from combinations of data from models,
etc. By then using a serialization factory, the constructed representation can injected into a serializer for a
particular media type. Using the specification of the associated media type and iterating over the Representor
state-machine interface, a particular media type response can be rendered.

Client-side, a hypermedia agent can use a deserializer factory to convert a server response to a Representor instance.
In this case, a deserializer would use the media type specification and the RepresentorBuilder to construct and return
a Representor instance.

### Hypermedia Agents
With the aforementioned functionality, the elusive general-purpose Hypermedia client becomes a trivial Representor
wrapper that is able to invoke transitions for implemented protocols.

### Hypermedia Testing Support
By using the Representor interface and various serialization factories, the complexities of testing different
Hypermedia APIs can be abstracted into general purpose step definitions that re-enforce properly consuming Hypermedia
APIs.

### Hypermedia Server-side Generators
In the simplest case, services can code the translation of data into a Representor using the builder and then easily
render it as a response for a particular media type. In more advanced cases, server-side frameworks can also be
implemented in like fashion that allow generic mapping of data into hypermedia responses.

## Roadmap
The following outlines the initial priorities for the project:

1. Isolate the common elements of generic Hypermedia media types to from the internal structure of
Representors.
2. Implement state-machine interface and builder pattern for Representors.
3. Implement serializer/deserializer factories for diverse media types.
4. Implement simple Hypermedia agents.
5. Implement testing support.

[2014 API-Craft Conference]: http://api.api-craft.org/conferences/detroit2014
[RFC2119]: https://www.ietf.org/rfc/rfc2119
[Representor]: design/representor.md
[Transition]: design/representor.md#transition
[RepresentorBuilder]:  design/representor.md#builder
