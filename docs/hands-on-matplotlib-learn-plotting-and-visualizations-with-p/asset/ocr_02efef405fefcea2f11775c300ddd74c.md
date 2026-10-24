You can save it to a file (in NumPy data format) as follows:

```
np.save('test.npy', x)
```

You can load the data from a file into a variable as follows:

```
data = np.load('test.npy')
print(data)
```

The output is as follows:

```
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71
 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
 96 97 98 99]
```

# Reading the Data from a CSV File with NumPy

The CSV file can be read with NumPy too as follows:

```
import numpy as np
# Reads only numeric data
data = np.loadtxt('data.csv', delimiter=',')
print(data)
```

The output is as follows:

```
[[ 0.   1.  18.   2.]
 [ 1.   6.   1.   3.]
 [ 2.   3. 154.   0.]
 [ 4. 978.   3.   6.]
 [ 5.   2.  41.  45.]
 [ 6.  67.   2.   3.]
 [ 7.   5.  67.   2.]]
```