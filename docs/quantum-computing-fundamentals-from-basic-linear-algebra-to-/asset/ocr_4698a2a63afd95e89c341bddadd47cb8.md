Finding the angle from the cosine is straightforward; you probably did this in secondary school trigonometry. However, even with just the dot product, we have some information. If the dot product is 0, then the vectors are perpendicular. This is because the cos θ of a 90° angle is 0. The two vectors are referred to as orthogonal.

Recall that the length of a vector we will see frequently later in this book. If that length/norm is 1, it is the unit vector. This leads us to another term we will see frequently later in this book. If two vectors are both orthogonal (i.e., perpendicular to each other) and have unit length (length 1), the vectors are said to be orthonormal.

Essentially, the dot product is used to produce a single number, a scalar, from two vertices or two matrices. This is contrasted with the tensor product. In math, a tensor is an object with multiple indices, such as a vertex or array. The tensor product of two vector spaces V and W, V ⊗ W, is also a vector space.

Another special type of matrix is a unimodular matrix. Unimodular matrices are also used in some lattice-based algorithms. A unimodular matrix is a square matrix of integers with a determinant of +1 or -1. Recall that a determinant is a value computed from the elements of a square matrix. The determinant of a matrix A is denoted by |A|.

Cyclic lattices are also used in some cryptographic applications. A cyclic lattice is a lattice that is closed under a rotational shift operator. A more rigorous definition follows:

A Lattice L ⊆ Z^n is cyclic if ∀ x ∈ L: rot(x) ∈ L.

The symbols here might be a bit confusing to some readers. The symbol ⊆ means “a subset of.” So, we are saying this lattice is a subset of the integers (Z). It is cyclic if there exists (∀) some x in the lattice that when rotated is still in the lattice.

Eigenvalues are a special set of scalars associated with a linear system of equations (i.e., a matrix equation), sometimes also known as characteristic roots, characteristic values, proper values, or latent roots. To clarify, consider a column vector we will call v. Then also consider an n × n matrix we will call A. Then consider some scalar, λ. If it is true that

Av = λv

then we say that v is an eigenvector of the matrix A and that λ is an eigenvalue of the matrix A.

Let us look a bit closer at this. The prefix eigen is actually a German word that can be translated as “specific,” “proper,” “particular,” etc. Put in its most basic form, an eigenvector of some linear transformation, T, is a vector that when T is applied to it does not change direction; it only changes scale. It changes scale by the scalar value λ, the eigenvalue. Now we can revisit the former equation just a bit and expand our knowledge of linear algebra:

T(v) = λv

This appears precisely like the former equation, but with one small difference: the matrix A is now replaced with the transformation T. Not only does this tell us about eigenvectors and eigenvalues, it tells us a bit more about matrices. A matrix, when applied to a vector, transforms that vector. The