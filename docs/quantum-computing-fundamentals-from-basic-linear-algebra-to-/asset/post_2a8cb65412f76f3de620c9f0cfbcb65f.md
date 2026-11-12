
The first row gives us this:

[0 1] (1/1) = (0 * 1) + (1 * 1)
= 1

Now the second row:

[1 0] (1/1) = (1 * 1) + (0 * 1)
= 1

That gives us (1/1).
Now the right side:

1 (1/1)

It should be obvious this equals (1/1) and we have found a second eigenvector of this Pauli matrix. If this is not completely clear to you, it is suggested that you continue and calculate the eigenvalues and eigenvectors for the other Pauli matrices. You will often see this in another form, one which will be introduced in Chapter 3, “Basic Physics for Quantum Computing.”

### 2.6 Transcendental Numbers

Not directly part of the issue of complex numbers, but related, is the topic of transcendental numbers. Transcendental numbers are numbers other than algebraic numbers. This, of course, begs the question, what is an algebraic number? An algebraic number is a root of a nonzero polynomial. Algebraic numbers include both real numbers and complex numbers.

Two examples of transcendental numbers are π and e; however, there are actually infinitely many transcendentals. One fact regarding real transcendental numbers is that they are irrational; however, the reverse is not true. Certainly not all irrational numbers are transcendental. A good example is √2, because that is actually a root of the polynomial equation x^2 − 2 = 0.

Euler’s number is not only one of the more common examples of a transcendental number, but Leonhard Euler was one of the first to provide a definition of transcendental numbers. He did so in the eighteenth century. It was Joseph Liouville in 1844 who first provided the existence of transcendental numbers. In 1873 Charles Hermite proved that (e) was indeed a transcendental number. The famous mathematician David Hilbert proposed a number of problems. Hilbert’s seventh problem is stated as follows:

If a is an algebraic number that is not zero or one, and b is an irrational algebraic number, is a^b necessarily transcendental?

In case you thought transcendental numbers are simply mathematical curiosities, without practical purpose, let us examine one that clearly has practical applications. Chaitin’s constant, sometimes called Chaitin’s construction, is essentially the probability that a randomly constructed program will halt. There are actually an infinite number of halting probabilities, given the infinite possibilities for encoding a program. Oftentimes this is simply represented by the single symbol, Ω. Gregory Chaitin described this eponymously named constant. Each halting probability is a transcendental real number. The details of Chaitin’s constant are beyond our current scope, but the interested reader is encouraged to delve deeper into the topic.

### 2.7 Summary

This chapter began with the basics of complex numbers, including how complex numbers originated. You learned how to do basic algebraic functions with complex numbers. Then working with graphical representations of complex numbers was covered. The next topic was vector representation. Both graphical and vector representations are very common in quantum physics and quantum computing, so it is quite important that you understand these concepts.

A section of this chapter was devoted to Pauli matrices. This topic is central to understanding quantum computing. You must absolutely ensure you are comfortable with the material on Pauli matrices before moving on. There was also a brief discussion of transcendental numbers.

### Test Your Skills

### REVIEW QUESTIONS

- 1. Add the following two complex numbers: (3 + 2i)(2 + 3i)
- 2. What is the product of (4 + 2i)(2 + 2i)?
- 3. What is the product of (2 + 2i)(-2 + 3i)?
- 4. Divide (6 + 4i) by (2 + 2i).
- 5. Given the two complex numbers (3 + 2i) and (4 + 4i), what is the distance between them?
- 6. Which of the following statements accurately describes the Kronecker delta?
  - a. It returns a 1 if the two inputs are equal, and a zero otherwise.
  - b. It returns a 0 if the two inputs are equal, and a 1 otherwise.
  - c. It calculates the distance between two complex numbers.
  - d. It calculates the distance between two points on a graph.
- 7. Calculate the determinant of the following Pauli matrix: \(\sigma 2 = \left[ \begin{array}{cc} 0 & -i \\ i & 0 \end{array} \right]\)
- 8. What is the primary difference between a dot product and an inner product from a practice viewpoint?
- 9. What is an involutory matrix?
  - a. One that has a complex inverse
  - b. One that has a determinant of –1
  - c. One that is its own inverse
  - d. One that has no inverse
- 10. What are the eigenvalues of the following Pauli matrix?
  ```
  [\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}]
  ```

## Chapter 3

## Basic Physics for Quantum Computing

### Chapter Objectives

After reading this chapter and completing the quiz, you will be able to do the following:

- Have a basic understanding of quantum physics
- Be able to explain quantum states
- Comprehend the uncertainty principle
- Describe quantum entanglement

Our ultimate goal in this book is to explore quantum computing; however, quantum computing is (quite obviously) based on quantum physics. In order to understand quantum computing, you must have some generalized idea of quantum physics. This chapter begins with a general overview of physics and then discusses the discoveries that led to quantum physics. This will provide us with a foundation to discuss the fundamentals of quantum physics. Obviously, one can fill several books and several courses on the topic of quantum physics. The goal of this chapter is to provide you with just enough working knowledge to proceed with the rest of the book.

It must also be noted that many concepts in this chapter can be quite difficult for the novice. Do not be surprised if you struggle with some concepts, or even need to re-read sections. This is quite normal for those who are new to quantum physics. For those who already have a background in physics, understand the goal of this chapter is to provide the novice just enough knowledge to pursue quantum computing at a working level. Clearly, there is much more that could be written on quantum physics than appears in this chapter, and the topics in this chapter could all be given more detail. However, the goal of this chapter is not to be a textbook on quantum physics. It is to provide just enough knowledge for the reader to proceed with concepts presented later in this book.

### 3.1 The Journey to Quantum

To best understand quantum physics, it is advantageous to learn the history of what led to quantum physics. There were a number of important discoveries in the latter days of the nineteenth century and the beginning of the twentieth century. Each of these discoveries added to a body of evidence that would ultimately require the revision of classical physics.

The real issue in quantum physics, however, begins with understanding the nature of light. Is light a wave or a particle? The debate goes back to the 1600s. Isaac Newton had proposed that light was a particle or corpuscle. Christian Huygens had advocated that light was a wave, much like a sound wave or water wave. Huygens was not alone in his view of light as a wave, but the prominence of Isaac Newton certainly gave weight to the corpuscular view of light. This began what would become the birth of quantum mechanics. Classical physics could not explain several things, including the nature of light.

These two competing concepts would continue to fuel the debate in physics for quite some time, until experiments could be conducted to help settle the matter. Waves generally propagate through some medium. Sound waves, as an example, propagate through the air. Proponents of the wave view of light proposed an ether that light propagated through. In fact, it was believed that all electromagnetic radiation propagated through this ether (also spelled *aether*). The concept of aether would hold sway in physics until the very early twentieth century. This aether was thought to be a space-filling substance or medium that was the transmission medium for not just light, but gravity and other forces. In fact, physicists attempted to understand planetary motion in the context of aether.

In 1801, Thomas Young performed an experiment that demonstrated the wave behavior of light. Young shined a light through two slits in a barrier and had a photosensitive plate on the other side. When the light went through two apertures, it appeared to have interference on the receiving side, much like a water wave or sound wave. Figure 3.1 demonstrates these results.

![](img/49061be7fe11d9f7f3e56114ab15acdc_79_0.png)

FIGURE 3.1 Double-slit experiment

Light is shown through the single slit, then on to the double slit. When the target is reached, the two light sources (one from each of the slits in the double slit) exhibit an interference pattern. The target
