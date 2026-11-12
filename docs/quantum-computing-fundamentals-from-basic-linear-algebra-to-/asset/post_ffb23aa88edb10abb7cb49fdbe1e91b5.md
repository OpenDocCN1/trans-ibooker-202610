
![](img/49061be7fe11d9f7f3e56114ab15acdc_84_0.png)

### FIGURE 3.3 The s orbital

This is l = 0. Now there can be s subshells or orbitals in any shell. So, there can be an n = 1 s subshell, an n = 2 s subshell, etc.

For l = 1 (which happens if n is at least 2), the orbital begins to look a bit strange. It has a dumbbell-like shape, as illustrated in Figure 3.4.

![](img/49061be7fe11d9f7f3e56114ab15acdc_84_1.png)

### FIGURE 3.4 The p orbital

Put more technically, p orbitals are wave functions with l = 1. Their distribution is angular, but not uniformly so, thus the shape of their distribution. There are three different p orbitals for three different $m_l$ values (–1, 0, and 1). Essentially, these orbitals have different orientations.

If the wave function has l = 2, then it is a d orbital. This has an even more odd distribution. The d orbital is often described as a cloverleaf, but it’s essentially like two dumbbells in a plane for most d orbitals, as illustrated in Figure 3.5. There are five different d orbitals for the $m_l$ values –2, –1, 0, 1, and 2.

![](img/49061be7fe11d9f7f3e56114ab15acdc_85_0.png)

### FIGURE 3.5 The d orbital

The d orbital can also appear as two lobes going through a torus—or put more colloquially, a dumbbell going through a donut. There are variations of this based on the $m_l$ values. Figure 3.6 shows the “donut” shape.

![](img/49061be7fe11d9f7f3e56114ab15acdc_85_1.png)

### FIGURE 3.6 The d orbital alternate form

If the $l$ is 3, then it is an $f$ orbital. The $f$ orbitals take on a range of possible shapes. The shapes depend on the $m_l$ values. There are seven $m_l$ values: -3, -2, -1, 0, 1, 2, and 3. Figure 3.7 shows their respective shapes.

![](img/49061be7fe11d9f7f3e56114ab15acdc_86_0.png)

The spin is represented by +1/2 or –1/2. This brings us to the Pauli exclusion principle. Wolfgang Pauli formulated the eponymously named Pauli exclusion principle in 1925. It states that no two fermions can occupy the same state within a quantum system at the same time. Fermions are those subatomic particles that have 1/2 integer spins. This includes electrons, which are our focus here.

The Pauli exclusion principle has some practical implications for electron distribution. An s suborbital can contain at most two electrons, and then only if they have different spins. So, in helium, there are two electrons, and both are in the s suborbital but have different spins; one is +1/2 and the other –1/2. The p subshells have at most six electrons in pairs, with each member of the pair having an opposite spin. The d subshells have at most ten electrons, again in pairs, with each member of the pair having an opposite spin.

This general overview of the structure of atoms should give you a general working knowledge sufficient to follow the rest of this book. We have just scratched the surface in this section; however, the goal here is not to teach a chemistry course but to ensure that you have a general overview of atomic structure.

## 3.2.2 Hilbert Spaces

Now we can tie in some of the mathematics covered in the first chapter. Think back to vectors, particularly unit vectors. A unit vector has a length of 1. Recall that the length of a vector is computed as follows:

```
||vector|| = √ x2 + y2
```

The possible state of a given quantum system or wave particle is represented by a unit vector. When applied to quantum states, they are called state vectors.

The various state vectors are part of a vector space called the state space. This state space is a complex Hilbert space. Now to describe Hilbert spaces, we will use the information we covered in both Chapter 1, “Introduction to Essential Linear Algebra,” and Chapter 2, “Complex Numbers.”

You can complete this book and understand it with only a general understanding of Hilbert spaces; however, if you intend to go further with quantum physics or quantum computing, you will need to have a stronger understanding of Hilbert spaces. A Hilbert space is a generalization of Euclidean space. This, of course, necessitates some explanation of Euclidean space.

Euclidean space is the space of classical geometry. It was initially three-dimensional space. This is the space you probably studied in primary or secondary school. The Cartesian coordinates, invented by Rene Descartes in the seventeenth century, are used in Euclidean space. For quite a long time, Euclidean space was limited to just three dimensions, and that comports well with our daily experiences.

In the nineteenth century, Ludwig Schläfli extended Euclidean space to include _n_ dimensions. In conjunction with this, Schläfli did a lot of work with polytopes, which are analogues of the traditional Platonic solids with more dimensions.

Euclidean space is a space over the real number system. A Euclidean vector space is a finite-dimensional inner product space over the real numbers. Recall from Chapter 2 that an inner product is a dot product with the properties of being symmetric: its first argument is linear and is greater than zero. Also recall from Chapter 1 that the dot product of two vectors is simply the two vectors multiplied.

A Hilbert space is any dimension; it can be finite or infinite. A Hilbert space also can be real or complex (recall complex numbers from Chapter 2). Every inner product gives rise to a norm. Remember from Chapter 1 that the nonnegative length is called the norm of the vector.

If the space is complete with respect to that norm, then it is a Hilbert space. This part might be a bit difficult for readers without a rigorous mathematical background, so allow me to devote a bit of space to explain this in some detail (feel free to skip this explanation if this is all old news to you).

In this case, the term _completeness_ simply means that every Cauchy sequence of elements in that space converges to an element in this space. This, at least for some readers, begs the question as to what a Cauchy sequence is and why it is so important. Cauchy sequences are named after Augustin-Louis Cauchy. Incidentally, Cauchy is pronounced _koh-shee_. A Cauchy sequence is a sequence whose elements become arbitrarily close to each other as the sequence progresses. There are actually quite a few rather simple examples of this. Here is an example of a Cauchy sequence:

```
a_n = \frac{1}{2^n}
```

Why is this a Cauchy sequence? Well, let us examine it a bit closer. First, select any i > 0. Now choose an N, such that 2^-N < i. Next, consider n and m that are > than N. Then

```
|a_n - a_m| = \left| \frac{1}{2^n} - \frac{1}{2^m} \right| \le \frac{1}{2^n} + \frac{1}{2^m} \le \frac{1}{2^N} + \frac{1}{2^N} = i
```

This might still not be completely clear to some readers. Consider a visual representation of a Cauchy sequence, as shown in Figure 3.8.

![](img/49061be7fe11d9f7f3e56114ab15acdc_88_0.png)

### FIGURE 3.8 Graphical representation of a Cauchy sequence

Basically, as the function progresses, it is progressing to a particular point. For those just trying to get the basic gist of quantum computing, you need not be overly concerned with the details of Cauchy sequences, but this explanation should help you to understand them and how they relate to Hilbert spaces.

## 3.2.3 Uncertainty

Now we come to a very conceptually difficult part of quantum mechanics: the Heisenberg uncertainty principle. Essentially, this principle states that the more you know about one property of a subatomic particle, the less you know about a related property. This principle was introduced in 1927 by Werner Heisenberg. While the principle applies to any property of a protocol, it is most often stated as ‘the more precisely you know the position of a given particle, the less precisely you can know its momentum’. The reverse is also true: the more precisely you know the momentum, the less precisely you can know the position. The practical effect of this is that you can never know with certainty the precise position and momentum of a given particle at a specific point in time.

This concept does not fit well into our understanding of the world. As I sit at my desk, I can take any object in my office and know precisely its position and momentum. You might think that those objects are all stationary, and unless something has gone very wrong, they are. However, I can apply that to moving objects. As I watch my dog walk across the floor, I can quite easily measure precisely his position and momentum at any given instant. This is simply how our everyday world works, but in the subatomic world, things operate in a quite counterintuitive manner. In fact, if you will forgive me for being so informal in my writing, the quantum world is frankly bizarre. Although there are many things we know about it, for some things we “don’t know why.” We just know that countless experiments have verified them.
