the transition from art to science and discipline. Nowadays, there are dedicated conferences and events in many fields of cryptography and information security. There are also international professional associations, such as the International Association for Cryptologic Research (IACR), whose aim is to promote the best results of research in the area.

This book is about cryptography and cryptanalysis, implementing algorithms and mechanisms using C++ with respect to standards.

# Background of Mathematical Functions

This goal of the book IS NOT to be a monograph on abstract mathematics. However, getting familiar with some of the fundamental mathematical concepts is necessary and will prove to be very useful in practical implementations. One of the most important concepts that is fundamental to cryptography is the function in the mathematical sense. A function is also known in the literature as transformation or mapping.

# Functions: One-to-One, One-Way, Trapdoor One-Way

Let’s consider as a concept a set, which is a distinct set of objects, which are known as elements of that specific set. The following example represents set A, which has the elements a, b, c, this being denoted as A = {a, b, c}.

Definition 2.1 [18]. Cryptography is defined as the study of the mathematical techniques that are related to the aspects of the information security such as confidentiality, integrity (data), authentication (entity), and authentication of the data origin.

Definition 2.2 [18]. Let’s consider that two sets A and B and rule f are defining a function. The rule f will assign to each element in A an element in B. The set A is known as the domain that characterizes the function and B represents the codomain. If a represents an element from A, written as a ∈ A, the image of a is represented by the element in B with the help of rule f; the image b of a is noted by b = f(a). The standard notation for a function f from set A to set B is represented as f: A → B. If b ∈ B, then we have a preimage of b, which is an element a ∈ A for which f(a) = b. The entire set of elements in B that have at least one preimage is known as the image of f, noted as Im(f).

Example 2.3. (function) Let’s consider sets A = {a, b, c} and B = {1, 2, 3, 4}, and the rule f from A to B as being defined as f(a) = 2, f(b) = 4, f(c) = 1. Figure 2-2 shows the