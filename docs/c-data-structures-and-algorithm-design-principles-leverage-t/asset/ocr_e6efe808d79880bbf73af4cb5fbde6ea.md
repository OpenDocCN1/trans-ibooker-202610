| STL function | Description of the function |
|---|---|
| `std::accumulate()` | Computes the sum of a given value and all the elements in the given container. It also accepts an external reduction operator. |
| `std::transform()` | Given a container and a function, it applies the function on each element in the container, thus modifying its values. |
| `std::reduce()`<br>(only for C++17 onwards) | Takes in a reduction operator, applies it to the given range, and returns the result. |

Figure 4.20: Some useful STL functions for algorithms

# Dividing and Conquering at a Higher Abstraction Level – MapReduce

So far in this chapter, we have looked at divide and conquer as an algorithm design technique and used it to solve our problems using a predefined set of divide-conquer-merge steps. In this section, we’ll take a slight detour and see how the same principle of dividing a problem into smaller parts and solving each part separately can be particularly helpful when we need to scale software beyond the computational power of a single machine and use clusters of computers to solve problems.

The original **MapReduce** paper starts as follows:

> “MapReduce is a programming model and an associated implementation for processing and generating large datasets. Users specify a map function that processes a key-value pair to generate a set of intermediate key/value pairs, and a reduce function that merges all the intermediate values associated with the same intermediate key.”

> **Note**
> 
> You can refer to the original research paper about the MapReduce model, which was published by Jeffrey Dean and Sanjay Ghemawat in 2004, here: https://static.googleusercontent.com/media/research.google.com/en/us/archive/mapreduce/osdi04.pdf.