
and fields. A vector space is a group. In fact, it is an abelian group. You can do addition of vectors, and the inverse. You also have a second operation scalar multiplication, without the inverse. Note that the first operation (addition) is commutative, whereas the second operation (multiplication) is not.

So, what are basis vectors? If you have a set of elements E (i.e., vectors) in some vector space V, the set of vectors E is considered a basis if every vector in the vector space V can be written as a linear combination of the elements of E. Put another way, you could begin with the set E, the basis, and through linear combinations of the vectors in E, create all the vectors in the vector space V. As the astute reader will have surmised, a vector space can have more than one basis set of vectors.

What is linear dependence and independence? You will see these terms a great deal later in this book as we discuss quantum computing. In the theory of vector spaces, a set of vectors is said to be linearly dependent if one of the vectors in the set can be defined as a linear combination of the others; if no vector in the set can be written in this way, then the vectors are said to be linearly independent.

A subspace is a subset of a vector space that is a vector space itself; for example, the plane z=0 is a subspace of R³ (it is essentially R²). We'll be looking at Rⁿ and subspaces of Rⁿ.

## 1.5 Set Theory

Set theory is an important part of discrete mathematics. A set is a collection of objects, which are called the members or elements of that set. If we have a set, we say that some objects belong (or do not belong) to this set, or are (or are not) in the set. We also say that sets consist of their elements.

As with much of mathematics, terminology and notation are critical. So let us begin our study in a similar fashion, building from simple concepts to more complex ones. The simplest way to define an element of a set is that we say x is a member of set A. This can be denoted as

x ∈ A

Sets are often listed in brackets. For example, the set of all odd integers < 10 would be shown as follows:

A = {1, 3, 5, 7, 9}

Also, a member of that set would be denoted as follows:

3 ∈ A

Negation can be symbolized by a line through a symbol. For example, in

2 ∉ A

2 is not an element of set A.

If a set is not ending, you can denote that with ellipses. For example, the set of all odd numbers (not just those less than 10) can be denoted as follows:

A = {1, 3, 5, 7, 9, 11, ...}

You can also denote a set using an equation or formula that defines membership in that set.

Sets can be related to each other; the most common relationships are briefly described here:

- **Union:** If you have two sets, A and B, elements that are members of A, B, or both represent the union of A and B, symbolized as A ∪ B. This is shown in Figure 1.3, with the shaded area being the area in common between A and B.

![](img/49061be7fe11d9f7f3e56114ab15acdc_44_0.png)

FIGURE 1.3 Union of A and B

- **Intersection:** If you have two sets, A and B, elements that are in both A and B are the intersection of sets A and B, symbolized as A ∩ B. If the intersection of set A and B is empty (i.e., the two sets have no elements in common), then the two sets are said to be disjoint, as illustrated in Figure 1.4.

![](img/49061be7fe11d9f7f3e56114ab15acdc_44_1.png)

FIGURE 1.4 Intersection of A and B

- **Difference:** If you have two sets, A and B, elements that are in one set but not both are the difference between A and B. This is denoted as A \ B. This is shown in Figure 1.5.

![](img/49061be7fe11d9f7f3e56114ab15acdc_45_0.png)

FIGURE 1.5 A \ B

- **Complement:** Set B is the complement of set A if B has no elements that are also in A. This is symbolized as B = Aᶜ and is shown in Figure 1.6.

![](img/49061be7fe11d9f7f3e56114ab15acdc_45_1.png)

FIGURE 1.6 B = Aᶜ

- **Double complement:** The complement of a set's complement is that set. In other words, the complement of Aᶜ is A. That may seem odd at first read, but reflect on the definition of the complement of a set for just a moment. The complement of a set has no elements in that set. Therefore, it stands to reason that to be the complement of the complement of a set, it would have to have all elements within the set.

These are basic set relationships. Now a few facts about sets:

- Order is irrelevant: {1, 2, 3} is the same as {3, 2, 1} or {3, 1, 2} or {2, 1, 3}.
- Subsets: Set A could be a subset of set B. For example, if set A is the set of all odd numbers < 10, and set B is the set of all odd numbers < 100, then set A is a subset of set B. This is symbolized as A ⊆ B.
- Power set: As you have seen, sets can have subsets. Consider set A as all integers < 10. Set B is a subset; it is all prime numbers < 10. Set C is a subset of A; it is all odd numbers < 10. Set D is a subset of A; it is all even numbers < 10. We could continue this exercise making arbitrary subsets such as E = {4, 7}, F = {1, 2, 3}, etc. The set of all subsets for a given set is called the *power set* for that set.

Sets also have properties that govern the interaction between sets. The most important of these properties are as follows:

- **Commutative law:** The intersection of set A with set B is equal to the intersection of set B with set A. The same is true for unions. Put another way, when one is considering the intersections and unions of sets, the order in which the sets are presented is irrelevant. That is symbolized as follows:

  (a) A ∩ B = B ∩ A

  (b) A ∪ B = B ∪ A

- **Associative law:** Basically, if you have three sets and the relationships between all three are all unions or all intersections, then the order does not matter. This is symbolized as follows:

  (a) (A ∩ B) ∩ C = A ∩ (B ∩ C)

  (b) (A ∪ B) ∪ C = A ∪ (B ∪ C)

- **Distributive law:** The distributive law is a bit different from the associative law, and order does not matter. The union of set A with the intersection of B and C is the same as taking the union of A and B intersected with the union of A and C. This is symbolized as follows:

  (a) A ∪ (B ∩ C) = (A ∪ B) ∩ (A ∪ C)

  (b) A ∩ (B ∪ C) = (A ∩ B) ∪ (A ∩ C)

- **De Morgan's laws:** These laws govern issues with unions and intersections and the complements thereof. These are more complex than the previously discussed properties. Essentially, the complement of the intersection of set A and set B is the union of the complement of A and the complement of B. The symbolism of De Morgan's laws are shown here:

  (a) (A ∩ B)ᶜ = Aᶜ ∪ Bᶜ

  (b) (A ∪ B)ᶜ = Aᶜ ∩ Bᶜ

These are the basic elements of set theory. You should be familiar with them before proceeding.

## 1.6 Summary

In this chapter you have learned the basic principles of linear algebra—just enough to enable you to understand the rest of this book. A few other mathematical details of linear algebra will be introduced as needed, but provided you fully understand this chapter, you can proceed. This chapter covered basic algebra, matrix mathematics, dot products, vector length, orthogonality, vector norms, and vector spaces. While this provides only an elementary introduction to linear algebra, it is important that you fully understand these concepts before proceeding.

## Test Your Skills

## REVIEW QUESTIONS

**1.** You have a set of numbers that has two operations, with the inverse of only one of those operations. The set is not necessarily commutative. What is the best description for this set?

- a. Group
- b. Abelian group
- c. Ring
- d. Field

**2.** Solve this equation:

3 \begin{bmatrix} 2 \\ 1 \\ 3 \end{bmatrix}

- a. \begin{bmatrix} 6 \\ 3 \\ 9 \end{bmatrix}
- b. \begin{bmatrix} 5 \\ 4 \\ 6 \end{bmatrix}
- c. 18
- d. 15

**3.** What is the dot product of these two vectors?

$$
\begin{bmatrix} 3 \\ 2 \\ 0 \\ \end{bmatrix} \cdot \begin{bmatrix} 1 \\ 3 \\ 2 \\ \end{bmatrix}
$$

- a. 11
- b. 10
- c. 28
- d. 0

**4.** Solve this determinant:

$$
|A| = \begin{vmatrix} 2 & 1 \\ 1 & 3 \\ \end{vmatrix}
$$

- a. 8
- b. 6
- c. 0
- d. 5

**5.** Solve this determinant:

$$
|A| = \begin{vmatrix} 1 & 2 & 3 \\ 2 & 1 & 4 \\ 3 & 1 & 2 \\ \end{vmatrix}
$$

- a. 11
- b. 12
- c. 15
- d. 10

**6.** What is the dot product of these two vectors?

$$
\begin{bmatrix} 1 \\ 2 \\ 2 \\ \end{bmatrix} \cdot \begin{bmatrix} 3 \\ 3 \\ 4 \\ \end{bmatrix}
$$

- a. 65
- b. 17
- c. 40
- d. 15
