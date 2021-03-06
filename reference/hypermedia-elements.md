# Elements of Hypermedia Message

The purpose of this document is to outline the common building blocks of hypermedia media types for use in designing a canonical hypermedial message.

The elements found in this document are derived from the most common hypermedia formats in use today.

1. [HAL](http://stateless.co/hal_specification.html)
1. [Collection+JSON](http://amundsen.com/media-types/collection/)
1. [Siren](http://sirenspec.org)
1. [UBER](https://rawgit.com/mamund/media-types/master/uber-hypermedia.html)
1. [Mason](https://github.com/JornWildt/Mason)
1. [HTML](http://www.w3.org/TR/html5/)

To faithfully represent many of the elements of each of these media types, this document will try cover every hypermedia element possible from these formats. A more detailed and specific document for each media type will be done outside of this document.

This document seeks to conform to [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt):

> The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

## Attributes

Attributes are considered to be properties of a resource. In some media types, attributes may be referred to as `properties`. While document types such as XML or JSON provide ways to do more complex types, an attribute is MUST consist of at least a **name** and a **value**.

There are also other optional properties of attributes that MAY be used.

1. **Label** - Some formats provide ways to give a human-readable label to an attribute. For example, where `first_name` is the attribute, "First Name" may be the label.
1. **Type** - Used in several media types for pointing to outside semantics, such as Schema.org directly or an ALPS descriptor. This can be seen in documents that focus on linked data, such as JSON-LD and HTML+RDFa.

Semantics may be defined in out-of-band documents, such as the specific media type documentation, link relations, or profiles. Usage of those types of documents are outside the scope of this document.

## Transitions

In essence, a transition is an available progression from one state to another state. There are many characteristics of transitions, and several different categories that hypermedia formats use.

These different types of transitions can be broken down into [Aspects](http://www.slideshare.net/rnewton/amundsen-costbenefitshypermedia/80) and [H-Factors](http://amundsen.com/hypermedia/hfactor/).  This document will primarily look at three different kinds of transitions:

1. Safe Transitions
1. Unsafe Transitions
1. Templated Transitions

A transition MUST have a relation type, which defines how the transition relates to current state. A transition MAY also have the following.

1. **Response Types** - What media types the transition can respond with
1. **Label** - Human-readable label for the transition
1. **Type** - Adding semantic information about the transition
1. **Embed** - Define whether or not the resource should be transcluded
1. **Embed As** - Define how to transclude a resource
1. **Language** - Define the language of the transition

### Safe Transitions

Safe transitions do not cause a change, and the resource transition is considered immutable. All safe transitions MUST have a URI.

#### Links

A link is the simplest form of a safe transition. In its simplest form, it is simply a link relation and a URI. It MAY contain embedded data, as outlined in [Embedded Resources](#embedded-resources).

#### Queries

A query is a safe link that has parameters that can be added as query string for the URI. These parameters SHOULD be [Inputs](#inputs).

#### Embedded Resources

Many media types provide ways to embed data for linked resources. Some profiles or formats may provide ways to partially embed a resource, such as through link hints, while others allow for embedding the entire resource.

##### Embedded Meta Data, Attributes, and Transitions

Some media types provide ways to embed meta data about the linked resource. This meta data may be links to a profile for the resource, or other relevant links defining how to handle the linked resource.

Many media types also provide ways for embedding attributes and transitions available for the linked resource. An embedded resource MAY be considered to be partially embedded or fully embedded. If fully embedded, it  MUST include all of the data that would be available if the URI had been requested.

Because of this, an embedded link MAY include meta items, resource attributes, and transition items.

##### Anonymous Links and Actions

There are several specs such as [link hints](http://tools.ietf.org/html/draft-nottingham-link-hint-00) which allow for providing information about the methods that can be invoked on a URI. While this is providing a way to embed available actions, it does so by merely providing the protocol-specific methods, and does not provide a name for the relation types. Because of the lack of name, these types of transitions are referred to as anonymous.

### Unsafe Transitions

An unsafe transition is any transition that causes a resource change. These transitions should be considered mutable. Some media types may refer to these types of transitions as `actions`. An unsafe transition MUST have a URI.

An unsafe transition has the following:

1. **Method** - this is the unsafe method or action being taken on a specific resource. Some media types use protocol-specific methods (e.g. GET or POST), or use more generic semantics (e.g. read or create) that map to protocol-specific methods.
1. **Request Types** - Media types in which the server can accept

Unsafe transitions MAY also included embedded meta data, though an unsafe transition MUST NOT include embedded attributes or transitions.

It also provides a way for defining attributes, which some media types call body parameters or fields. These attributes MUST be [Inputs](#inputs).

### Templated Transitions

Instead of using a normal URI for safe or unsafe transitions, templates use a URI template  based on [RFC 6570](http://tools.ietf.org/html/rfc6570). While formats like HAL combine links and link templates, it is helpful to keep these conceptually different because of this statement of from the RFC.

> URI Templates are not URIs: they do not identify an abstract or physical resource, they are not parsed as URIs, and they should not be used in places where a URI would be expected unless the template expressions will be expanded by a template processor prior to use.

A templated transition MUST have a URI template attribute. The parameters for the URI template MUST be [Inputs](#inputs). Parameters that are part of the URI path MUST be required.

Templated transitions MAY be [safe](#unsafe-transitions) or [unsafe](#unsafe-transitions) transitions once expanded depending on the context of the templates. In their unexpanded form, they SHOULD NOT be considered resolvable URIs.

## Meta

Hypermedia types provide ways to include meta data for both the current resource and linked resources.

### Attributes

Meta attributes includes data about the resource or linked resource. Meta attributes may include data like:

1. The `title` of the resource
1. A `description` of the resource

### Links

Meta links are links that provide relevant resources for processing the returned resource. This may include profile links, help links, or even links to styling documents.

### Curies

Curies are ways to shorten URLs based on the [W3 spec](http://www.w3.org/TR/curie/). It MUST include a shortened name for the curie along with a URI prefix. In some contexts, this is referred to as a prefix.

## Inputs

An input can be used in various contexts to provide information on how data can be provided for queries, URI templates, and forms. An input MUST have the following attributes:

1. **Name** - The name of the parameter
1. **Value** - The value of the parameter

There are also various other attributes that MAY be used. These occur in various formats such as Siren or HTML.

1. **Options** - Similar to an HTML `select` tag, this allows for providing options for the input. An option will have a name and a value.
1. **Placeholder** - Define a placeholder for the input to give hints on the format of the data.
1. **Type** - Allows to specify a type for an input, similar to how HTML has a type for the `input` tags.
1. **Default Value** - The default value of the parameter, useful for giving a suggestion for the inputs value.
