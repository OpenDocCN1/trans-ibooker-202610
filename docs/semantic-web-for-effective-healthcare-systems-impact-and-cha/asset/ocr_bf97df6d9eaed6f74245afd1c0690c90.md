searching [22]. The search engine also matches search terms against the MeSH headings to find documents.

+   - **Ranking Results:** Results are not sorted based on relevance but are by default sorted by date. Further filtering can be done on the basis of author, journal or title.

# 4.3.4.3.2 Quertle

It relies on natural language processing techniques to match search against actual assertions made by the author, rather than simple keyword matching or proximity. It uses ontology extensively so that all the meanings, forms, aliases of the search term are applied automatically [40]. The ontology also allows sub concepts to be found; for example, a search for “rodent” will find “mice,” “rats,” etc.

+   - **Ranking Results:** Relevance of the results is decided by the number of times the document supports the assertion.

# 4.3.4.3.3 FACTA+

It is an index based entity search engine [41]. It is used for finding associated biomedical concepts and these concepts are presented to user along with textual evidence of associations between query item and concept. Several biomedical databases like UniProt, BioThesaurus, UMLS, KEGG, and DrugBank are used for building indexes using conceptID and their names/synonyms.

+   - **Ranking Results:** The ranking is done based on co-occurrence statistics. The search space that is uses is PubMed.

# 4.3.4.3.4 THALIA

This semantic search engine recognizes concepts occurring in biomedical abstracts [42]. Thalia stands for Highlighting, Aggregating and Linking Information in Articles. It uses text mining tool Argo to recognize the entities using various models based on dictionary matching and CRF models trained using human annotated data.

+   - **Ranking Results:** The search space that it uses is PubMed. Same concept is present in the query and the retrieved documents. Searching API also has options for filtering