of algorithm analysis. Before delving into quantum algorithms, it is important to deepen your understanding of algorithms (or for some readers, review).

Chapter 4 provided a colloquial definition of an algorithm as essentially a recipe. Although not inaccurate, it’s simply too informal. In mathematics, an algorithm is a finite sequence of well-defined instructions to solve a problem or class of problems. To quote from one of my favorite algorithm textbooks, “Informally, an algorithm is any well-defined computational procedure that takes some value, or set of values, as input and produces some value, or set of values, as output. An algorithm is thus a sequence of computational steps that transform the input into the output.”1

Algorithms must be unambiguous. What is meant by this is that the steps are clear and very well-defined, and the output is predictable based on the inputs. This comports with the recipe analogy of Chapter 4. An algorithm provides a clear, step-by-step process for accomplishing some task. The specific steps of an algorithm must be a well-defined and finite sequence of steps.

Now that you have a clearer understanding of what an algorithm is, let us address a few questions regarding algorithms. Whether or not the algorithm is correct is actually a remarkably simple question. If, for every input, the algorithm ceases when the correct output is consistently found, then the algorithm is correct. In fact, the term consistently might be a bit too anemic. An algorithm should be reliable and always produce a correct output. Consistently does not necessarily imply “every time,” and an algorithm should function every time.

Obviously, our interest is in computer algorithms, and as we progress through this section we will examine a few algorithms—more specifically, quantum algorithms. However, it must be noted that an algorithm can be expressed in many forms, including using words from your preferred language. The aforementioned recipe is an algorithm. A specific set of steps to accomplish any task is an algorithm.

The Euclidean algorithm is a classic algorithm that illustrates the essence of algorithms. For mathematics and computer science students, this will be quite familiar. The Euclidean algorithm is a method for discovering the greatest common divisor of two integers. This might seem like a trivial task, but as integers get larger, the problem quickly becomes substantial. The Euclidean algorithm proceeds in a series of well-defined steps such that the output of each step is used as the input for the next one. Let n be an integer that counts the steps of the algorithm, starting with zero. Thus, the initial step corresponds to n = 0, the next step corresponds to n = 1, etc.

Each step, after the first, begins with two remainders, r_{n-1} and r_{n-2}, from the preceding step. You will notice that at each step the remainder is smaller than the remainder from the preceding step, such that r_{n-1} is less than its predecessor r_{n-2}. This is deliberate and central to the functioning of the algorithm. The goal of the nth step is to find a quotient qn and remainder r_n such that the equation

```
r_{n-2} = q_n r_{n-1} + r_n
```

1. Cormen, Thomas H.; Leiserson, Charles E.; Rivest, Ronald L.; Stein, Clifford. Introduction to Algorithms, Third Edition (p. 27). MIT Press, 2009.