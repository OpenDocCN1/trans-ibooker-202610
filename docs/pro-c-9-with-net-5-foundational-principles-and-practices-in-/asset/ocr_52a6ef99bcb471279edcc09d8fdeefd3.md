If you were to execute the program yet again, you would find the following output. Notice that the second time you iterate over the requested sequence, you find an additional member, as you set the first item in the array to be a value less than ten.

```
1 < 10
2 < 10
3 < 10
8 < 10

4 < 10
1 < 10
2 < 10
3 < 10
8 < 10
```

One useful aspect of Visual Studio is that if you set a breakpoint before the evaluation of a LINQ query, you can view the contents during a debugging session. Simply locate your mouse cursor over the LINQ result set variable (subset in Figure 13-1). When you do, you will be given the option of evaluating the query at that time by expanding the Results View option.

![](bbox=[0.07, 0.435, 0.895, 0.675])

Figure 13-1 Debugging LINQ expressions

# The Role of Immediate Execution

When you need to evaluate a LINQ expression resulting in a sequence from outside the confines of foreach logic, you can call any number of extension methods defined by the Enumerable type such as ToArray<T>(), ToDictionary<TSource, TKey>(), and ToList<T>(). These methods will cause a LINQ query to execute at the exact moment you call them to obtain a snapshot of the data. After you have done so, the snapshot of data may be independently manipulated.

Additionally, if you are seeking only a single element, the query is executed immediately. First() returns the first member of the sequence (and should always be used with an orderby). FirstOrDefault() returns the default value for the type of item in the list if there aren’t any to return, such as when the original sequence is empty or the where clause filters out all elements. Single() also returns the first member of the