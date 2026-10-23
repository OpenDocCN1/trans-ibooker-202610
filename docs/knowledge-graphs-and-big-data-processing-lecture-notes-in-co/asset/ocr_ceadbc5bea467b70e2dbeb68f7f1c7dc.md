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

# 4 [R2]RML Extensions and Alternatives

Other languages were proposed based on differentiation on (i) data retrieval and (ii) data transformations. The table below (Table 2) summarizes mapping languages extensions, their prefixes and URIs. xR2RML [306] and KR2RML [407] are the two most prominent solutions that showcase extensions and alternatives respectively for data retrieval. On the one hand, xR2RML extends R2RML following the RML paradigm to support heterogeneous data from non-relational databases. On the other hand, KR2RML extends R2RML by relying on the Nested Relational Model (NRM) [455] as an intermediate form to represent data originally stored in relational databases. KR2RML also provided an alternative for data transformations, but FunUL is the most prominent alternative to FnO.

**Table 2.** [R2]RML extensions, their URIs and prefixes

| Language | Prefix | URI |
|---|---|---|
| R2RML | rr | http://www.w3.org/ns/r2rml# |
| RML | rml | http://semweb.mmlab.be/ns/rml# |
| xR2RML | xrr | http://www.i3s.unice.fr/ns/xr2rml# |
| FnO+RML | fnml | http://semweb.mmlab.be/ns/fnml# |
| FnO | fno | https://w3id.org/function/ontology# |

# 4.1  xR2RML

xR2RML [306] was proposed in 2014 in the intersection of R2RML and RML. xR2RML extends R2RML beyond relational databases and RML to include non-relational databases. xR2RML extends R2RML following the RML paradigm but is specialized for non-relational databases and, in particular, NoSQL and XML databases. NoSQL systems have heterogeneous data models (e.g., key-value, document, extensible column, or graph store), as opposed to relational databases. xR2RML assumes, as R2RML does, that a processor executing the rules is connected to a certain database. How the connection or authentication is established against the database is out of the language’s scope, as in R2RML.

The xR2RML vocabulary preferred prefix is xrr and the namespace is the following: http://www.i3s.unice.fr/ns/xr2rml#.