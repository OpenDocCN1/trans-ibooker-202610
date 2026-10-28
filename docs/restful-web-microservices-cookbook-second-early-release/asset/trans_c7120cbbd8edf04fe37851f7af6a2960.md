

```json
"href": "http://localhost:8181",
"title": "BigCo Activity Records",
"links": [
  {
    "name": "home", "href": "http://localhost:8181/",
    "rel": "home", "prompt": "Home"
  },
  {
    "name": "list", "href": "http://localhost:8181/list/",
    "rel": "list", "prompt": "List"
  }
],
"items": [
  {
    "id": "22s3k36pkn4",
    "rel": "person",
    "href": "http://localhost:8181/22s3k36pkn4",
    "data": [
      {"name": "id", "value": "22s3k36pkn4", "prompt": "id"},
      {"name": "givenName", "value": "Mork", "prompt": "givenName"},
      {"name": "familyName", "value": "Mockery", "prompt": "familyName"}
    ],
    "links": [
      {
        "name": "read", "href": "http://localhost:8181/22s3k36pkn4",
        "rel": "read", "prompt": "Read"
      }
    ]
  }
],
"queries": [
  {
    "name": "filter",
    "href": "http://localhost:8181/filter/",
    "rel": "filter",
    "prompt": "Search",
    "data": [
      {"name": "givenName", "value": ""},
      {"name": "familyName", "value": ""}
    ]
  }
],
"template": [
  {"name": "id", "value": "", "prompt": "id"},
  {"name": "givenName", "value": "", "prompt": "givenName"},
  {"name": "familyName", "value": "", "prompt": "familyName"}
]
```

至少，你应该列出所有语义标识符并包含描述：

| Identifier | Description |
|---|---|
| id | Record identifier for a person record |
| givenName | givenName for a person record |
| familyName | familyName for a person record |
| person | indicates a person record |
| home | Hypermedia control to navigate to Home view |
| list | Hypermedia control to navigate to List view |
| read | Hypermedia control to navigate to single person record |
| filter | Hypermedia control to filter the List view |

作为附加信息，你还可以列出每个元素可能出现在消息中的位置：

| Identifier | Description | Element |
|---|---|---|
| id | Record identifier for a person record | name |
| givenName | givenName for a person record | name |
| familyName | familyName for a person record | name |
| person | indicates a person record | rel |
| collection | Identifies a collection of records | rel |
| item | Identifies a single record | rel |
| home | Hypermedia control to navigate to Home view | rel, name |
| list | Hypermedia control to navigate to List view | rel, name |

请注意，对于 FormsJSON 格式，一些语义值作为 JSON 中的结构元素出现（`id`、`givenName`、`familyName`、`person`），而其他则作为值元素出现（`collection`、`item`、`list`、`home`、`read`、`filter`）。这种结构与值的混合使得每种媒体类型独一无二。如果你的服务支持多种媒体类型，你需要提供语义配置文件信息，以便正确地将语义元素映射到消息元素。

```json
{
  "person": {
    "links": [
      {
        "id": "home",
        "name": "home",
        "href": "http://localhost:8181/",
        "rel": "home",
        "title": "Home",
        "method": "GET",
        "properties": []
      },
      {
        "id": "list",
        "name": "list",
        "href": "http://localhost:8181/list/",
        "rel": "list collection",
        "title": "List",
        "method": "GET",
        "properties": []
      },
      {
        "id": "filter",
        "name": "filter",
        "href": "http://localhost:8181/filter/",
        "rel": "filter collection",
        "title": "Search",
        "method": "GET",
        "properties": [
          {"name": "givenName", "value": ""},
          {"name": "familyName", "value": ""}
        ]
      }
    ],
    "items": [
      {
        "links": [
          {
            "id": "read_22s3k36pkn4",
            "name": "read",
            "href": "http://localhost:8181/22s3k36pkn4",
            "rel": "read item",
            "title": "Read",
            "method": "GET",
            "properties": []
          }
        ],
        "id": "22s3k36pkn4",
        "givenName": "Mork",
        "familyName": "Mockery"
      }
    ]
  }
}
```

## Discussion

本质上，语义配置文件承载了服务的域元素（person、list、filter 等）。你可以将语义配置文件视为记录服务域的方法。

值得指出的是，这些语义配置文件并不能解释 *如何* 过滤超媒体控件工作——只是它可能出现在响应中。语义配置文件假定 API 消费者应用程序独立于配置文件理解该媒体类型。理解消息格式与理解消息 *内部* 的语义之间的这种分离，是设计良好的超媒体服务的重要特性。

当你的服务不使用已注册的媒体类型而是使用纯 JSON 或 XML 等序列化格式时，服务通常会将域的语义嵌入到消息的结构中。例如，这里是 person 服务在 XML 中的一个可能响应：

```xml
<lists>
  <home href="..." rel="collection"/>
  <list href="..." rel="collection"/>
  <filter href="..." method="get" rel="collection">
    <givenName></givenName>
    <familyName></familyName>
  </filter>
  <person id="..." rel="item read" href="...">
    <familyName>...</familyName>
    <givenName>...</givenName>
  </person>
</lists>
```

而语义配置文件映射看起来像这样：

| Identifier | Description | Element (Cj) | Element (Fj) | Element (XML) |
|---|---|---|---|---|
| id | Record identifier for a person record | name | KEY | ATTRIBUTE |
| givenName | givenName for a person record | name | KEY | ELEMENT |
| familyName | familyName for a person record | name | KEY | ELEMENT |
| person | indicates a person record | rel | KEY | ELEMENT |
| collection | Identifies a collection of records | rel | rel | rel |
| item | Identifies a single record | rel | rel | rel |
| home | Hypermedia control to navigate to Home view | rel, name | rel, name, id | ELEMENT |
| list | Hypermedia control to navigate to List view | rel, name | rel, name, id | ELEMENT |
| read | Hypermedia control to navigate to single person record | rel, name | rel, name, id | rel |
| filter | Hypermedia control to filter the List view | rel, name | rel, name, id | ELEMENT |

在这种情况下，域语义与消息格式如此交织，以至于内部域元素的更改会导致外部接口的破坏性结构更改（例如重命名元素、添加/删除属性等）。只要可能，就应保持消息细节（外部接口）独立于域细节（内部功能）。当我看到域语义作为响应中的结构元素出现时，这会提醒我重新思考消息设计，以确保我没有将内部细节泄露到外部接口。参见 Recipe 4.4 了解更多关于此主题的内容。

## Related

- Chapter 2
- Chapter 3
- Chapter 4
- Recipe 4.4
- [即将发布的链接]

## 4.8 Supporting Shared Vocabularies in Standard Formats

当服务接口采用使用标准、已注册媒体类型来交换消息的做法时，它们还需要标准化在运行时表达服务域特定属性和操作（或 *语义*）的方式。这与消息格式挑战类似。解决方案是依赖标准化格式来表达含义。真正的难点在于将内部数据/对象模型表达为一致的外部语义模型。

## Problem

我们如何标准化机器之间传输内容含义的方式？有哪些格式可用于此工作，以及表达内部数据和流程模型为标准化语义模型的模式是什么？

## Solution

正如 API 消费者依赖标准化协议（例如 HTTP）和标准化消息格式（例如 SIREN、Collection+JSON 等），它们也可以从标准化词汇表中受益。这意味着以客户端理解的标准格式列出所有可能出现在 API 中的数据属性和操作名称。

这意味着需要有一个单一来源，包含可能出现在 API 中所有事物的可能名称。例如，`id`、`name`、`class`、`rel` 或类似结构元素中可能出现的任何值。这些值的集合可以涵盖一个主题或域（如“用户管理”或“会计”）。

有几种选项可以以标准化形式携带词汇数据。最知名的是使用资源描述框架（RDF）⁵⁴ 格式，如 RDF/XML⁵⁵、Turtle⁵⁶ 和 JSON-LD⁵⁷。JSON-LD 格式是我在 API 想要使用基于 RDF 的格式时更常看到的。

我用来记录词汇的格式是 Application-Level Profile Semantics（ALPS）⁵⁸ 格式。ALPS 有 XML、JSON 甚至 YAML 的序列化。

### Example

下面是用 FOAF（Friend of a Friend）⁵⁹ ⁶⁰ 词汇表达的 `person` 的 RDF 版本。

```turtle
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

<#JW>
        a foaf:Person ;
        foaf:name "James Wales" ;
        foaf:mbox <mailto:jwales@bomis.com> ;
        foaf:homepage <http://www.jameswales.com> ;
        foaf:nick "Jimbo" ;
        foaf:depiction <http://www.jameswales.com/aus_img_small.jpg>
;
        foaf:interest <http://www.wikimedia.org> ;
        foaf:knows [
```
