# Components
The following sections provide conceptual overviews of various components that support the _[Representor Pattern][]_ 
and links to their evolving design documents.

## Representors
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

### Transitions
[Transition][] instances encapsulate information about interacting with links and forms (or their equivalents in a
particular generic hypermedia type) in a Hypermedia message. For media types that support embedding protocol specific
information in the message, Transition instances will abstract protocol specifics and allow interacting with the
state-machine transition in a protocol and media type independent fashion.

The Transition interface includes:

1. Miscellaneous metadata about the transition including protocol details.
2. The URI of the transition.
3. Parameters associated with a URI template, if any, including any validators on the values.
4. Attributes associated with request bodies, if any, including any validators on the values.

### Representor Builder
Internally, Representors will have a robust structure to accommodate mapping diverse Hypermedia media types. In order
to not pollute the interface of Representor instances with this structure, a builder pattern will be scoped that allows
construction of diverse messages via a [RepresentorBuilder][] class.

The interface includes:

1. Methods to add metadata.
2. Methods to build and add transitions.
3. Methods to add attributes.
4. Methods to nest Representors.

### Serialization
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

## Hypermedia Agents
With the aforementioned functionality, the elusive general-purpose Hypermedia client becomes a trivial Representor
wrapper that is able to invoke transitions for implemented protocols.

## Hypermedia Testing Support
By using the Representor interface and various serialization factories, the complexities of testing different
Hypermedia APIs can be abstracted into general purpose step definitions that re-enforce properly consuming Hypermedia
APIs.

## Hypermedia Server-side Generators
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
5. Implement testing <support class=""></support>

[Representor Pattern]: ../representor-pattern
[Representor]: design/representor.md
[Transition]: design/representor.md#transition
[RepresentorBuilder]:  design/representor.md#builder
