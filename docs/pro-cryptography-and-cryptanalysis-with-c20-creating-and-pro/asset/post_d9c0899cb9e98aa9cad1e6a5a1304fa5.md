
- *Non-repudiation* is a service that prevents denials of previous actions made by an entity. When a conflict occurs because an entity denies its previous actions, it will be resolved by an existing trusted third party that will show the actions made over data.

One of the main goals of cryptography is to fulfill the four objectives described above on both sides, theory and practice.

## Cryptographic Primitives

During the book, we will present several fundamental cryptographic tools, called *primitives*. Examples of primitives are encryption schemes (Definitions 2.5 and 2.8), hash functions (Definition 2.9), and schemes for digital signatures (Definition 2.6). Figure 2-1 shows a schematic description of these primitives and the relation between them. We will use many of the cryptographic primitives during the book, and we will provide practical implementations every time we use them. Before using them in real-life applications, the primitives should be subjected to an evaluation process in order to check if the below criteria are fulfilled:

- **Level of security**: It is slightly difficult to quantify the level of security. However, it can be quantified as the number of operations made in order to accomplish the desired objective. The level of security is usually defined based on the superior bound given by the volume of work necessary to defeat the objective.

- **Functionality**: To accomplish security objectives, in many situations the primitives are combined. You need to be sure that they work properly.

- **Operation methods**: When the primitives are used, they need different inputs and have different ways of working, resulting in different characteristics. In these situations, the primitives provide very different functionality that will depend on the mode of operation.

- **Performance**: This concept is related to the efficiency that a primitive can achieve in a specific and particular mode of operation.

- **Ease of implementation**: This concept is more a process than a criterion, and it refers to the primitive be used in practice.

![](img/c3d9424f64735276d661e7aba725b2a2_34_0.png)

## Figure 2-1. Cryptographic primitives taxonomy

The application and the available resources give importance to each of the above criteria.

Cryptography may be seen as an art practiced by professionals and specialists who proposed and developed ad-hoc techniques whose purpose was to fulfill important information security requirements. In the last few decades, cryptography has suffered

the transition from art to science and discipline. Nowadays, there are dedicated conferences and events in many fields of cryptography and information security. There are also international professional associations, such as the International Association for Cryptologic Research (IACR), whose aim is to promote the best results of research in the area.

This book is about cryptography and cryptanalysis, implementing algorithms and mechanisms using C++ with respect to standards.

## Background of Mathematical Functions

This goal of the book IS NOT to be a monograph on abstract mathematics. However, getting familiar with some of the fundamental mathematical concepts is necessary and will prove to be very useful in practical implementations. One of the most important concepts that is fundamental to cryptography is the function in the mathematical sense. A function is also known in the literature as transformation or mapping.

## Functions: One-to-One, One-Way, Trapdoor One-Way

Let’s consider as a concept a set, which is a distinct set of objects, which are known as elements of that specific set. The following example represents set A, which has the elements a, b, c, this being denoted as A = {a, b, c}.

Definition 2.1 [18]. Cryptography is defined as the study of the mathematical techniques that are related to the aspects of the information security such as confidentiality, integrity (data), authentication (entity), and authentication of the data origin.

Definition 2.2 [18]. Let’s consider that two sets A and B and rule f are defining a function. The rule f will assign to each element in A an element in B. The set A is known as the domain that characterizes the function and B represents the codomain. If a represents an element from A, written as a ∈ A, the image of a is represented by the element in B with the help of rule f; the image b of a is noted by b = f(a). The standard notation for a function f from set A to set B is represented as f: A → B. If b ∈ B, then we have a preimage of b, which is an element a ∈ A for which f(a) = b. The entire set of elements in B that have at least one preimage is known as the image of f, noted as Im(f).

Example 2.3. (function) Let’s consider sets A = {a, b, c} and B = {1, 2, 3, 4}, and the rule f from A to B as being defined as f(a) = 2, f(b) = 4, f(c) = 1. Figure 2-2 shows the

The scheme represents the main fundamental tool for thinking of a function (you can find it the literature as the functional diagram) as depicted in Figure 2-2, and each element from the domain A has precisely one arrow originated from it. For each element from codomain B we can have any number of arrows as being incident to it (including also zero lines).

Example 2.5. (function) Let’s consider the following set defined as A = {1, 2, 3, …, 10⁵⁰} and consider the f to be the rule f(a) = rₐ where rₐ represents the remainder in the case when a² is divided by 10⁵⁰ + 1 for all a ∈ A. In this situation, it is not feasible to write down f explicitly as in Example 2.4. This being said, the function is completely defined by the domain and the mathematical description that characterize the rule f.

## One-to-One Functions

Definition 2.6 [18]. We will consider a function or transformation as 1 – 1 (one-to-one) if each of the elements that can be found within the codomain B is represented as the image of at most one element in the domain A.

Definition 2.7 [18]. Let’s consider that a function or transformation is onto if each of the elements found within the codomain B represents the image of at least one element that can be found in the domain. At the same time, function f: A → B is known as being onto if Im(f) = B.

Definition 2.8 [18]. Let’s consider a function f: A → B to be considered 1 – 1 and Im(f) = B. Then the function f is called bijection.

Conclusion 2.9 [18]. If f: A → B is considered 1 – 1, then f: A → Im(f) represents the bijection. In special cases, if f: A → B is represented as 1 – 1, and A and B are represented as finite sets with the same size, then f represents a bijection.

Using the scheme and its representation, if f is a bijection, then each element from B has exactly one line that is incidental with it. The functions in Examples 2.3 and 2.4 do not represent bijections. As you can see in Example 2.3, element 3 doesn’t have the image of any other element that can be found within the domain. In Example 2.4, each element from the codomain is identified with two preimages.

Definition 2.10 [18]. If f is a bijection from A to B, then it is a quite simple matter to define a bijection g from B to A as follows: for each b ∈ B, we define g(b) = a where a ∈ A and f(a) = b. The function g is obtained from f and it is called inverse function of f and it denoted as g = f⁻¹.

![](img/c3d9424f64735276d661e7aba725b2a2_38_0.png)

![](img/c3d9424f64735276d661e7aba725b2a2_38_1.png)

## Figure 2-3. Representation of bijection f and its inverse, g = f^{-1}

Example 2.11. (inverse function) Let’s consider sets A = {a, b, c, d, e} and Y = {1, 2, 3, 4, 5}, and consider the rule f which is given and represented by the lines from

Figure 2-3. f represents a bijection and its inverse g is formed by reversing the sense of the arrows. The domain of g is represented by B and the codomain is A.

Note that if f is a bijection, f⁻¹ is also a bijection. The bijections in cryptography are tools used for message encryption. The inverse transformations are used for decryption. The main condition for decryption is for transformations to be bijections.

## One-Way Functions

In cryptography there are a certain types of functions that play an important role. Due to the rigor, a definition for a one-way function is given as follows.

Definition 2.12 [18]. Let’s consider a function f from a set A to a set B that is called a one-way function if f(a) proves to be simple and easy to be computed for all a ∈ A but for “essentially all” elements b ∈ Im (f) it is computationally infeasible to manage to find any a ∈ A in such way that f(a) = b.

Note 2.13 [18]. This note represents some additional notes and clarifications of the terms used in Definition 2.12.

- 1. For the terms easy and computationally infeasible, a rigorous definition is necessary but it will distract attention from the general idea that is being agreed upon. For the goal of this chapter, the simple and intuitive meaning is sufficient.
- 2. The words essentially all stand for the idea that there are a couple of values b ∈ B for which it is easy to find an a ∈ A in such way that b = f(a). As an example, one may compute b = f(a) for a small number of a values and then for these, the inverse is known by a table look-up. A different way to describe this property of a one-way function is as follows: for any random b ∈ Im (f) it is computationally feasible to have and find any a ∈ A in such way that f(a) = b.

The following examples will show the concept behind a one-way function.

Example 2.14. (one-way function) Consider A = {1, 2, 3, ..., 16} and let’s define f(a) = rₐ for all the elements a ∈ A where rₐ represents the remainder when 3ˣ will be divided with 17.

| a | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| f(a) | 3 | 9 | 10 | 13 | 5 | 15 | 11 | 16 | 14 | 8 | 7 | 4 | 12 | 2 | 6 | 1 |
