You can complete this book and understand it with only a general understanding of Hilbert spaces; however, if you intend to go further with quantum physics or quantum computing, you will need to have a stronger understanding of Hilbert spaces. A Hilbert space is a generalization of Euclidean space. This, of course, necessitates some explanation of Euclidean space.

Euclidean space is the space of classical geometry. It was initially three-dimensional space. This is the space you probably studied in primary or secondary school. The Cartesian coordinates, invented by Rene Descartes in the seventeenth century, are used in Euclidean space. For quite a long time, Euclidean space was limited to just three dimensions, and that comports well with our daily experiences.

In the nineteenth century, Ludwig Schlafi extended Euclidean space to include _n_ dimensions. In conjunction with this, Schlafi did a lot of work with polytopes, which are analogues of the traditional Platonic solids with more dimensions.

Euclidean space is a space over the real number system. A Euclidean vector space is a finite- dimensional inner product space over the real numbers. Recall from Chapter 2 that an inner product is a dot product with the properties of being symmetric: its first argument is linear and is greater than zero. Also recall from Chapter 1 that the dot product of two vectors is simply the two vectors multiplied.

A Hilbert space is any dimension; it can be finite or infinite. A Hilbert space also can be real or complex (recall complex numbers from Chapter 2). Every inner product gives rise to a norm. Remember from Chapter 1 that the nonnegative length is called the norm of the vector.

If the space is complete with respect to that norm, then it is a Hilbert space. This part might be a bit difficult for readers without a rigorous mathematical background, so allow me to devote a bit of space to explain this in some detail (feel free to skip this explanation if this is all old news to you).

In this case, the term _completeness_ simply means that every Cauchy sequence of elements in that space converges to an element in this space. This, at least for some readers, begs the question as to what a Cauchy sequence is and why it is so important. Cauchy sequences are named after Augustin-Louis Cauchy. Incidentally, Cauchy is pronounced _koh-shee_. A Cauchy sequence is a sequence whose elements become arbitrarily close to each other as the sequence progresses. There are actually quite a few rather simple examples of this. Here is an example of a Cauchy sequence:

```
a_n = \frac{1}{2^N}
```

Why is this a Cauchy sequence? Well, let us examine it a bit closer. First, select any i > 0. Now choose an N, such that 2^-N < i. Next, consider n and m that are > than N. Then

```
|a_n - a_m| = \left| \frac{1}{2^n} - \frac{1}{2^m} \right| \le \frac{1}{2^n} + \frac{1}{2^m} \le \frac{1}{2^N} + \frac{1}{2^N} = i
```

This might still not be completely clear to some readers. Consider a visual representation of a Cauchy sequence, as shown in Figure 3.8.