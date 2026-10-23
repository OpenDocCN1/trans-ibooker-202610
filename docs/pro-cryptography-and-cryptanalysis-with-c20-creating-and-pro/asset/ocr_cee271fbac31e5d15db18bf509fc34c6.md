# An Example

The following example [18] shows that searchable encryption (SE) is a very powerful encryption technique. The advantage is that the user may search for keywords within encrypted documents. Recall that the participants to the system are the data user, who owns a set of documents S = {D₁, ..., Dₙ}, prepares the system by generating the keys, encrypts the documents and the keywords, and stores them on a cloud server; the data owner, who is allowed to submit search queries on the cloud server; and the cloud server, which stores the documents in an encrypted format and runs the search algorithm.

The work [18] uses elliptic curves (see Chapter 9) in the searchable encryption scheme. Nowadays, elliptic curves are used in important areas such as blockchains ([14], [15]) or the Internet of Things ([16], [17]).

Figure 11-1 [18] shows an example of a searchable encryption scheme that uses elliptic-curve cryptography and is designed for a big data environment (see Chapter 15). In the work [18], the Elliptic Curve Digital Signature Algorithm (ECDSA) is used to secure the content of courses available for students on an e-learning platform. The security parameter (λ) for the key generation algorithm of the searchable encryption scheme is the private key from the ECDSA algorithm.

At this moment, there is no practical implementation of a searchable encryption scheme that can be used in a real environment, due to the technique’s complexity, although there are attempts. After an in-depth study of the current research, we could not find at this moment a practical implementation in the form of a library, module, or framework. For implementing a searchable encryption scheme, several basic guidelines should be taken into consideration before beginning the implementation:

+   - The architecture of the software application (server, database, services, etc.)
+   - The hardware components and the way they are managed for the current applications, which include security and cryptographic techniques
+   - The architecture should be designed such that processes within the searchable encryption are represented as independent algorithms such that their deployment is made correctly between the end users and the existing network infrastructure.