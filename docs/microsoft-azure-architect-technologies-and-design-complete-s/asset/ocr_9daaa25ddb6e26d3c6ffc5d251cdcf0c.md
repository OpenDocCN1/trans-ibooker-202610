Azure VM series. The same applies here. Which of the following data stores fit best with the application you are about to build?

+   - Document databases
+   - Graph databases
+   - Key/value pairs
+   - Object storage
+   - RDBMS
+   - Search engine databases
+   - Data analytics/data warehouse
+   - Shared files

The following sections cover the intents and purposes of those database models. In addition to that, the Azure Data Store or Azure Storage product that is designed for each specific model are provided as well. Later in the chapter there will be more depth provided into many of those products. The intent of the following is to provide you with the knowledge to get an understanding of available Azure Data Store and Azure Storage products and which data model they are intended to be used for.

# Document Databases

A document database is one that stores data typically in one or more of the following formats: XML, JSON, or YAML. The row in a document database consists of two columns: a key and a document. As you can see in Figure 5.3, the key, being unique, gives a means to retrieve the document directly, allowing the application to parse and process it. A powerful feature of the document database is that you can query the contents of the document itself. You could query all the documents where OriginalOwner is csharpuitar.

The content stored in the document column are relatively small and typically in the form of collections, lists, or scalar items. The document typically includes all the data for the entire entity, which can have a unique schema. What constitutes an entity is based on the requirements of the application. What you see in Figure 5.3 are two entities, one that defines a guitar and the other a programming language. Do those entities provide all the details about a guitar and C#? No, but for my purpose they contain what I need. Although the structure (schema) of the documents looks similar, they have different attributes and are therefore unique. The Azure product that is designed to meet the needs of an application requiring a document database is Azure Cosmos DB, which is described in much more detail later in the chapter.