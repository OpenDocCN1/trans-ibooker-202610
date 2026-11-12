and fields. A vector space is a group. In fact, it is an abelian group. You can do addition of vectors, and the inverse. You also have a second operation scalar, multiplication, without the inverse. Note that the first operation (addition) is commutative, whereas the second operation (multiplication) is not.

So, what are basis vectors? If you have a set of elements E (i.e., vectors) in some vector space V, the set of vectors E is considered a basis if every vector in the vector space V can be written as a linear combination of the elements of E. Put another way, you could begin with the set E, the basis, and through a linear combinations of the vectors in E, create all the vectors in the vector space V. As the astute reader will have surmised, a vector space can have more than one basis set of vectors.

What is linear dependence and independence? You will see these terms a great deal later in this book as we discuss quantum computing. In the theory of vector spaces, a set of vectors is said to be linearly dependent if one of the vectors in the set can be defined as a linear combination of the others; if no vector in the set can be written in this way, then the vectors are said to be linearly independent.

A subspace is a subset of a vector space that is a vector space itself; for example, the plane z=0 is a subspace of R³ (it is essentially R²). We'll be looking at Rⁿ and subspaces of Rⁿ.

# # 1.5 Set Theory

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