You can write a custom registration script to evaluate the schema during the registration process. A simple and common check is to verify that every value in a schema has a “doc” string of nonzero length to ensure that there is _some_ documentation for each field. Another option is to scan for PII patterns and request an additional verification step if something suspicious is found. Similarly, you can automatically generate class definitions and run validation tests prior to deployment.

Schema registries provide many benefits for a low overhead and are an essential component of an event-driven data mesh. The savings on network and storage costs alone make them a valuable choice, with the remaining functionality providing the icing on the cake.

So now that we’ve covered schemas, schema registries, and the benefits of using both, we have one last schema-related question before winding up the chapter. How do we manage schemas in relation to our data product producer and consumer applications?

# # Best Practices for Managing Schemas in Your Codebase

There are several schools of thought for storing event schemas in your codebase. One is to centralize all of your data product schemas in one big repository, using references to tie them to the codebase that actually owns the schema. Another is to decentralize and store schemas in the code repository of the producers and rely on the self-service data platform to tie them all together.

However, the best practice is a combination of the two: a centralized code repository for shared-use schemas with data product owners retaining their own schemas internal to their projects. Let’s take a deeper look at how this works in practice by first examining *schema composition* and its role in a data mesh.

Apache Avro, Protobuf, and JSON Schema each provide the ability to import schemas from other files just as you would import a class or a library into your code. This is a powerful feature that enables us to compose a schema from simple common building blocks.

First, you’ll need to consult with your federated governance team to come up with standardizations of common cross-domain entities. In this case, previous meetings, discussions, and arguments have led to the standardization of two simple (but useful!) entities within the organization—base_user, representing the unique ID of a user: