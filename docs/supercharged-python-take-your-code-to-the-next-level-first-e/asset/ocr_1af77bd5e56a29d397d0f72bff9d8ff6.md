are. Many of these same abilities are realized in functions, such as `len`, `count`, and `index`, which apply to other collection classes as well, including strings and tuples. However, because lists are mutable, there are some list capabilities not supported by those other types, such as `sort` and `reverse`, which alter list data “in place.”

This chapter also introduced some exotic abilities, such as the use of `functools` and lambda functions. It also explained techniques for creating multidimensional lists, an ability that Chapter 12 provides efficient and superior alternatives to; still, it’s useful to know how to create multidimensional lists using the core language.

# # CHAPTER 3 REVIEW QUESTIONS

+   1 Can you write a program, or a function, that uses both positive and negative indexing? Is there any penalty for doing so?

+   2 What’s the most efficient way of creating a Python list that has 1,000 elements to start with? Assume every element should be initialized to the same value.

+   3 How do you use slicing to get every other element of a list, while ignoring the rest? (For example, you want to create a new list that has the first, third, fifth, seventh, and so on element.)

+   4 Describe some of the differences between indexing and slicing.