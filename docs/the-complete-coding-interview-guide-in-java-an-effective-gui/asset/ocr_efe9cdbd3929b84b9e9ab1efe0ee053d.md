# Example 6 – different steps are summed or multiplied

Consider the following two snippets of code (a and b are arrays). How do you express Big O for each of these snippets?

```
```java
// snippet 1
for (int i=0;i<a.length;i++){
    System.out.println(a[i]);
}

for (int j=0;j<b.length;j++){
    System.out.println(b[j]);
}
```
```

```
```java
// snippet 2
for (int i=0;i<a.length;i++){
    for (int j=0;j<b.length;j++){
        System.out.println(a[i]+b[j]);
    }
}
```
```

7.6 – Code snippet a and b

We already know from the previous example that, in the case of the first snippet, Big O is O(a + b). We sum up the runtimes since their work is not interweaved as in the case of the second snippet. So, in the second snippet, we cannot sum up the runtimes since, for each case of a[i], the code loops the b array, and so Big O is O(a * b).

Think twice before deciding between summing and multiplying the runtimes. This is a common mistake made in interviews. Also, it is quite common to not notice that there is more than one input (here, there are two) and to mistakenly express Big O using a single variable. That would be wrong! Always pay attention to how many inputs are present. For each input that affects the runtime rate of increase, you should have a separate variable (see Example 5).

> **Important note**
> 
> As a rule of thumb, different steps can be summed or multiplied. The runtimes should be summed or multiplied based on the following two statements:
> 
> If you describe your algorithm as *it foos and when it's done, it buzzes*, then sum the runtimes.
> 
> If you describe your algorithm as *for each time it foos, it buzzes*, then multiply the runtimes.

Now, let's discuss *log n* runtimes.