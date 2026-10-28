```
return true;
}
```

This code runs in O(1) time. Alternatively, we can condense these two conditions into a single one, as follows:

```
public static boolean overlap(Point r1lt, Point r1rb,
    Point r2lt, Point r2rb) {

    return (r1lt.x <= r2rb.x && r1rb.x >= r2lt.x
        && r1lt.y >= r2rb.y && r1rb.y <= r2lt.y);
}
```

The complete applications is called *RectangleOverlap*. Note that the interviewer may define *overlapping* in different ways. Based on this problem, you should be able to adapt the code accordingly.

# Coding challenge 9 - Multiplying large numbers

**Amazon, Microsoft**

**Problem:** Consider you've been given two positive large numbers as strings, *a* and *b*. These numbers don't fit in `int` or `long` domains. Write a snippet of code that computes *a*b*.

**Solution:** Let's consider that *a*=4145775 and *b*=771467. Then, *a*b*=3198328601925. Solving this problem relies on mathematics. The following image depicts the *a*b* solution that can be applied on paper and coded as well:

![](bbox=[0.27, 0.65, 0.75, 0.85])

Figure 15.11 – Multiplying two large numbers