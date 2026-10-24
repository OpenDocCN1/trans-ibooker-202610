To delete everything we just need to reset the List to its initial state:

```
layout/calculator/lib/calculator.dart
```

```
void deleteAll() => a = [];
```

But deleting one character at a time is more complicated, as we'll need to check multiple things:

+   1. Check if the List is empty, in that case we'll do nothing.
+   2. Check if the last element of the List is longer than one character, in that case we'll delete the last character of that string.
+   3. If the last element is just one character, we'll delete it completely.

Number 2 is a bit complicated: to delete the last character we actually need to assign to the last element a substring of itself with the last character deleted.

To create substrings, we are going to use the String.substring() method, which is used in the following way:

```
var substr = string.substring(startIndex, length);
```

where startIndex is an integer representing the index at which the substring starts (relative to the string from which the substring is created) and length is the length of the substring.

In this case, we need it to start from 0 and to have a length that is equal to the length of the original string with 1 subtracted to it.

Combining all of that, the code for the deleteOne method is as follows:

```
layout/calculator/lib/calculator.dart
```

```
void deleteOne() {
  if(a.length > 0) {
    if(a.last.length > 1) {
      a.last = a.last.substring(0, a.last.length-1);
    } else {
      a.removeLast();
    }
  }
}
```