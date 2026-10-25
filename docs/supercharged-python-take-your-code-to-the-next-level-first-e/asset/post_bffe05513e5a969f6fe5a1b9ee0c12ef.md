

numbers—specifically, the **numpy.random** function, **rand**,  
which takes an array shape as input and generates an array of  
that shape. Each element is a random floating-point number  
between 0.0 and 1.0.

```
import numpy as np  
import numpy.random as ran  
A = ran.rand(10)
```

Printing A produces an array full of random floating-point  
values.

> Click here to view code image

```
[0.49353738 0.88551261 0.69064065 0.93626092  
0.17818198 0.16637079 0.55144244 0.16262533  
0.36946706 0.61859074]
```

The **numpy** package can handle much bigger data sets and  
handle them efficiently, such as the following large array. But  
note: Don’t print this unless you want Python to be busy for a  
long, long time!

```
A = ran.rand(100000)
```

This statement creates an array of 100,000 elements, each of  
which is a random floating-point value, and it does it in a  
fraction of a second. Even more astonishing is the speed with  
which **numpy** statistical functions process this array. The  
following IDLE session demonstrates how quickly you can get  
stats on this large data set.

```
>>> import numpy as np
>>> import numpy.random as ran
>>> A = ran.random(100000)
>>> np.mean(A)
0.49940282901121
>>> np.sum(A)
49940.282901121005
>>> np.median(A)
0.5005147698475437
>>> np.std(A)
0.2889516828729947
```

If you try this session yourself, you should experience the response times as instantaneous, even the standard deviation.  
Most of these stats are straightforward in meaning. Because the probability distribution in this case is a uniform distribution from 0.0 to 1.0, you’d reasonably expect the mean to be close to 0.5, which it is: approximately 0.4994. The sum is exactly 100,000 times that, or about 49,940. The median is not the same as the mean, although you’d expect it to be close to the center of values, which it is: just over 0.50.

The standard deviation is what statisticians would predict for a uniform distribution like this: just under 0.29. So roughly 60 percent of the values fall within one standard deviation (plus or minus) of the mean.

Using **numpy** saves you from having to do this calculation yourself, but it’s useful to review how standard deviation is calculated and what it means. Assume A and A2 represent arrays, and `i` refers to elements:

A2 = (i − mean(A)) ^ 2, for all i in A.  
std(A) = sqrt(mean(A2))

In plainer language, these equations mean the following:

- Figure out the average of the elements in array A. This is also called the mean.
- Subtract the mean from each element in A to create a new array full of “deviations.”
- In this array of deviations, square each member, and call the resulting array A2.
- Find the average of all elements in A2, take the square root of the result, and voila! You have produced the standard deviation of the array you started with.

Although **numpy** provides the standard-deviation function for free, it’s useful to see what it would take to produce the result through the standard batch operations. First, getting A2 would be easy enough: Subtracting the mean of A (a scalar) from A itself gives us an array filled with deviations. All these are then squared.

A2 = (A - mean(A)) ** 2

Having obtained this new array, we need only get the square root of the mean of the deviations.

result = (mean(A2)) ** 0.5

Or, in terms of Python code, it requires the **np** qualifier to call the **mean** function:

```
>>> A2 = (A - np.mean(A)) ** 2
>>> result = (np.mean(A2)) ** 0.5
>>> result
0.2889516828729947
>>> np.std(A)
0.2889516828729947
```

The results, as you can see, are precisely the same in both cases—calculating standard deviation “the hard way” and getting it from `np.std`—which is good evidence that the **numpy** routines are following the same algorithm.

It’s instructive to run the standard deviation function on an even larger array—say, an array of 1 million random numbers—and the equivalent code in Python, using standard lists.

Now comes the interesting part: If you benchmark this technique with a list of 1 million elements in size, against a **numpy** version with an array containing the same data, the **numpy** version—getting the standard deviation directly—beats out the non-**numpy** version by a factor of more than 100 to 1!

Here’s the full benchmark code:

```
import numpy as np
import time
import numpy.random as ran

def get_std1(ls):
    t1 = time.time()
    m = sum(ls)/len(ls)
    ls2 = [(i - m) ** 2 for i in ls]
    sd = (sum(ls2)/len(ls2)) ** .5
    t2 = time.time()
    print('Python took', t2-t1)

def get_std2(A):
    t1 = time.time()
    A2 = (A - np.mean(A)) ** 2
    result = (np.mean(A2)) **.5
    t2 = time.time()
    print('Numpy took', t2-t1)

def get_std3(A):
    t1 = time.time()
    result = np.std(A)
    t2 = time.time()
    print('np.std took', t2-t1)

A = ran.rand(1000000)
get_std1(A)
get_std2(A)
get_std3(A)
```

Running all three gets the following results, expressed in parts of a second. Remember this is the time taken to get standard deviation _for 1 million elements_.

```
Python took 0.6885709762573242
Numpy took 0.0189220905303955
np.std took 0.0059509277343750
```

You can see how enormous the gains in performance are, as we go from Python lists, to **numpy** arrays, to finally using **numpy** to get standard deviation directly, with a single function call.

The increase in speed from not using **numpy** at all, compared to using `np.std` (the **numpy** standard deviation function) is well over 100 to 1. Now that's greased lightning!

# 12.13 GETTING DATA ON

# "NUMPY" ROWS AND COLUMNS

Section 12.12, “Getting ‘numpy’ Stats (Standard Deviation),” assumed the simplest possible case: getting statistics on a floating-point array of one dimension. However, all those functions accept other arguments. You can look these up in online documentation or in IDLE, through the help command.

The most important argument, other than the array itself, is the axis argument, which is used with higher dimensions—that is, dimensions greater than one.

Let’s start with another array of random numbers—this time, integers. To produce such an array, you can use the randint function of the numpy.random package. Here’s an example:

```
import numpy as np
import numpy.random as ran
A = ran.randint(1, 20, (3, 4))
print(A)
```

Here’s some sample output. Your results, of course, will vary.

```
[[ 4 13 11  8]
 [ 7 14 16  1]
 [ 4  1  5  9]]
```

The numpy.random package has its own randint function, just as the random package does. This is another reason that using namespace qualifiers is so important. In this case, using the numpy random package, the function takes begin and end arguments, as you’d expect, but it also takes an additional argument: a tuple giving the shape of the array.

Another thing to note is that with `ran.randint`, the _begin_ and _end_ arguments include numbers starting with the _begin_ argument, up to but not including the _end_ argument. So this example produces numbers up to 19.

Finally, the _shape_ argument—which comes after the _begin_ and _end_ arguments—is (3, 3), which causes the random integers to be placed throughout a 3 × 3 array. Here it is again. Your mileage will vary.

```
[[ 4 13 11  8]
 [ 7 14 16  1]
 [ 4  1  5  9]]
```

As you learned in the previous section, the **numpy** statistical functions can be used to study this array as one large source of data. If `np.mean` is applied directly to the array, for example, it gets the mean of all 20 elements.

```
>>> np.mean(A)
7.75
```

Likewise, we can sum the data or get the standard deviation.

```
>>> np.sum(A)
93
>>> np.std(A) # standard deviation
4.780603169754489
```

The fun part comes when we collect statistics along an axis: either the row or the column dimension. Such operations enable the treatment of a **numpy** array as if it were a spreadsheet, containing totals for each row and column. However, it’s easy to get the axis confused. Table 12.7 should help clarify.

| Setting | Description |
|---|---|
| `axis = 0` | Create a row collecting data for each column. Size of the resulting one-dimensional array is the number of columns. |
| `axis = 1` | Create a column collecting data for each row. Size of the resulting one-dimensional array is the number of rows. |

For even higher dimensional arrays, the axis settings can run higher. The axis settings can even be tuples. Although it may be confusing at first, the way to approach the word “axis” is to think of it like a Cartesian coordinate system, as the name suggests. Look at A again.

```
[[ 4 13 11  8]
 [ 7 14 16  1]
 [ 4  1  5  9]]
```

