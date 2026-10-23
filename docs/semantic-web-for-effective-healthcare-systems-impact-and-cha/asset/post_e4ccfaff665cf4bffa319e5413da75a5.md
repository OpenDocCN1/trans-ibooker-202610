
“intrinsic” properties such as the gender; “extrinsic” properties such as a patient’s name, age, gender, etc., and “Is-a” is the relationships between individual of the class object and with other objects.

## Step 6: Create instances

The next step is to build the individuals of a class object which requires 1. Select a class, 2. Create an individual instance of that class, and 3. Creating the restrictions for the class and individual. Figure 3.2 shows the ontology for the patient’s monitoring system in which Patient is the root or parent class, Name, Gender, Phone_Number and AgeGroup is the child or subclass. Again the Gender class is classified into three subclasses like Male, Female and Transgender subclass.

After constructing the ontology, the pre-processing query is started with Part-of-Speech (POS) tagging. POS Tagging is a method of classification of query that may be defined as the automatic assignment of grammatical description to query. The descriptor is called as a tag of POS which gives the semantic information of the phrase. The tag will be assigned to each word of the phrase which is called labeling. The process of tagging is to assign POS tags to words related to its syntactic category. The POS tagger will segment the word and assign the proper labeling based on the given context. Usually, words can be categorized

![](img/cc49610e64f7c15534c72aab7adc7c74_105_0.png)

Figure 3.2 Ontology for patient’s monitoring system.

into nouns, verb, adverbs, adjectives, pronouns, conjunction and their sub-categories.

- **N(oun):** The living or nonliving object or entity is called as noun. Examples are name, age etc. **N** is the symbol used in POS tagging to represent noun.
- **V(erb):** The actions of the word are called as verb. Examples of verbs are visiting, breathing, see and so on. **V** is the symbol used in POS tagging to represent verb.
- **Adj(ective):** Adjectives are used to describe the nouns or noun phrases. **ADJ** is the symbol used in POS tagging to represent adjective.
- **Adv(erb):** Adverbs are the modifiers of either nouns, adjectives, verbs, or other adverbs. **ADV** is the symbol used in POS tagging to represent adverb.

Noun POS tag can be further classified into singular nouns (NN), singular proper nouns (NNP), and plural nouns (NNS). The Patient’s monitoring system utilizes, POS tagger which identifies the query and extracts the root word from the user’s input query. For example, the query “Find the patient’s name affected by Corona?” will be classified as follows:

```
[S [NP Find][DT the][VP [V patient’s name affected][IN by] [NP Corona]]]
```

After completing the POS tagger process, built a parse tree to find the query keywords and to understand the concept quickly. This is implemented by the help of Stanford Parser. Parsing is the process of splitting the sentence into words to find out the grammatical relationship between each word in a graphical structure representation. It is a splitting process of sentence into atomic words, which helps to investigate data, the structure of the sentence and also it is meaning. The parse tree identifies the basic connection between a sentence and the grammar by giving the detail analyze of describing how the grammar was used to produce the sentence. The parsing technique includes Top-Down and Bottom-Up is used. These techniques describe the direction of the parsing process [32]. Figure 3.3 shows the example parse tree for a query.

To find out the answer for a particular query, we need to depend on search engines by computing the terms in the query [18]. After completing the pre-processing, the root word has to be extracted to retrieve the

![](img/cc49610e64f7c15534c72aab7adc7c74_107_0.png)

Figure 3.3 Parse tree for patient's monitoring system.

result for the query. The keyword extraction is executed by calculating the TF*IDF weight of the term. It is calculated according to the formula:

```
TF(t) = (Number of times term ‘t’ appears in a document) / (Total number of terms in the document) (3.1)
```

```
IDF(t) = logₑ (Total number of documents) / Number of documents with term ‘t’ in it (3.2)
```

```
TF*IDF(term) = TF(t) * IDF(t) (3.3)
```

where TF(term) indicates the frequency of a term in the given document, IDF indicates Inverse Document Frequency.

After the query analysis with the support of POS tagger, the extracted keyword will be used to find their results. Before extracting the results, with the support of Description Logic there is a necessitate writing SWRL rules for the given concept and the relationship with the classes. In OWL (Web Ontology Language), rules are expressed with the help of SWRL (Semantic Web Rule Language) is the rule language of the semantic web. In SWRL Rule processing, the user query has been taken as the input and process into the rule engine with the help of previous knowledge.

In SWRL the facts and rules were stored in the Rule database where facts are considered as a class or subclass and rules are used as URI reference. Depending on the user query, the rule engine checks for the rules for the given query and produces the results with the support of the inference engine which acts as the intermediate to produce the result. Figure 3.4 shows the results of SWRL rule for the Patient's Monitoring System.

```
Rule 1: hasName(?x, ?y) -> hasFName(?x, ?y), hasLName(?x, ?y)
Rule 2: Patient (?x), hasName(?x, ?y), hasAge(?y) -> hasGender(?x, ?y)
```

![](img/cc49610e64f7c15534c72aab7adc7c74_108_0.png)

Figure 3.4 SWRL for patient's monitoring system.

Rule 3: Patient (?x), hasName(?x, ?y), hasSymtoms(?y) -> hasDisease(?x, ?y)
Rule 4: hasDisease (?x, ?z) -> hasName(?x, ?y)
Rule 5: hasDisease (?x, ??z) -> select(?x), count(?z), orderByDescending(?z)

## 3.3.2 Reasoner in Patient Monitoring

Reasoner is used to get the data from the knowledge repository with the support of the inference engine. At the time of ontology constructions, Reasoners helps to detect inconsistent ontologies. By correcting the inconsistent ontology, the quality of ontologies is preserved [19]. The entire query is validated for the correctness with the three reasoners namely Fact++, HermiT and Pellet. Pellet is the best reasoner by comparing the other two reasoners because of its debugging feature. It is open-source software implemented in Java and it follows tableau algorithm. To handle the inconsistent ontology, Pellet follows the following guidelines.

- service clash detection is method of detecting the root inconsistency
- axiom tracing is used to find irrelevant ontology by creating more number of axioms on objects.

Reasoners are integrated with 450 significantly diverse ontologies that achieve high accuracy result for the prediction model. Such prediction models have a wide range of applications. It gives better accuracy even for the large and complex ontology [11]. FaCT++ is open-source software, which follows tableaux-based algorithm and it supports C++ language. It follows description logic on the classes. The concepts and its related instance can be represented using OWL [22, 25].

HermiT is another reasoner used to identify the inconsistent ontology and also clearly identify the relationships between classes, and its objects.

It follows novel hypertableau algorithm, which is having well-organized reasoning capacity. The pallet is having the best features comparing to other reasoners.

- Consistency checking is used to find out the contradictory ontology.
- Concept satisfiability deals with many instances for a particular class.
- Classification creates the class hierarchy for every class and its subclasss
- Realization helps to identity the individuals each class.

Jena is an open source software which includes different reasoning modules such as Assertional Box (Abox), Terminological Box (Tbox), and Knowledge Base (KB). A box describes the assertions about individuals, example type and property value. Realizing the ABox, defines the most precise concepts of each individual and the relationship with its instances. The list of axioms on the classes and subclasses can be defined in the Tbox. Any Knowledge Base can be created with the combination of an ABox and a TBox to form a complete OWL ontology.

Jena ontology API is an ontology language and Java ontology class represents OWL class, RDFS class, or DAML class. Every ontology model is bounded with a Profile [12]. The inference made by reasoners follows the inductive reasoning and gone through the more number of facts and rules. It helps the medical practitioners to take an effective decision on the patient’s health issue. So that the system will act as a better recommendation system. Therefore, the patients too believe that the given information is much reliable and the accuracy is also much relevant than the system which is not undergone with the reasoners.

## 3.4 Experimental Results

Before executing the query, all the pre-processing, keyword extraction, rule construction and validation must be done by the reasoner. Finally, the query has been executed with SPARQL. Then the comparison has been made with the traditional database. As a result, it is proved that the result of ontology is more accurate and semantic. Whatever the problem is identified in the traditional database has been recovered.
