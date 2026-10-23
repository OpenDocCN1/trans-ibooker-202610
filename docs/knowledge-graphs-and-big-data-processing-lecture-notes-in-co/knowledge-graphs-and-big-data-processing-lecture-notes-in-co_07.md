## 第4章 知识图谱的创建

Anastasia Dimou  
电子与信息系统系  
根特大学，比利时根特  
anastasia.dimou@ugent.be

**摘要.** 本章介绍知识图谱是如何生成的。目标是概述已有提出的不同方法，并了解更多关于当前主流方法的细节。阅读本章后，读者应当了解可用于生成知识图谱的不同解决方案，并能够选择最适合特定用例的映射语言。

### 1 引言

语义网的真正力量将在大量需要来自不同异构数据源信息的软件代理出现后得以实现。然而，人类代理和机器代理与异构数据交互的能力仍然有限，因为大多数数据并非以知识图谱的形式提供，而知识图谱是语义网的基石。这些数据具有不同的结构（例如表格型、层次型），以异构格式（例如 CSV、XML、JSON）出现，并通过异构接口（例如数据库接口或 Web API）访问。

因此，人们提出了不同的方法从现有数据生成知识图谱。最初，提出了自定义实现 [67, 292]，并且至今仍然盛行 [71, 77]；不过，也出现了更通用的方法。这类方法最初侧重于特定格式的数据，例如专门针对关系数据库 [93]、Excel 数据（例如 [274]）或 XML 格式数据（例如 [272]）的方法。然而，持有不同格式数据的数据所有者需要学习和维护多种工具 [111]。

为了解决这个问题，人们提出了不同的方法，在生成知识图谱的同时集成异构数据源。这些方法遵循不同的方向，但将规则定义与规则执行分离的做法占了上风，因为这使得规则在不同实现之间具有互操作性，而处理这些规则的系统则是独立于用例的。为了生成知识图谱，一方面提出了专用的映射语言，例如 RML [111]；另一方面，将用于其他任务的现有语言重新用作映射语言，例如 SPARQL-Generate [278]。

## Function Calling 介绍

本文主要介绍如何使用 Function Calling 功能，并结合代码示例讲解使用细节。

### 什么是 Function Calling

Function Calling 允许模型在对话中识别需要调用外部函数的场景，并输出结构化的调用参数，由应用侧执行真实的函数调用。

- 支持多轮对话中的函数调用
- 支持自动填充函数参数
- 支持并行函数调用

### 快速开始

```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="moonshot-v1-8k",
    messages=[{"role": "user", "content": "上海天气怎么样？"}],
    tools=[{
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get current weather",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {"type": "string"}
                }
            }
        }
    }]
)
```

通过 `tools` 参数声明可用函数，模型根据用户输入自动生成调用请求。

```text
1  # Referencing Object Map
2 <#Country_ROM>  rr:parentTriplesMap <#Country_TM> ;
3                 rr:join [
4                   rr:child "nationality" ;
5                   rr:parent "country_name" ] .

7 <#Country_TM>  rr:logicalTable [ rr:tableName "country" ];
8                 rr:subjectMap  rr:template "http://ex.com/country/{country_name}" .
```

清单4.4. 引用对象映射（Referencing Object Map）根据另一个三元组映射的主题映射生成一个对象。

术语映射（Term Map，rr:TermMap）定义了如何生成 RDF 项（IRI、空节点或字面量），它可以是常量值、列值或模板值。

常量值术语映射（rr:constant，清单4.3第5行）总是生成相同的 RDF 项，默认情况下该项是 IRI。

列值术语映射（rr:column，清单4.3第6行）默认生成一个字面量，该字面量是给定逻辑表行中的某一列。可以选择性地定义语言（rr:language，第7行）和数据类型（rr:datatype）。

模板值术语映射（rr:template，清单4.3第8行）是一个包含被引用列的有效字符串模板，默认生成 IRI。如果想改变默认的项类型，需要显式定义项类型（rr:termType）（rr:IRI、rr:Literal、rr:BlankNode）。

### 3 RML

RDF 映射语言（RML）[110,111] 表达了从异构数据结构、格式和序列化到 RDF 的自定义映射规则。RML 是 R2RML 的超集，旨在扩展其适用性和范围，增加对异构数据的支持。RML 保留了 R2RML 中的映射规则，但从核心模型中排除了其特定于数据库的引用。这样，在 R2RML 中仅限于某个数据库的输入数据（因为每个 R2RML 处理器可能只关联一个数据库），在 RML 中变成了一个或多个输入数据源的广泛集合。

RML 提供了一种通用方式，定义涉及不同数据结构的映射规则，并结合特定于用例的扩展，同时保持与 R2RML 的向后兼容性，因为关系数据库正是这样一个特定案例。RML 使得映射规则能够以组合和统一的方式定义知识图谱如何从一组共同描述某个领域的源生成。映射规则可以在描述同一领域的不同源之间重复使用，以逐步形成高度集成的数据集。

RML 词汇表命名空间为 http://semweb.mmlab.be/ns/rml#，推荐前缀为 rml。

在本小节的其余部分，我们将更详细地讨论 RML 中的数据检索和转换，以及 RML 的其他表示形式。

#### 3.1 数据检索

数据最初可以 (i) 位于不同的位置，例如本地网络上的文件或数据库，或发布在 Web 上；(ii) 通过不同的方式访问


逻辑引用。根据 R2RML，列值或模板值项映射被定义为引用列名。在 RML 的情况下，引入了更通用的概念，即逻辑引用。其值必须是对输入数据集数据的有效引用，根据指定的引用表述方式。因此，引用值应根据逻辑源中定义的引用表述方式为有效表达式。

```
1  # Predicate Object Map with Object Map
2  <#CountryName_POM>  rr:predicate ex:name ;
3                    rr:objectMap [
4                        rml:reference "country_name" ;
5                        rml:languageMap [ rml:reference "@country_language" ] ] .
```

列表 4.10. RML 中的对象映射，其中包含根据引用表述方式对数据的引用以及语言映射来定义语言。

RDF 项映射使用引用表述方式相关的、对应数据格式的数据片段进行实例化。这些片段源自从逻辑源在某一迭代中提取的数据。这样的逻辑源由从根据对应数据集或服务描述词汇访问的存储库中检索的数据形成。

语言映射。RML 引入了一种新的项映射来定义语言，即语言映射（rml:LanguageMap，列表 4.10，第 5 行），它扩展了 R2RML 的语言标签（rr:language）。语言映射允许不仅使用语言的常量值，还允许使用从输入数据派生的引用。rr:language 然后被视为 rml:languageMap 的缩写，就像 rr:predicate 是 rr:predicateMap 的缩写一样。

## 3.2 数据转换：FnO

映射规则涉及对原始数据的（重新）建模，描述对象如何通过指定不同模式中数据之间的对应关系而相关联 [126]，并决定使用哪些词汇和本体。数据转换与映射规则所代表的模式转换相反，是为了支持数据结构、表示或内容的任何更改 [367] 所需的，例如执行字符串转换或计算。

函数本体（FnO）[102,104] 以统一、无歧义且独立于实现其技术的技术的方式描述函数。由于 RML 相对于 R2RML 扩展了模式转换，因此 RML 与 FnO 的结合相对于 R2RML 扩展了数据转换。

函数（fno:Function）是一种具有输入参数、输出并实现某些算法（列表 4.11，第 1 行）的活动。参数（fno:Parameter）是函数的输入值（列表 4.11，第 4 行）。输出（fno:Output）是函数的输出值（列表 4.11，5）。执行（fno:Execution）为特定执行为函数参数分配值。实现（fno:Implementation）定义一个或多个函数的内部工作方式。

```
```yaml
mappings:
  country:
    sources:
      - ['countries.xml~xpath', '/countries/country']
    s: http://ex.com/$(country_abb)
    po:
      - [ex:name, $(country_name)]
      - [ex:abbreviation, $(country_abb)]
```
```

# 列表 4.13. 一组 YARRRML 映射规则

## 4 [R2]RML 扩展和替代方案

其他语言是基于 (i) 数据检索和 (ii) 数据转换的差异而提出的。下表（表 2）总结了映射语言扩展、其前缀和 URI。xR2RML [306] 和 KR2RML [407] 是展示数据检索扩展和替代方案的两个最突出的解决方案。一方面，xR2RML 遵循 RML 范式扩展 R2RML，以支持来自非关系数据库的异构数据。另一方面，KR2RML 通过依赖嵌套关系模型（NRM）[455] 作为中间形式来扩展 R2RML，以表示最初存储在关系数据库中的数据。KR2RML 也为数据转换提供了替代方案，但 FunUL 是 FnO 最突出的替代方案。

**表 2.** [R2]RML 扩展、其 URI 和前缀

| 语言 | 前缀 | URI |
|---|---|---|
| R2RML | rr | http://www.w3.org/ns/r2rml# |
| RML | rml | http://semweb.mmlab.be/ns/rml# |
| xR2RML | xrr | http://www.i3s.unice.fr/ns/xr2rml# |
| FnO+RML | fnml | http://semweb.mmlab.be/ns/fnml# |
| FnO | fno | https://w3id.org/function/ontology# |

### 4.1  xR2RML

xR2RML [306] 于 2014 年在 R2RML 和 RML 的交集处提出。xR2RML 将 R2RML 扩展到关系数据库之外，并扩展 RML 以包含非关系数据库。xR2RML 遵循 RML 范式扩展 R2RML，但专门针对非关系数据库，特别是 NoSQL 和 XML 数据库。NoSQL 系统具有异构数据模型（例如，键值、文档、可扩展列或图存储），与关系数据库相反。xR2RML 假设，如 R2RML 所做，执行规则的处理程序连接到某个数据库。如何针对数据库建立连接或身份验证超出了语言范围，如 R2RML 所述。

xR2RML 词汇的首选前缀为 xrr，命名空间如下：http://www.i3s.unice.fr/ns/xr2rml#。

*数据来源。* 类似于 RML，xR2RML 三元组映射引用逻辑源（`xrr:logicalSource`，列表 4.14，第 3 行），但类似于 R2RML，此逻辑源可以是 xR2RML 基表（`xrr:sourceName`，数据库存在表的数据库）或表示对输入数据库执行查询结果的 xR2RML 视图（`xrr:query`，第 4 行）。

```
```text
1   @prefix xrr: <http://www.i3s.unice.fr/ns/xr2rml#> .
2
3   <#CountriesXML> xrr:logicalSource [
4       xrr:query """for $i in //countries/country return $i; """;
5       rml:iterator "/countries/country";];
6   <#CountryName_POM> rr:predicate ex:name ;
7       rr:objectMap [ xrr:reference "country_name" ] .
```
```

**列表 4.14.** xR2RML 在支持 XQuery 的 XML 数据库上的逻辑源

*迭代器。* xR2RML 最初根据 `rml:iterator` 引入了 `xrr:iterator`，以迭代结果。在后续版本中，xR2RML 收敛使用 `rml:iterator`（列表 4.14，第 5 行）。

*格式或引用表述方式。* 与 RML 考虑表述方式（`rml:referenceFormulation`）来引用其输入数据相反，xR2RML 最初使用属性 `xrr:format`（列表 4.15，第 2 行）显式指定从数据库检索的数据格式。例如，RML 考虑 XPath 或 XQuery 或任何其他表述方式来引用 XML 格式中的数据，xR2RML 将引用格式，例如 `xrr:XML`。虽然 RML 允许引入其他类型的查询语言，但 xR2RML 决定使用确切的查询语言。为了努力与 RML 收敛，xR2RML 可选地考虑引用表述方式。

```
```text
1   <#FemalePoleVault> xrr:logicalSource <#PoleVaultersCSVtable> ;
2   <#PoleVaultersCSVtable> xrr:format xrr:Row .
```
```

**列表 4.15.** Web 上的 CSV 文件作为 xR2RML 逻辑源

*引用。* 类似于 RML，xR2RML 使用引用（`xrr:reference`）来引用数据元素（列表 4.14，第 7 行）。xR2RML 扩展 RML 的引用以引用具有混合格式的数据中的数据元素。xR2RML 考虑不同格式嵌套的情况；例如，JSON 提取嵌入在表格结构的单元格中。混合语法路径由多个路径表达式的串联组成，以斜杠 ‘/’ 字符分隔。

*集合和容器。* 在一次迭代中，*项映射* 可以通过返回多个值生成多个 RDF 项。这通常可以生成多个三元组，但也可以生成以 RDF 集合或容器的形式存在的分层值。为了实现后者，xR2RML 通过引入相应的数据类型来扩展 R2RML，以支持容器的生成。xR2RML 引入了新的 *项类型*（`rr:termType`）：`xrr:RdfList` 用于 `rdf:List`，`xrr:RdfBag` 用于 `rdf:Bag`，`xrr:RdfSeq` 用于 `rdf:Seq` 和 `xrr:RdfAlt` 用于 `rdf:Alt`。所有由 *对象映射* 在一次三元组



map iteration step are then grouped as members of one term. To achieve this, two more constructs are introduced: Nested Term Maps and Push Downs.

```
<#Countries> rr:predicateObjectMap [
  rr:predicate ex:name;
  rr:objectMap [
    xrr:reference "country_name";
    rr:termType xrr:RdfList;
    xrr:pushDown [ xrr:reference "@continent"; xrr:as "continent" ];
    xrr:nestedTermMap [
      rr:template "{continent}: {country_name}" ;
      rr:termType rr:Literal ;
      rr:dataType xsd:string ] ].
```

Listing 4.16. An xrr:RdfList in xR2RML

Nested Term Map. A Nested Term Map (xrr:NestedTermMap, Listing 4.16, line 7) accepts the same properties as a Term Map and can be used to specify a term type, a language tag or a data type for the members of the generated RDF collection or container.

Push Down. Within an iteration, it may be needed to access data elements higher in the hierarchical documents in the context of hierarchical data formats, such as XML or JSON. To deal with this, xR2RML introduces the xrr:pushDown property (Listing 4.16, line 6).

## 4.2 KR2RML

KR2RML [407] extends R2RML in a different way than xR2RML. KR2RML relies on the Nested Relational Model (NRM) as an intermediate form to represent data. The data is mapped into tables by translating it into tables and rows where a column in a table can be either a scalar value or a nested table. Besides the data retrieval part, KR2RML extends R2RML with data transformations using User Defined Functions (UDFs) written in Python.

Data Source. Mapping tabular data (e.g., CSV) into the Nested Relational Model is straightforward. The model has a one-to-one mapping of tables, rows, and columns, unless a transformation like splitting on a column occurs, which will create a new column that contains a nested table.

Mapping hierarchical data (e.g., JSON, XML) into the Nested Relational Model requires a translation algorithm for each data format next to the mapping language. Such an algorithm is considered for data in XML and JSON format. If the data is in JSON, an object maps to a single row table in NRM with a column for each field. Each column is populated with the value of the appropriate field. Fields with scalar values do not need translation, but fields with array values are translated to their own nested tables: if the array contains scalar or object values, each array element becomes a row in the nested table. If the elements are scalar values like strings as in the tags field, a default column name “values” is provided. If a JSON document contains a JSON array at the top level, each element is treated like a row in a database table. If the data is in XML format,
its elements are treated like JSON objects, and its attributes and repeated child elements as single-row nested table where each attribute is a column.

**References.** The column-valued term map is not limited to SQL identifiers as it occurs in R2RML to support mapping nested columns in the NRM. A JSON array is used to capture the column names that make up the path to a nested column from the document root. The *template*-valued term map is also extended to include columns that do not exist in the original input but are the result of the transformations applied by the processor.

**Joins.** Joins are not supported because they are considered to be impractical and require extensive planning and external support.

**Execution Planning.** A tag `(km-dev:hasWorksheetHistory)` is introduced to capture the cleaning, transformation and modeling steps.

**Data Transformations.** The Nested Transformation Model can also be used to embed transformation functions. A transformation function can create a new set of nested tables instead of transforming the data values.

### 4.3 FunUL

FunUL [232] is an alternative to FnO for data transformations. FunUL allows the definition of functions as part of the mapping language. In FunUL, functions have a name and a body. The name needs to be unique. The body defines the function using a standardized programming language. It has a return statement and a call refers to a function with an optional set of parameters.

The FunUL vocabulary namespace is http://kdeg.scss.tcd.ie/ns/rrf# and the preferred prefix is `rrf`.

The class `rrf:Function` defines a function (Listing 4.17, line 3). A function definition has two properties defining the name (`rrf:functionName`, line 4), and the function body (`rrf:functionBody`, line 5).

A function can be called using the property `rrf:functionCall` (Listing 4.17, line 13). This property refers to a `rrf:Function` with the property `rrf:function` (line 14). Parameters are defined using `rrf:parameterBindings` (line 15).

```
```turtle
@prefix rrf: <http://kdeg.scss.tcd.ie/ns/rrf#> .

<#SplitTransformation> a rrf:Function ;
    rrf:functionName "SplitTransformation" ;
    rrf:functionBody
        """function split(value, separator) {
                str = value.split(separator).trim();
                return str; """ ; .

<#FemalePoleVault> rr:predicateObjectMap [
    rr:predicate ex:record;
    rr:objectMap [
        rrf:functionCall [
            rrf:function <#SplitTransformation> ;
            rrf:parameterBindings
```
```

# 5 Conclusions

A lack of in-depth understanding of the complexity of generating knowledge graphs and the many degrees of freedom in modeling and representing knowledge prevents human and software agents from profiting of the Semantic Web potential. This chapter identified the different approaches that were proposed in recent years for generating knowledge graphs from heterogeneous data sources. Then, the chapter focused on approaches that distinguish mapping rules definition from their execution. Two types of mapping languages prevailed, dedicated mapping languages and repurposed mapping languages. The chapter further focused on dedicated mapping languages because they follow the W3C-recommended R2RML.

This chapter presents the author’s view on knowledge graph generation. It serves as an introductory chapter to knowledge graphs, which are considered in greater detail in the following chapters. The next two chapters will explain how to perform federated querying and reasoning over knowledge graphs (Table 3).

# Table 3. Mapping Languages comparison with respect to data retrieval

|  | R2RML | RML | xR2RML | KR2RML |
|---|---|---|---|---|
| Extends | – | R2RML | R2RML & RML | R2RML |
| Data source | rr:LogicalTable | rml:LogicalSource | xrr:LogicalSource | rr:LogicalTable |
| Data references | – | Reference formulation | xrr:format | – |
| Reference | rr:column<br>rr:template | rml:reference<br>rr:template | xrr:reference<br>rr:template | rr:column rr:template |
| Reference formulation | SQL | SQL/XPath/JSONPath acc. Reference formulation | SQL/XPath/JSONPath acc. xrr:format | SQL/XPath/ JSONPath |
| Join | rr:join | rr:join (extended) | rr:join (extended) | Not supported |
| Declarative iterator | No | Yes | Yes | No |
| Iterator | – | rml:iterator | xrr:iterator | – |
| Query | rr:sqlQuery | rml:query | xrr:query | rr:sqlQuery |
| Lists | – | – | xrr:RdfList | – |

# Listing 4.17. A Function Call aligns FunUL with RML

```
[ rml:reference "notes" ]
[ rml:reference "," ] ); ];
```

**Open Access** This chapter is licensed under the terms of the Creative Commons Attribution 4.0 International License (http://creativecommons.org/licenses/by/4.0/), which permits use, sharing, adaptation, distribution and reproduction in any medium or format, as long as you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons license and indicate if changes were made.

The images or other third party material in this chapter are included in the chapter’s Creative Commons license, unless indicated otherwise in a credit line to the material. If material is not included in the chapter’s Creative Commons license and your intended use is not permitted by statutory regulation or exceeds the permitted use, you will need to obtain permission directly from the copyright holder.

![](img/c21ce6788ba392bc08a8057d8b20603e_78_0.png)




