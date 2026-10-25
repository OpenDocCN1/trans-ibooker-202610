my_lst, but it does not. To process a list in this way, changing values in place, it's necessary to use indexing.

```
my_lst = [10, 15, 25]
for i in [0, 1, 2]:
    my_lst[i] *= 2
```

This has the intended effect: doubling each individual element of my_lst, so that now the list data is [20, 30, 50] .

To index into a list this way, you need to create a sequence of indexes of the form

```
0, 1, 2, ... N-1
```

in which N is the length of the list. You can automate the production of such sequences of indexes by using the **range** function. For example, to double every element of an array of length 5, use this code:

Click here to view code image

```
my_lst = [100, 102, 50, 25, 72]
for i in range(5):
    my_lst[i] *= 2
```

This code fragment is not optimal because it hard-codes the length of the list, that length being 5, into the code. Here is a better way to write this loop:

Click here to view code image