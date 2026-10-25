This example works by taking advantage of the _start_ argument. After each successful call to the **find** method, the initial searching position, n, is set to the previous successful find index and then is adjusted upward by 1. This guarantees that the next call to the **find** method must look for a new instance of the substring.

If the **find** operation fails to find any occurrences, it returns a value of –1.

The **index** and **rfind** methods are almost identical to the **find** method, with a few differences. The **index** function does not return –1 when it fails to find an occurrence of the substring. Instead it raises a **ValueError** exception.

The **rfind** method searches for the last occurrence of the substring argument. By default, this method starts at the end and searches to the left. However, this does not mean it looks for a reverse of the substring. Instead, it searches for a regular copy of the substring, and it returns the starting index number of the last occurrence—that is, where the last occurrence starts.

Click here to view code image

```
```
frank_str = 'doo be doo be doo...'
print(frank_str.rfind('doo'))  # Prints 14.
```
```

The example prints 14 because the rightmost occurrence of 'doo' starts in zero-based position 14.

Finally, the **replace** method replaces each and every