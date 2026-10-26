The binary search takes approximately equal time in all three cases, even with vastly different vector sizes!

Note that our implementation of binary search uses iterators and the C++ Standard Library functions such as std::distance() and std::advance(). This is considered good practice in modern C++ since it helps keep our code agnostic of the underlying data type and safe from index out-of-bounds errors.

Now, say we wanted to perform a search on a vector of floating-point numbers. How would we modify our functions in the previous exercise? The answer is exceedingly simple. We can modify the function signatures as follows:

```
bool linear_search(float N, std::vector<float>& S)
```

```
bool binary_search(float N, std::vector<float>& S)
```

The rest of the code inside of the search functions can still remain exactly the same since it is completely independent of the underlying datatype and depends only on the behavior of a container datatype. This separation of core algorithm logic from the underlying datatype on which the algorithm operates is a cornerstone of writing reusable code in modern C++. We shall see several examples of such separation in the duration of this book and dive into more functions that the Standard Library provides that can help us write reusable and robust code.

# Activity 8: Vaccinations

Imagine that it is flu season and health department officials are planning to visit a school to ensure that all the enrolled children are administered their flu shot. However, there is a problem: a few children have already taken their flu shots but do not remember if they have been vaccinated against the specific category of flu that the health officials plan to vaccinate all the students against. Official records are sought out and the department is able to find a list of students that have already been administered the vaccine. A small excerpt of the list is shown here:

| First name | Last name | Flu shot administered? |
|---|---|---|
| 1 | 3 | Yes |
| 2 | 2 | No |
| 2 | 3 | Yes |

Figure 4.7: Excerpt of vaccination records