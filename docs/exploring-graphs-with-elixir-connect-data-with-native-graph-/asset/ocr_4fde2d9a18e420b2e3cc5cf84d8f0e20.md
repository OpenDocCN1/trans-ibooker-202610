# Reducing a Collection to a Single Value

We've gone over a few techniques to manipulate collections so far: picking matching elements, selecting a particular element, and transforming a collection. All these operations have one thing in common: they all worked independently on individual elements in the collection. None required comparing elements against each other or carrying over computations from one element to the next. In this section we look at how to compare elements and carry over a computational state across a collection.

Let's start with some basic operations and build up to something a bit more sophisticated. As the first example, let's read over the values in the friends collection of names and determine the total number of characters.

```
collections/fpij/PickALongest.java

System.out.println("Total number of characters in all names: " +
friends.stream()
.mapToInt(name -> name.length())
.sum());
```

To find the total number of characters, we need the length of each name. We can easily compute that using the mapToInt method. Once we transform the data from the names to their lengths, the final step is to total them. This step we perform using the built-in sum method. Here is the output for this operation:

> Total number of characters in all names: 26

We leveraged the mapToInt method, which is a variation of the map operation (variations like mapToInt, mapToDouble, and so on