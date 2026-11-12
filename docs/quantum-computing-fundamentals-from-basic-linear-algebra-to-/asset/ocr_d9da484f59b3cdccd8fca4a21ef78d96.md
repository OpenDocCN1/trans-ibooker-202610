you find daunting, simply review it a few times to ensure you get the general gist of it and move on. You can certainly work with qubits, Q#, and other quantum tools later in this book without a deep understanding of how to do the mathematics.

# ## 6.1 Further with Quantum Mechanics

Chapter 3 introduced some fundamental concepts in quantum physics. This section expands our exploration of quantum mechanics a bit. In 1932, Werner Heisenberg was awarded the Nobel Prize in Physics for the “creation of quantum mechanics.” I am not sure that one person can be solely credited with the creation of quantum mechanics, but certainly Heisenberg deserves that credit as much as anyone.

The publication that earned him the Nobel Prize was titled “Quantum-Theoretical Re-interpretation of Kinematic and Mechanical Relations.” This paper is rather sophisticated mathematically, and we won’t be exploring it in detail here. The paper introduced a number of concepts that formed the basis of quantum physics. The interested reader can consult several resources, including the following:

+   - https://arxiv.org/pdf/quant-ph/0404009.pdf
+   - https://www.heisenberg-gesellschaft.de/3-the-development-of-quantum-mechanics-1925-ndash-1927.html
+   - https://inis.iaea.org/collection/NCLCollectionStore/_Public/08/282/8282072.pdf

# ### 6.1.1 Bra-Ket Notation

Bra-ket notation was introduced a bit earlier, in Chapter 3. However, this notation is so essential to understanding quantum physics and quantum computing that we will revisit it, with more detail. Recall that quantum states are really vectors. These vectors include complex numbers. However, it is often possible to ignore the details of the vector and work with a representation of the vector. This notation is called Dirac notation or bra-ket notation.

A bra is denoted by ⟨V|, and a ket is denoted by |V⟩. Yes, the terms are intentional, meaning braket, or bracket. But what does this actually mean? A bra describes some linear function that maps each vector in V to a number in the complex plane. Bra-ket notation is really about linear operators on complex vector spaces, and it is the standard way that states are represented in quantum physics and quantum computing. One reason for this notation is to avoid confusion. The term vector in linear algebra is a bit different from the term vector in classical physics; In classical physics, a vector denotes something like velocity that has magnitude and direction; however, in quantum physics, a vector (linear algebra vector) is used to represent a quantum state, thus the need for a different notation. It is important to keep in mind that these are really just vectors. Therefore, the linear algebra that you saw in Chapter 1, “Introduction to Essential Linear Algebra,” applies.