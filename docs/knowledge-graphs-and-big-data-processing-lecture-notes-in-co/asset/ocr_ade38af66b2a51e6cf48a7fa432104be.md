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

# 3.2 Data Transformations: FnO

Mapping rules involve (re-)modeling the original data, describing how objects are related by specifying correspondences between data in different schemas [126], and deciding which vocabularies and ontologies to use. Data transformations, as opposed to schema transformations that the mapping rules represent, are needed to support any changes in the structure, representation or content of data [367], for instance, performing string transformations or computations.

The Function Ontology (FnO) [102,104] describes functions uniformly, unambiguously, and independently of the technology that implements them. As RML extends R2RML with respect to schema transformations, the combination of RML with FnO extends R2RML with respect to data transformations.

A function (fno:Function) is an activity which has input parameters, output, and implements certain algorithm(s) (Listing 4.11, line 1). A parameter (fno:Parameter) is a function's input value (Listing 4.11, line 4). An output (fno:Output) is the function's output value (Listing 4.11, 5). An execution (fno:Execution) assigns values to the parameters of a function for a certain execution. An implementation (fno:Implementation) defines the internal workings of one or more functions.