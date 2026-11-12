

> Martin Kleppman 在 _设计数据密集型应用_（O’Reilly）第 4 章中对 JSON、XML、Protobuf、Thrift 和 Avro 撰写了出色的拆解与分析。如果你想了解这些模式技术的底层工作原理，建议阅读此书。

我们不会涉及其他序列化技术（包括 Thrift），因为它们不常用于事件驱动的数据网格。这并不表示它们从未被使用，或不能被使用，而只是它们并非首选。最常见的三种选择包括 Protobuf、Avro 和 JSON Schema。让我们逐一查看。

## Google 的 Protocol Buffers，即 Protobuf

Protobuf 于 2008 年开源，长期以来因其 gRPC 格式而广受欢迎。近来，它已被证明是与 Avro 竞争顶级模式格式的有力竞争者。以下是关于 Protobuf 数据类型和模式管理的几个要点：

- 支持标量（例如，string、integer、boolean 等）和复杂数据类型。
- 模式不作为事件的一部分存储，而是在单独的文件中维护。Protobuf 文件通常通过 _模式注册表_ 共享，本章后面将在第 143 页的“模式注册表的作用”中介绍。
- 你可以通过引用存储在其他文件中的模式声明来组合复杂模式。
- 不支持动态类型。不过，考虑到数据产品需要强类型和定义明确的模式，这并非重大缺陷。

以下是使用 Protobuf Proto3 版本定义的 Person 对象示例：

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

Protobuf 有两个主要版本，在默认值和缺失数据处理方面功能不同。Protobuf v2 允许将字段标记为 required 或 optional，支持设置自定义默认值，并能够确定字段是包含在消息中还是缺失。这些功能都使模式演进更易于管理，因为它们为生产者提供了更严格的边界，为消费者提供了更强的保证。

Protobuf v3 引入了对 JSON 编码的支持，以及许多其他功能，但代价是移除了 required/optional 和自定义默认值。与任何开源社区一样，这一决定在两个阵营中都有支持者和反对者。许多组织选择继续使用 Protobuf v2 标准，而其他组织则已转向 Protobuf v3。

## Apache Avro

Apache Avro 是另一种极其常见的模式和序列化格式，由 Apache 软件基金会创建。Avro 最初于 2009 年发布，多年来常与 Apache Kafka 关联，同时也是一种基于行的大数据存储格式。以下是关于 Avro 数据类型和模式管理的几个要点：

- 支持原始类型（例如，string、integer、boolean 等）和复杂数据类型。
- 默认情况下，模式作为数据的一部分存储。模式也可以与数据解耦并独立存储。
- 通过随事件包含模式，Avro 提供动态反序列化。消费者可以将事件反序列化为 GenericRecord 对象，该对象根据模式和数据动态构建和填充。当消费客户端没有代码生成器时，通常使用此功能。
- 你可以通过引用存储在其他文件中的模式声明来组合复杂模式。
- 虽然模式演进是可能的，但它未被明确定义为标准的一部分。

以下是前一个代码片段中同一 Person 对象的 Avro 模式示例：

```
{
  "type": "record",
  "name": "Person",
  "namespace": "com.event.driven.datamesh",
  "doc": "Example of a Person record",
  "fields": [
    {
      "name": "id",
      "type": "integer",
      "doc": "The person's unique ID"
    },
    {
      "name": "name",
      "type": "string",
      "doc": "The person's full legal name"
    },
    {
      "name": "height",
      "type": "integer",
      "doc": "Measured in centimeters, rounded to the nearest centimeter"
    },
    {
      "name": "countryCode",
      "type": "enum",
      "symbols": ["AAA", "ABW", ... "ZWE"],
      "doc": "ISO3166-1-alpha-3 standard. AAA=OTHER"
    }
  ]
}
```

你可能注意到，Avro 模式比 Protobuf 略显冗长。在 Avro 中，type 和 doc 等所有属性都完全包含在字段定义中。相比之下，Protobuf 中的注释只是使用 C/C++ 风格语法添加。从语义上讲，两者非常相似。

本章将要介绍的下一个也是最后一种模式技术是 JSON Schema。

## JSON Schema

JSON Schema 格式允许你对 JSON 文档进行注释和验证。与 Avro 和 Protobuf 不同，你可以使用 *无模式* 的 JSON 文档。无模式 JSON 意味着模式中并未真正定义什么 *应该* 和 *不应该* 包含在内，也没有任何类型或默认值。无模式 JSON 不适合用于数据产品，因为它为错误、误解和缺失数据留下了太多空间。

以下是关于 JSON Schema 数据类型和模式管理的几个要点：

- 支持六种原始类型（null、boolean、object、array、number、string）以及一些更复杂的类型。
- 你可以通过引用存储在其他文件或位置中的模式来组合复杂模式。
- 数据验证与 Protobuf 和 Avro 类似。生产者在将数据写入事件流之前，会先针对其模式进行验证。
- 支持向数字、字符串、数组和对象添加验证关键字，以实施数据质量。

以下是前一个代码片段中同一 Person 对象的 JSON Schema 表示：

```
{
  "$id": "https://example.com/person.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Person",
  "type": "object",
  "properties": {
    "id": {
      "type": "number",
      "description": "The person's unique ID"
    },
    "name": {
      "type": "string",
      "description": "The person's full legal name"
    },
    "height": {
      "type": "number",
      "description": "Measured in centimeters, rounded to the nearest centimeter",
      "minimum": 1,
      "maximum": 300
    },
    "countryCode": {
      "type": "string",
      "enum": ["AAA", "ABW", ... "ZWE"],
      "description": "ISO3166-1-alpha-3 standard. AAA=OTHER"
    }
  }
}
```

JSON Schema 是迄今为止讨论的唯一一种既包含用于指定模式的语言，又包含用于限制某些属性范围的验证参数的技术。如 height 属性所示，人的身高被限制在 1 到 300 厘米之间，描述中还包含关于四舍五入到最近整数的更多信息。这些约束将防止系统意外将身高以毫米输入，但不幸的是，它们仍无法防止系统以英寸写入身高。

## 模式演进：随时间变更你的模式

即使你在第一次尝试中创建了完美的数据模型，它也不可避免地需要随时间而改变。新的业务职责可能导致领域扩展，从而为数据产品创造新的需求。虽然新系统可能需要访问最新的数据格式，但无需新数据的现有系统需要保证其数据契约不会发生变化。模式演进允许我们变更模式，以便在不破坏现有消费者兼容性的情况下满足新消费者的用例。

Avro、Protobuf 和 JSON Schema 之间共有的模式演进属性有几种。第一种是兼容模式，事件可以根据模式版本之间的更改进行前向或后向转换。兼容模式对于变更管理、防止（非故意的）破坏性更改，以及免除消费者为每个模式版本编写自定义代码（例如，如果 schema.version==1 则执行此操作，如果 schema.version==2 则执行那项操作，等等）至关重要。

主要兼容模式如下：

- 向后兼容

使用新模式的消费者可以读取使用旧模式生成的数据。这种兼容模式对于确保使用最新模式的消费者仍能读取和处理早期版本编码的旧数据非常重要。

例如，删除字段是向后兼容的。假设我们有如下所示的记录：

```
{
"type": "record",
"name": "Example",
"doc": "This is Version 1",
"fields": [
{ "name": "id", "type": "integer" },
{ "name": "foobar", "type": "string" }
]
}
```

我们可以为版本 2 移除 foobar 字段，如下所示：

```
{
"type": "record",
"name": "Example",
"doc": "This is Version 2",
"fields": [
```

