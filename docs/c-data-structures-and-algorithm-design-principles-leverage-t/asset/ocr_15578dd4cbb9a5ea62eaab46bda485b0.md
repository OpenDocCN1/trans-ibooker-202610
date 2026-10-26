```
return DP[capacity];
```

+   6. Now, let's return to **main()**, add a call to **UnboundedKnapsack()**, and output the results:

```
int main()
{
    ...

    int result = UnboundedKnapsack(items, capacity, values, weight);

    cout << "Maximum value of items that can be contained in the knapsack: " << result << endl;

    return 0;
}
```

+   7. Try running your program with the following input:

```
30 335
91 81 86 64 24 61 13 57 60 25 94 54 39 62 5 34 95 12 53 33 53 3 42 75 56 1
84 38 46 62
40 13 4 17 16 35 5 33 35 16 25 29 6 28 12 37 26 27 32 27 7 24 5 28 39 15
38 37 15 40
```

Your output should be as follows:

```
Maximum value of items that can be contained in the knapsack: 7138
```

As demonstrated by the preceding implementation, it is often worth it to consider less costly ways to cache solutions in a DP algorithm. Problems that seem to require complex state representations can often be simplified significantly after closer examination.