like searching for existing documents, submitting data to a server for storage, and so forth. Gibson and Norman represent the psychological and social aspects of computer interaction we’ll be relying upon in our recipes. For that reason, you’ll find many recipes involve using links and forms to enable the modification of application state across multiple services.

# The Value of Messages

As we saw earlier in this chapter, Alan Kay saw object-oriented programming as a concept rooted in *passing messages* (see [“Alan Kay’s Extreme Late Binding”]). Tim Berners-Lee adopted this same point of view when he outlined the message-centric Hypertext Transfer Protocol (HTTP) in 1992 and helped define the message format of Hypertext Markup Language (HTML) the following year.

By creating a protocol and format for passing messages (rather than a for passing *objects* or *functions*), the future of the web was established. A message-based approach is easier to constrain, easier to modify over time, and offers a more reliable platform for future enhancements such as entirely new formats (XML, JSON, etc.) and modified usage of the protocol (documents, web sites, web apps, etc.).

> **SOME NOT-NO-SUCCESSFUL EXAMPLES**
> 
> HTTP’s encapsulated message approach even allowed for “no-so-successful” innovations like Java Applets, Flash, and XHTML. Even though the protocol was designed to support things like these ‘failed’ alternatives to message-centric HTML, these alternative formats had only a limited lifetime and removing them from the ecosystem did not cause any long term damage to the HTTP protocol. This is a testament to the resilience and flexibility of the HTTP approach to application-level communication.

Message-centric solutions online have parallels in the physical world, too. Insect colonies such as termites and ants, famous for not having any hierarchy or leadership, communicate using a pheromone-based message system. Around the same time Nelson was talking about hypermedia and Gibson was talking about affordances, American biologist and naturalist E. O. Wilson (along with William Brossert) was writing about ant colonies