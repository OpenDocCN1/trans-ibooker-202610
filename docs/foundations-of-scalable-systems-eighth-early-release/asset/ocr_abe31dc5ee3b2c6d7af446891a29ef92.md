# NORMALIZATION

The design of relational databases encourages normalization. Normalization structures the business domain data to eliminate data redundancy and support data integrity.

Normalization is a complex topic that is beyond the scope of this book. In a nutshell, the result of normalization is a data model that adheres to the rules described by one of six – yes, six - major normal forms.⁸ Each normal form defines rules for how the domain data should be organized into a collection of tables and columns.

In reality, many databases I have seen over many years are designed to the rules defined by third normal form (3NF). I’ve heard rumors of fourth normal form databases, but suspect any higher normal forms have never left the environs of academia.

Essentially, 3NF data models are designed to simplify data management. Domain data is split amongst multiple relations such that every data item has a single entry which can be referenced by a unique identifier when required. Data in 3NF data models can be mechanically translated into a relational schema and instantiated by a relational database engine. Applications can then use the SQL query language to INSERT, UPDATE, SELECT and DELETE data from the database.

It’s not uncommon however for relational data models to be demoralized to enhance query performance and application scalability. This insight is one of the key tenets that underpins the simpler data models that are supported by NoSQL databases.

# NoSQL Data Models

As illustrated in Figure 10-6, there are four main NoSQL data models, all of which are somewhat simpler than the relational model.