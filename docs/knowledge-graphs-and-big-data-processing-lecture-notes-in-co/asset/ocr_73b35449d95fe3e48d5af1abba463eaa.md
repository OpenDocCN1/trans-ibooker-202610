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

# 4.2 KR2RML

KR2RML [407] extends R2RML in a different way than xR2RML. KR2RML relies on the Nested Relational Model (NRM) as an intermediate form to represent data. The data is mapped into tables by translating it into tables and rows where a column in a table can be either a scalar value or a nested table. Besides the data retrieval part, KR2RML extends R2RML with data transformations using User Defined Functions (UDFs) written in Python.

Data Source. Mapping tabular data (e.g., CSV) into the Nested Relational Model is straightforward. The model has a one-to-one mapping of tables, rows, and columns, unless a transformation like splitting on a column occurs, which will create a new column that contains a nested table.

Mapping hierarchical data (e.g., JSON, XML) into the Nested Relational Model requires a translation algorithm for each data format next to the mapping language. Such an algorithm is considered for data in XML and JSON format. If the data is in JSON, an object maps to a single row table in NRM with a column for each field. Each column is populated with the value of the appropriate field. Fields with scalar values do not need translation, but fields with array values are translated to their own nested tables: if the array contains scalar or object values, each array element becomes a row in the nested table. If the elements are scalar values like strings as in the tags field, a default column name “values” is provided. If a JSON document contains a JSON array at the top level, each element is treated like a row in a database table. If the data is in XML format,