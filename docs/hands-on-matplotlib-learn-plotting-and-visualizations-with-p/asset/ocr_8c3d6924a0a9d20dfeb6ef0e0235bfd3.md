You can also skip rows and columns as follows:

```
data = np.loadtxt('data.csv', delimiter=',',
                  skiprows=3, usecols=[1, 3])
print(data)
```

The output is as follows:

```
[[978.   6.]
 [  2.  45.]
 [ 67.   3.]
 [  5.   2.]]
```

# Matplotlib CBook

You can read data that is stored in Matplotlib’s CBook format. Matplotlib comes with a few sample files in that format. Let’s see how to read the data:

```
import matplotlib.cbook as cbook
datafile = cbook.get_sample_data('aapl.npz')
r = np.load(datafile)
print(r.files)
```

This will print the names of the data files, as shown here:

```
['price_data']
```

Let’s read the data from that data file:

```
print(r['price_data'])
```

This shows the Apple share price data as follows:

```
[('1984-09-07', 26.5, 26.87, 26.25, 26.5, 2981600, 3.02)
 ('1984-09-10', 26.5, 26.62, 25.87, 26.37, 2346400, 3.01)
 ('1984-09-11', 26.62, 27.37, 26.62, 26.87, 5444000, 3.07) ...
 ('2008-10-10', 85.7, 100., 85., 96.8, 79260700, 96.8)
 ('2008-10-13', 104.55, 110.53, 101.02, 110.26, 54967000, 110.26)
 ('2008-10-14', 116.26, 116.4, 103.14, 104.08, 70749800, 104.08)]
```