be done by Fuzzy Reasoning. Fuzzy ontologies were applied in diversified domains [43], namely in human behavior [44], modeling in the diabetes field especially in diet and diagnosis [45]. The application of fuzzy ontology with a rule based system is not widely used compared to another system because of its essential needs to bring together unambiguous models of domains [43].

Mainly there are five important steps involved in the reasoners based approach, they are as follows:

+   **Step 1**
+   *Extract information from the knowledge base*
+   **Step 2**
+   *Identify any inconsistent ontology*
+   **Step 3**
+   *Perform the correctness check using fuzzy reasoner*
+   **Step 4**
+   *Generate new rule*
+   **Step 5**
+   *Accomplish the validation of the inference*

# 6.5 Implementation and Results

In the final stage of the medicine information retrieval can be achieved SPARQL (Simple Protocol and RDF Query Language) which SPARQL agrees for a query to involve of triple patterns, conjunctions, disjunctions, and optional patterns. The medicine ontology executed with the help of SPARQL SPARQL query output displayed in Figure 6.5.

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX med: <http://www.medinfo.com/med#>
SELECT ?Medicine ?MComp
WHERE { ?Medicine med:MComp ?MComp }
```

# 6.6 Conclusion and Future Prospective

Ontology defines dependencies between entities, which permits an RDF store to enhance users’ answers with relevant results not explicitly defined