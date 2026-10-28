Just as API consumers rely on standardized protocols (e.g. HTTP) and standardized message formats (e.g. SIREN, Collection+JSON, etc.), they also can benefit from standardized vocabularies. That means listing all the data properties and action names in a standard format that the client understands.

This means there needs to be a single source for all the possible names of things that might appear in your API. For example, any value that might appear in attributes like `id`, `name`, `class`, `rel` or similar structural elements. A collection of these values can cover a topic or domain (like “User Management” or “Accounting”).

There are couple options for carrying vocabulary data in a standardized form. The best-known solution is to use an Resource Description Framework (RDF)⁵⁴ format like RDF/XML⁵⁵, Turtle⁵⁶, and JSON-LD⁵⁷. The JSON-LD format is the one I see more often when APIs want to use an RDF-based format.

The format I use to document vocabularies is the Application-Level Profile Semantics (ALPS)⁵⁸ format. There are serializations of ALPS for XML, JSON, and even YAML.

# ## Example

Below is an RDF version of a `person` expressed in the FOAF (Friend of a Friend)⁵⁹ ⁶⁰ vocabulary.

```
```turtle
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

<#JW>
        a foaf:Person ;
        foaf:name "James Wales" ;
        foaf:mbox <mailto:jwales@bom is.com> ;
        foaf:homepage <http://www.jameswales.com> ;
        foaf:nick "Jimbo" ;
        foaf:depiction <http://www.jameswales.com/aus_img_small.jpg>
;
        foaf:interest <http://www.wikimedia.org> ;
        foaf:knows [
```
```