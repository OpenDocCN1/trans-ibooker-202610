
## A Brief Introduction to Serialization and Deserialization

Before we get too far into schemas, let's step back and get a better picture of how they relate to making important business data available through data products. For this, we’re going to need to take a look at how an event is created, serialized, and sent across a network for storage in an event stream. We’ll also take a look at the reverse of this process, where an event is consumed from an event stream, deserialized, and processed by the consumer.

Serialization is the action of taking an event object with a well-defined schema and converting it into a sequence of bytes. The representation in the producer’s memory is serialized (converted) into a sequence of bytes such that it can be easily sent across the network and written to the event stream. The schema provides the constraints for ensuring that the data can be converted into a sequence of bytes. For example, a schema that specifies that the field named length must be an Integer will throw an exception when attempting to serialize with length set to a String value of "six feet".

Figure 6-1 shows the producer workflow of converting a producer-language object into a sequence of bytes that is then written to the event stream. Note that the schema is attached to the serialized event, such that the consumer is provided with a copy for deserialization. There are some tricks we can use to remove the need for sending a schema with every event, which we’ll look at later in this chapter.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_141_0.png)

Figure 6-1. Producer serializing the event object into bytes and writing it to the event stream

On the other end of the event stream, a consumer reverses this process and deserializes the byte sequence into an object. Figure 6-2 illustrates the process of consuming, deserializing, and converting the data into a representation of the event that can be processed by the consumer’s business logic code.

Schema technologies are an essential part of an event-driven data mesh, because they underpin the contract of the data product with its consumers.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_142_0.png)

Figure 6-2. A consumer reading from the event stream and deserializing the bytes back into an event representation

## What Is a Schema?

An event schema is synonymous with the definition of a database table: at a minimum, it specifies names, types, restrictions, and default values. Schemas ensure that both the event data producer and all of its consumers have a shared common understanding of the data. The schema, in conjunction with the event-stream API, forms part of the API of the event stream. You can think of this as equivalent to a REST API that serves JSON-encoded data—both have a communications protocol and both return a well-formed payload of information:

> Event stream API + event schema == REST API + json schema

The data product owner is responsible for creating and managing the event schema definition, including conformance with any federated governance standards, such as standard time formats and PII requirements. Prospective consumers and stakeholders provide feedback to schema proposals, often in the form of code review in this era of remote work.

We'll start this section with a simple Protobuf-powered example to illustrate some of the more compelling reasons for using schemas. Let's start with Example 6-1, which showcases a schema for Person.

### Example 6-1. Person schema with Protobuf

```
message Person {
//The person's unique ID
int32 id = 1;
//The person's full legal name
string name = 2;
//Measured in centimeters, rounded to the nearest centimeter
int32 height = 3;

enum CountryCode {
ABW = 0;
AFG = 1;
...
ZWE = 248;
}
//ISO3166-1-alpha-3 standard. AAA=OTHER
CountryCode country = 4;
}
```

The event schema details the id, name, and height of a person, along with their ISO 3166 three-letter Latin-script country code. This schema forms part of the _data contract_ between the producer and the consumer of the data. It is the common understanding of how one party writes the data and the other party reads the data.

Figure 6-3 shows two clients: a Java producer that writes to the event stream and a C++ consumer that reads the events out of the stream and into its own memory space. The producer can use the Protobuf code generator for Java to automatically create classes and client code for the Person schema. The consumer can similarly generate its own structs and client code using the C++ code generator.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_143_0.png)

Figure 6-3. The producer to event stream to consumer workflow of an event serialized with a schema

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_143_1.png)

Both Avro and Protobuf have many code generators that support a wide variety of languages. You can find support and code generators for C++, C#, Dart, Go, Java, Kotlin, and Python, to name a few.

Example 6-2 shows a producer Java client with which we create a Person named “Mackenzie Bellemare” and the associated properties. Next, we serialize the object and write it to a file named ProtoPerson.data (We’re using a file as a placeholder for the event broker client, simply to avoid bogging this example down with the proper event-broker client produce, retry, and error-handling code).

### Example 6-2. Using Java to populate a Person object, generated from the Protobuf schema

```java
Person mack = Person.newBuilder()
    .setId(4291)
    .setName("Mackenzie Bellemare")
    .setHeight(45)
    .setCountryCode("CAD")
    .build();
output = new FileOutputStream("ProtoPerson.data");
mack.writeTo(output);
```

The consumer reads the Person data from the file (i.e., the event), deserializes it using the schema, and finally converts it into an object or structure native to the consumer’s language. Example 6-3 shows a C++ consumer using the ParseFromIstream function to convert the serialized bytes into a well-structured Person class object.

### Example 6-3. Using C++ to parse the serialized Protobuf Person object

```cpp
Person mack;
fstream input(argv[1],
    ios::in | ios::binary);
mack.ParseFromIstream(&input);

// mack is now populated with the id, name, and height received in the event
id = mack.id();
name = mack.name();
height = mack.height();
cc = mack.countryCode();
```

So what are the main benefits of this approach?

### Standardized data contract

The schema forms the basis of understanding between the producer of the data and all of its consumers, both now and in the future. The producer responsible for producing data according to the schema and will be prevented from serializing the object if the data is not in compliance. The consumers in turn can rely on the schema to provide strongly defined types, names, documentation, and additional information about the data. Maintaining a high standard of quality from the moment the data is created is essential for a healthy data mesh.

### Provides a foundation for discussion

Schemas give us the basis for tangible arguments over the content and form of the data. Explicit schemas, in conjunction with pull request reviews, enable productive discussions over what should and should not be in an event, and if the data product will solve the problems it’s meant to solve.

### Code generation

Code generation lets the application operate on the events as objects in its native language, handling the parsing, conversion, and object creation for you. It greatly simplifies business logic by letting you write code against well-defined classes in your programming language instead of against a map of generic object types.

### Schema evolution

Your data contract will change over time. The best schema options provide a safe path for schema evolution, including rules, restrictions, and safeguards that prevent you from inadvertently violating your data contract with your consumers. Schema evolution is covered in more detail later in this chapter.

### Test event generators

Event generators let you create data that matches your schemas, including specific parameter constraints such as foreign-key and primary-key relationships. You can use these events to test your event-driven service code and use it to produce sample events for consumers to try in their services. Kafka Connect Datagen is an example of such a generator, where you can specify constraints and ranges on the data being generated. Example 6-4 shows a snippet of a schema that contains a userid, where the range of valid data output will vary from User_1 to User_9.

### Example 6-4. Kafka Connect Datagen specification

```
{
"name": "userid",
"type": {
    "type": "string",
    "arg.properties": {
        "regex": "User_[1-9]{0,1}"
    }
}
},
```

As we can see, there are some significant benefits to schemas, so the next question is: what schema technology should you use?

## What Are Our Schema Technology Options?

There are many schema technologies available for building event-driven data products. However, there are a few that stand above the rest of the field in terms of features, commonality, community development, ease of use, and supportive tooling. Google's Protobuf and Apache Avro both fit this bill. JSON Schema is somewhat popular for those who favor JSON, but please do not confuse it with the tangled mess that is schemaless JSON (more on this in a bit).
