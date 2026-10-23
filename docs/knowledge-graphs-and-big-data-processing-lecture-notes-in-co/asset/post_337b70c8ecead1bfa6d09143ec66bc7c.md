
their knowledge. First of all, RDF comes with an associated query language (SPARQL) in order to extract data from sources. Moreover, several standards specify how to structure the data:

- 1. The RDF Schema (RDFS) lists a set of classes with certain properties using the RDF representation data model and provides basic elements for the description of ontologies.
- 2. The Web Ontology Language (OWL) is a family of knowledge representation languages for authoring ontologies which are a formal ways to describe taxonomies and classification networks, essentially defining the structure of knowledge for various domains.
- 3. The Shapes Constraint Language (SHACL) allows to design validations over graph-based data considering a set of conditions. Among others, it includes features to express conditions that constrain the number of values that a property may have, the type of such values, numeric ranges etc. . . .

These specifications then allow users to specify several properties about Semantic Web data and therefore one can use them to extend one’s own knowledge. Indeed, ontologies are the cornerstone of all the studies made around inferring data from a set of triples e.g. using the structure of the graph, it is possible to “materialize” additional statements and thereby to extend the general knowledge.

As a consequence, the W3C – via the diverse standards and recommendations it set up – allows users to structure pieces of information. However, the large majority of existing tools are focusing on one or two standards at once, meaning that they are usually not encompassing the full scope of what the Semantic Web is supposed to provide and enable. Indeed, designing such a “wide-scope” Semantic Web tool is challenging. Recently, such an initiative was created: SANSA [411]; in addition, SANSA also pays attention to the Big Data context of the Semantic Web and adopts a fully distributed strategy.

## 3 Processing Big Knowledge Graphs with SANSA

In a nutshell, SANSA¹ presents:

- 1. efficient data distribution techniques and semantics-aware computation of latent resource embeddings for knowledge graphs;
- 2. adaptive distributed querying;
- 3. efficient self-optimising inference execution plans; and
- 4. efficient distributed machine learning on semantic knowledge graphs of extremely large scale.

### 3.1 Knowledge Representation and Distribution

SANSA follows the modular architecture where each layer represents a unique component of functionality, which could be used by other layers of the SANSA framework. The Knowledge Representation and Distribution is the lowest layer

on top of the existing distributed computing framework (either Apache Spark $ ^{2} $ or Apache Flink $ ^{3} $ ). Within this layer, SANSA provides the functionality to read and write native RDF or OWL data from HDFS or a local file and represents it in native distributed data structures of the framework. Currently, it supports different RDF and OWL serializations/syntax formats. Furthermore, it provides a dedicated serialization mechanism for faster I/O. The layer also supports Jena and OWL API interfaces for processing RDF and OWL data, respectively. This particularly targets usability, as many users are already familiar with the corresponding libraries.

This layer also gives access to a mechanism for RDF data compression in order to lower the space and processing time when querying RDF data (cf. Sect. 3.2 ). It also provides different partitioning strategies in order to facilitate better maintenance and faster access to this scale of data. Partitioning the RDF data is the process of dividing datasets in a specific logical and/or physical representation in order to ease faster access and better maintenance. Often, this process is performed to improve the system availability, load balancing and query processing time. There are many different data partitioning techniques proposed in the literature. Within SANSA, we provide 1) semantic-based partitioning [392], 2) vertical-based partitioning [411], and 3) graph-based partitioning.

*Semantic-based partitioning* – A semantically partitioned fact is a tuple $(S, R)$ containing pieces of information $R \in (P, O)$ about the same $S$ where $S$ is a unique subject on the RDF graph and $R$ represents all its associated facts i.e. predicates $P$ and objects $O$. This partitioned technique was proposed in the SHARD [376] system. We have implemented this technique using the in-memory processing engine, Apache Spark, for better performance.

*Vertical partitioning* – The vertical partitioning approach in SANSA is designed to support extensible partitioning of RDF data. Instead of dealing with a single three-column table $(s, p, o)$, data is partitioned into multiple tables based on the used RDF predicates, RDF term types and literal datatypes. The first column of these tables is always a string representing the subject. The second column always represents the literal value as a Scala/Java datatype. Tables for storing literals with language tags have an additional third string column for the language tag.

In addition, this layer of SANSA allows users to compute RDF statistics [391] and to apply quality assessment [393] in a distributed manner. More specifically, it provides a possibility to compute different RDF dataset statistics in a distributed manner via the so-called DistLODStats [392] software component. It describes the first distributed in-memory approach for computing 32 different statistical criteria for RDF datasets using Apache Spark. The computation of statistical criteria consists of three steps: (1) saving RDF data in scalable storage, (2) parsing and mapping the RDF data into the *main dataset* – an RDD data structure composed of three elements: *Subject*, *Property* and *Object*, and (3) performing statistical criteria evaluation on the *main dataset* and generating results.

![](img/c21ce6788ba392bc08a8057d8b20603e_113_0.png)

Fig. 2. Overview of DistLODStats’s abstract architecture [392].

Fetching the RDF Data (Step 1): RDF data needs first to be loaded into a large-scale storage that Spark can efficiently read from. For this purpose, we use HDFS (Hadoop Distributed File-System). HDFS is able to accommodate any type of data in its raw format, horizontally scale to arbitrary number of nodes, and replicate data among the cluster nodes for fault tolerance. In such a distributed environment, Spark adopts different data locality strategies to try to perform computations as close to the needed data as possible in HDFS and thus avoid data transfer overhead.

Parsing and Mapping RDF into the Main Dataset (Step 2): In the course of Spark execution, data is parsed into triples and loaded into an RDD of the following format: Triple<Subj,Pred,Obj> (by using the Spark map transformation).

Statistical Criteria Evaluation (Step 3): For each criterion, Spark generates an execution plan, which is composed of one or more of the following Spark transformations: map, filter, reduce and group-by. Filtering operation apply the Rule’s Filter and produce a new filtered RDD. The filtered RDD will serve as an input to the next step: Computing where the rule’s action and/or post processing are effectively applied. The output of the Computing phase will be the statistical results represented in a human-readable format, e.g. VoID, or row data.

Often when designing and performing large-scale RDF processing tasks, the quality of the data is one of the key components to be considered. Existing solutions are not capable of dealing with such amounts of data, therefore a need for a distributed solution for a quality check arises. To address this, within SANSA we present DistQualityAssessment [393] – an open-source implementation of quality assessment of large RDF datasets that can scale out to a cluster of machines. This is the first distributed, in-memory approach for computing different quality metrics for large RDF datasets using Apache Spark. We also provide a quality assessment pattern that can be used to generate new scalable metrics that can be applied to big data. A more detailed overview of the approach is given below. The computation of the quality assessment using the Spark framework consists of four steps:
