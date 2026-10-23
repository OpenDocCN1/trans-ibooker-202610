
Logical Reference. According to R2RML, a column-valued or template-valued term map is defined as referring to a column name. In the case of RML, a more generic notion is introduced, the logical reference. Its value must be a valid reference to the data of the input dataset according to the specified reference formulation. Thus, the reference's value should be a valid expression according to the Reference Formulation defined at the Logical Source.

```
1  # Predicate Object Map with Object Map
2  <#CountryName_POM>  rr:predicate ex:name ;
3                    rr:objectMap [
4                        rml:reference "country_name" ;
5                        rml:languageMap [ rml:reference "@country_language" ] ] .
```

Listing 4.10. An Object Map in RML with a reference to data according to the Reference Formulation and a language Map to define the language.

RDF Term Maps are instantiated with data fractions referred to using a reference formulation relevant to the corresponding data format. Those fractions are derived from data extracted at a certain iteration from a Logical Source. Such a Logical Source is formed by data retrieved from a repository accessed as defined by the corresponding dataset or service description vocabulary.

Language Map. RML introduces a new Term Map for defining the language, the Language Map (rml:LanguageMap, Listing 4.10, line 5), which extends R2RML's language tag (rr:language). The Language Map allows not only constant values for language but also references derived from the input data. rr:language is considered then an abbreviation for the rml:languageMap, as rr:predicate is for the rr:predicateMap.

## 3.2 Data Transformations: FnO

Mapping rules involve (re-)modeling the original data, describing how objects are related by specifying correspondences between data in different schemas [126], and deciding which vocabularies and ontologies to use. Data transformations, as opposed to schema transformations that the mapping rules represent, are needed to support any changes in the structure, representation or content of data [367], for instance, performing string transformations or computations.

The Function Ontology (FnO) [102,104] describes functions uniformly, unambiguously, and independently of the technology that implements them. As RML extends R2RML with respect to schema transformations, the combination of RML with FnO extends R2RML with respect to data transformations.

A function (fno:Function) is an activity which has input parameters, output, and implements certain algorithm(s) (Listing 4.11, line 1). A parameter (fno:Parameter) is a function's input value (Listing 4.11, line 4). An output (fno:Output) is the function's output value (Listing 4.11, 5). An execution (fno:Execution) assigns values to the parameters of a function for a certain execution. An implementation (fno:Implementation) defines the internal workings of one or more functions.

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

# Listing 4.13. A YARRRML set of mapping rules

## 4 [R2]RML Extensions and Alternatives

Other languages were proposed based on differentiation on (i) data retrieval and (ii) data transformations. The table below (Table 2) summarizes mapping languages extensions, their prefixes and URIs. xR2RML [306] and KR2RML [407] are the two most prominent solutions that showcase extensions and alternatives respectively for data retrieval. On the one hand, xR2RML extends R2RML following the RML paradigm to support heterogeneous data from non-relational databases. On the other hand, KR2RML extends R2RML by relying on the Nested Relational Model (NRM) [455] as an intermediate form to represent data originally stored in relational databases. KR2RML also provided an alternative for data transformations, but FunUL is the most prominent alternative to FnO.

**Table 2.** [R2]RML extensions, their URIs and prefixes

| Language | Prefix | URI |
|---|---|---|
| R2RML | rr | http://www.w3.org/ns/r2rml# |
| RML | rml | http://semweb.mmlab.be/ns/rml# |
| xR2RML | xrr | http://www.i3s.unice.fr/ns/xr2rml# |
| FnO+RML | fnml | http://semweb.mmlab.be/ns/fnml# |
| FnO | fno | https://w3id.org/function/ontology# |

### 4.1  xR2RML

xR2RML [306] was proposed in 2014 in the intersection of R2RML and RML. xR2RML extends R2RML beyond relational databases and RML to include non-relational databases. xR2RML extends R2RML following the RML paradigm but is specialized for non-relational databases and, in particular, NoSQL and XML databases. NoSQL systems have heterogeneous data models (e.g., key-value, document, extensible column, or graph store), as opposed to relational databases. xR2RML assumes, as R2RML does, that a processor executing the rules is connected to a certain database. How the connection or authentication is established against the database is out of the language’s scope, as in R2RML.

The xR2RML vocabulary preferred prefix is xrr and the namespace is the following: http://www.i3s.unice.fr/ns/xr2rml#.

*Data Source.* Similarly to RML, an xR2RML Triples Map refers to a Logical Source (`xrr:logicalSource`, Listing 4.14, line 3), but similarly to R2RML, this Logical Source can be either an xR2RML base table (`xrr:sourceName`, for databases where tables exist) or an xR2RML view representing the results of executing a query against the input database (`xrr:query`, line 4).

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

**Listing 4.14.** xR2RML logical source over an XML database supporting XQuery

*Iterator.* xR2RML originally introduced the `xrr:iterator`, according to the `rml:iterator`, to iterate over the results. In a later version, xR2RML converged using the `rml:iterator` (Listing 4.14, line 5).

*Format or Reference Formulation.* In contrast to RML that considers a formulation (`rml:referenceFormulation`) to refer to its input data, xR2RML originally specified explicitly the format of data retrieved from the database using the property `xrr:format` (Listing 4.15, line 2). For instance, RML considers XPath or XQuery or any other formulation to refer to data in XML format, xR2RML would refer to the format, e.g. `xrr:XML`. While RML allows for other kinds of query languages to be introduced, xR2RML decides exactly which query language to use. In an effort to converge with RML, xR2RML considers optionally a reference formulation.

```
```text
1   <#FemalePoleVault> xrr:logicalSource <#PoleVaultersCSVtable> ;
2   <#PoleVaultersCSVtable> xrr:format xrr:Row .
```
```

**Listing 4.15.** A CSV file on the Web as xR2RML Logical Source

*Reference.* Similarly to RML, xR2RML uses a reference (`xrr:reference`) to refer to the data elements (Listing 4.14, line 7). xR2RML extends RML’s reference to refer to data elements in data with mixed formats. xR2RML considers cases where different formats are nested; for instance, a JSON extract is embedded in a cell of a tabular structure. A path with mixed syntax consists of the concatenation of several path expressions separated by the slash ‘/’ character.

*Collections and Containers.* Several RDF terms can be generated by a *Term Map* during an iteration if multiple values are returned. This can normally generate several triples, but it can also generate hierarchical values in the form of RDF collections or containers. To achieve the latter, xR2RML extends R2RML by introducing corresponding datatypes to support the generation of containers. xR2RML introduces new *term types* (`rr:termType`): `xrr:RdfList` for an `rdf:List`, `xrr:RdfBag` for `rdf:Bag`, `xrr:RdfSeq` for `rdf:Seq` and `xrr:RdfAlt` for `rdf:Alt`. All *RDF terms* produced by the *Object Map* during one triples
