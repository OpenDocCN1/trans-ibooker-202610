

medical files can be searched in an encrypted form. Other applications are in education, business, and basically in any domain that requires search processes through data.

## Components

The components of a searchable encryption scheme are as follows: the entities and the algorithms. In this section, we present these components in a detailed view.

## Entities

When a software solution is implemented, more aspects should be clarified before the implementation itself: the clients who will use the application, the entity that will maintain it, the type of data, the roles supported by the application, and so on.

In a system that uses a searchable encryption scheme, the following entities are involved in the whole process:

- **Data owner:** The data owner, who is assumed to be a trusted party, has a number of *n* documents, *D* = {*D*₁, …, *Dₙ}, which are characterized by keywords (note that these keywords are not metadata). Both the documents and the keywords will be outsourced. Prior to outsourcing the documents (and the keywords, which are often organized into a structure called *index structure*) on the server, they are encrypted by the data owner using an encryption algorithm of a searchable encryption scheme.

- **Data user:** The data user, who is an authorized user of the data, may trigger the search process. Using the query keyword for which the search will be made, the data user generates a trapdoor value that will be used when searching over the encrypted data. Also, the data user may decrypt the documents from the search process if the data user possesses the private key. Note that the data owner can be a data user.

- **Server:** The server, which is considered semi-trusted or honest-but-curious, stores the encrypted data and performs the search algorithms based on the trapdoor value that it receives from the data user. *Semi-trusted or honest-but-curious* means that it performs the search algorithms as instructed but can analyze the data that was given to it.

## Types

From the cryptographic point of view, searchable encryption schemes can be categorized as follows: *symmetric searchable encryption* (SSE) schemes and *public key encryption with keyword search* (PEKS) schemes. Symmetric searchable encryption schemes use just a key for the encryption or decryption of the content and additionally in other specific algorithms, as you will see below. Public-key encryption schemes with keyword search use two keys, namely a public key to encrypt content and a private (or secret) key to decrypt the encrypted content.

The SSE schemes contain the following algorithms [1]:

- **KeyGeneration**: The data owner runs this algorithm. The input is a security parameter λ, and the output is the secret key SK.
- **BuildIndex**: The data owner runs this algorithm. Its purpose is to generate a structure of indices that will contain the keywords that describe the documents. The input is the secret key SK and the set of documents D that will be stored on the server, while the output is an index structure I. Specifically, this algorithm begins with an empty index structure and for every document of the set, it appends to the index structure some keywords that describe the current document. Note that the keywords are encrypted using the secret key SK in a specific way that can be different from the encryption of the documents before being added to the index structure. The index structure can be a tree, a hash table, a list, etc.
- **Trapdoor**: The data user runs this algorithm. The input for the trapdoor algorithm is the desired query keyword kw, for which the search process is triggered, and the secret key SK, while the output is a value T_kw called trapdoor. Note that the trapdoor algorithm does not just encrypt the query keyword kw. Instead, it adds a noise value or works with something of the sort.
- **Search**: The server runs the search algorithm. The input for the search algorithm is the trapdoor value T_kw obtained from the previous algorithm and the index structure I obtained from the BuildIndex algorithm. Note that the search algorithm does not just try to match the trapdoor T_kw in I. The search algorithm should specify how the trapdoor value T_kw is searched in the index structure (remember that T_kw is not a simple encryption of a plain keyword).

If the search algorithm returns one or more documents that contain the query keyword, the documents are sent to the data user, or else the server sends a proper message. Note that the encryption and decryption algorithms are not listed above. That is because two different encryption schemes can be chosen by the data owner, namely one to encrypt the documents and one for the searchable encryption scheme. This situation is possible because the searchable encryption scheme does not directly involve the documents. All algorithms of an SSE scheme work only with the keywords and/or the index structure of encrypted keywords.

A little different from the SSE version, the algorithms of a PEKS scheme are as follows [2]:

- **KeyGeneration**: This algorithm is similar to the KeyGeneration from SSE, and the data owner runs this algorithm, too. The input is also a security parameter λ, while this time the output of the key generation is a pair of keys, namely the public and the private keys (PK, SK).
- **Encryption**: The data owner runs this algorithm, for which the public key PK and a keyword KW are the input values, while the output is the encrypted value SW of KW.
- **Trapdoor**: Similar to the trapdoor algorithm from SSE, the data user runs this algorithm to generate the trapdoor value. The input is the secret key SK and the query keyword KW for which the search is made, while the output is the trapdoor value T_KW corresponding to the keyword KW.
- **Test**: The server runs the test algorithm, for which the input is the public key PK, an encrypted value C (representing the encryption of a keyword KW'), and the trapdoor value T_KW. The output of the test algorithm is 1 if KW' = KW, and 0 otherwise.

The same remarks apply to the trapdoor and the test algorithm; the algorithms do not just offer encryption or simple matches, respectively. Still, the above algorithms for the SSE schemes and PEKS schemes are presented according to their introductions in this field in early works [1] and [2]. Since then, the algorithms were adapted alongside the options that the search process supported. Namely, multiple keywords search is allowed in some works, others enable fuzzy search (which allows small typos or inconsistencies of the format) based on keywords [3, 4], and yet others enable a semantic search (the search process returns documents that contain keywords from the semantic field of the query keyword) [5], etc. Other works focus on the documents (specifically, the documents can be updated directly on the server, without it being necessary to retrieve them from the server, decrypt, update, encrypt, and store them again on the server); other works focus on the index structure that can be updated directly on the server [6]. However, the algorithms contained in any searchable encryption scheme are the trapdoor and the search/test algorithms, and, of course, the encryption and decryption.

## Security Characteristics

There are some things that need to be protected in a searchable encryption such as the *search pattern* and the *access pattern*. The *search pattern* is the information that can be discovered from the fact that two different search results belong to the same query keywords. The *access pattern* is the set of documents resulting from a trapdoor corresponding to a given keyword *KW*. Besides, searchable encryption schemes should meet security requirements related to search queries, too. According to [7], SE schemes should have the following characteristics: controlled searching (search queries may be submitted only by authorized users), encrypted queries (the search query itself should be encrypted before being submitted to the server), and query isolation (the server learns nothing from the queries that it receives).

The SSE schemes should be IND1-CKA and/or IND2-CKA (chosen keyword attack for indexes) resistant, which means that the index structure cannot be compromised. In IND1-CKA, the same number of keywords is chosen for all documents used in the build index structure, while in IND2-CKA the documents can be described by a different number of keywords. On the other hand, the PEKS scheme should be resistant to the chosen keyword attack (which is a challenge between an attacker and the structure that manages the PEKS scheme).

Recently introduced security requirements are forward and backward privacy for the dynamic SE schemes, which allow inserting, updating, or deleting to be applied over the set of documents or the keywords directly on the server, without the need of decrypting them. Backward and forward privacies refer to the information that can be discovered in the process of inserting/deleting/updating. *Backward privacy* refers to the information that is discovered when the search is made for a keyword for which documents have been deleted before the current search, while *forward privacy* means that the current update operation is not related to previous operations.

