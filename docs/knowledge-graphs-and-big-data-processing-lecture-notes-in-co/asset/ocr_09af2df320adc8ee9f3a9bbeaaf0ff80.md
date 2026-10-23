links are generated retrospectively by devising similarity metrics over sets of attributes of the artifact descriptions. Interlinking of such metadata provides shareable, extensible, and easily re-usable metadata in the form of KGs. We also address the scholarly domain as one of the example applications.

**Rule Mining and Reasoners.** One of the early attempts in systematic definitions of KGs goes beyond seeing them as only a representation tool but more as a management system close to database management systems.

> “A Knowledge Graph acquires and integrates information into an ontology and applies a reasoner to derive new knowledge.” – Ehrlinger, 2016 – [121].

This is one of the early attempts in defining KGs in a systematic way with a different view. Similarly, the following definitions sees KGs as a specific data model. There are several rule mining reasoners around which are purely designed to consume the ontology and mine relational patterns out of the KG. One example of this category is AMIE [144]. We categorize it under this view because it is more than just a representation tool and performs some data management steps. It has RDF as the data model for representing the facts and rules and uses its own internal functions for rule mining.

**Data Application Platform.** The VADA project [257] saw many application services built on top of its Knowledge Graph Management System (KGMS) Vadalog [164]. Before going into concrete examples, let us inspect the application service requirements given in [42]:

+   - *data cleaning, exchange, integration*: often summarized as “data wrangling”
+   - *web data extraction, interaction and IoT*: to interact with the outside world
+   - *machine learning, text mining, NLP, data analytics*: providing and interfacing with external such services. An interesting side-note is that the authors here invert the perspective: it is not always the knowledge graph system providing the application service, but sometimes also using it.
+   - *data visualization*: for providing data consumable by an end-user or analyst

Let us now proceed to concrete examples of these abstract requirements. Prime among them is:

+   - *Data Wrangling*, i.e., the whole process of bringing raw data into an integrated format amenable to Big Data Analytics [141, 257, 258]. Further services seen as key were at the data acquisition phase the application service
+   - *Data Extraction* [132, 262, 308]. Further key application services are those of *Recommender Systems* [82], including services for downstream machine-learning applications which need feature engineering. A connected but independent application platform requirement is that of
+   - *Social Choice* [89,90] where the application requirement is to choose among a number of different users’ preferences the best joint solution. A further one, for which it is somewhat debatable whether it is a management system requirement or an application service is that of