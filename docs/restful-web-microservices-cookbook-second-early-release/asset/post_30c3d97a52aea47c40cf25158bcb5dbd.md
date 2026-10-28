
In this way, setting out these first recipes is a kind of “*a priori* design” approach. One of the definitions of *a priori* from the Merriam-Webster dictionary is “formed or conceived beforehand” and that is what we are doing here. We are setting out elements of our systems beforehand. And there is an advantage to adopting this *a priori* design approach. It allows us to define stable elements of the system upon which we can build the services and implement their interaction.

Creating a design approach means we need a model that works for more than a single solution. For example, an approach that only works for content management systems (CMSs) but not for customer relationship management systems (CRMs) is not a very useful design approach. We intuitively know that these two very different solutions share quite a bit in common (both at the design and the technical solution level) but it often takes some work to tease out those similarities into a coherent set — a set of design principles.

This can be especially challenging when we want to create solutions that can change over time. Ones that remain stable while new features are added, new technology solutions are implemented, and additional resources like servers and client apps are created to interact with the system over time. What we need is a foundational design element that provides stability while supporting change.

In this set of recipes, that foundational element is the use of hypermedia, or links and forms (see [Recipe 1.2](#)), as the device for enabling communications between services. Fielding called hypermedia “the engine of application state”¹⁷. Hypermedia provides that meta-messaging Licklider identified (see [“Licklider’s Aliens”](#)). And it is the use of hypermedia that enables Kay’s “extreme late binding” (see [“Alan Kay’s Extreme Late Binding”](#)).

You’ll find most of the recipes in this book lean heavily on hypermedia in order to enable both connection and general communication between machines. In this chapter, [Recipe 2.5](#), [Recipe 2.1](#), and [Recipe 2.6](#) are specifically aimed at establishing hypermedia as a key element in this *a priori* design. Another key element in supporting this design approach is the use of the three pillars of information architecture (see “Morville’s Information Architecture”).

So, how do we use design recipes to create hypermedia-powered shared “a priori” understanding based on three-part information architecture that lives up to Licklider’s vision? Let’s look at the recipes below for examples.

## 2.1 Creating Interoperability with Registered Media Types

A key to establishing a stable, reliable system is to ensure long-term interoperability between services. That means services created years in the past are able to successfully exchange messages with services created years in the future.

### Problem

How do you design services that have a high degree of interoperability well into the future?

### Solution

The best way to ensure a high level of long-term interoperability between services is to establish stable rules for exchanging information. On the web, the best way to do that is to select and document support for one or more open source media type formats for data exchange. A good source for long-term, stable media types is the Internet Assigned Numbers Authority, a.k.a. IANA. Viable candidate media types for long-term support of RWMs are unstructured media types like XML and JSON as well as structured media types such as HTML, Collection+JSON, UBER, HAL, and Siren. See Appendix C for a list of viable media types at the time of this book’s release.

> **NOTE**
>
> See [Recipe 2.2](#) for more on the difference between structured and unstructured media types.

### Discussion

When you create services, you should document which registered media types (RMTs) your service can support. It is recommended that your service support more than one RMT and that you allow service consumers to both discover which RMTs your service supports and how they can indicate their preference when exchanging messages (see *design negotiation*).

> **TIP**
>
> When I release a service, I almost always add HTML as one of the designated message formats. It has been around for 30+ years; you can use any common browser as a client for the service (great for testing), and there are enough parsers and other HTML tooling to make it relatively easy for consumers of your service to be able to exchange data with you.

Additional recommendations for candidate media types are:

- they should support hypermedia within the message (see [Recipe 2.5](#)).
- they should support custom extensions (e.g. your ability to safely add new features to the format without breaking any existing applications).

This last point will come in handy when you are stuck supporting a format that has fallen out of favor and you need to make some modifications to extend the life of your service.

It is also possible to create your own media type format for your services. That can work if:

- a) your universe of service API consumers is relatively limited (e.g. within your own company),
- b) your universe of service API consumers is crazy large (e.g. Google, Facebook, Amazon, etc.), or
- c) your services are the leader in a vertical (e.g. document management, financial services, health care, etc.).

If your efforts do not fall into one of these categories, I recommend you DO NOT author your own media type.

### SO YOU WANT TO CREATE YOUR OWN MEDIA TYPE, EH?

If you decide to author your own custom media type, you should treat it as if it will become a popular, public format. That means documenting it and registering it properly. You also need to be ready to support any community that grows up around your media type format — including example apps and other tooling. Finally, you need to be committed to doing this work for a long time. Also, keep in mind that some people might build their own services using your format and they will deserve the proper support well into the future.

Over the years, I’ve authored close to a dozen original media types. Only a few have ever garnered much attention. But I still try to keep up my responsibilities to all of them.

### Related Recipes

- Recipe 2.2
- *design negotiation*

## 2.2 Ensuring Future Compatibility with Structured Media Types (SMTs)

An important foundational element in the design of RWM is supporting future compatibility. This means making it possible for services written in the past to continue to interact with services well into the future. It also means designing interactions so that it is unlikely that future changes to already-deployed services will *break* other existing services or clients.

### Problem

How do you design machine-to-machine interactions that can support modifications to in-production services that do not break existing service consumers?

### Solution

To support non-breaking changes to existing services you should use *structured media types* (SMTs) to pass information back and forth. SMTs make it possible to emit a stable, non-breaking message even when the *contents* of that message (e.g. the properties of a record, list of actions, etc.) has changed. The key is to design interactions where the message shared between machines maintains the same structure even when the data conveyed by that message changes.

A structured media type (SMT) provides a strongly-typed format that does not change based on the data being expressed. This is in opposition to unstructured message formats like XML and JSON. See the example for details.

It is a good idea to use well-known media types in your interactions. Media types registered through the Internet Assigned Numbers Authority (IANA) are good candidates. For example, HTML is a good example of an SMT. Other viable general use SMT formats are Collection+JSON and UBER. See *Appendix C* for a longer list of media types to consider.

### Example

A structured media type (SMT) provides a strongly-typed format that does not change based on the data being expressed. Here’s an example of a simple message expressed as HTML

```html
<ul name="Person">
  <li name="givenName">Marti</li>
  <li name="familyName">Contardi</li>
</ul>
...
<ul name="Person">
  <li name="givenName">Marti</li>
  <li name="familyName">Contardi</li>
  <li name="emailAddress">mcontardi@example.org</li>
</ul>
```

The *structure* of the above message can be expressed (in an overly simplified way) as “One or more li elements enclosed by a single ul element.” Note that adding more content (for example, an email address) does not change the *structure* of the message (you could use the same message validator), just the *contents*.

Contrast the above example with the following JSON objects:

