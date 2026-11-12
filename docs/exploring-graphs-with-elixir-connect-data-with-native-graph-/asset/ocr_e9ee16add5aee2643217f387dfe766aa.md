# # Finding Elements

The now-familiar elegant map() methods to traverse and transform collections won't directly help pick elements from a collection. The filter method is designed for that purpose.

From a list of names, let's pick the ones that start with the letter N. Since there may be zero matching names in the list, the result may be an empty list. Let's first code it using the old approach.

```
collections/fpij/PickElements.java

final List<String> startsWithN = new ArrayList<>();
for(String name : friends) {
    if(name.startsWith("N")) {
        startsWithN.add(name);
    }
}
```

That's a chatty piece of code for a simple task. We created a variable and initialized it to an empty collection. Then we looped through the collection, looking for a name that starts with the desired letter. If found, we added the element to the collection.

Let's refactor this code to use the filter method, and see how it changes things.

```
collections/fpij/PickElements.java

final List<String> startsWithN =
friends.stream()
    .filter(name -> name.startsWith("N"))
    .collect(Collectors.toList());
```