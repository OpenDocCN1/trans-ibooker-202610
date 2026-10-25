

| Symbol | Meaning |
|--------|---------|
| >      | Little endian |
| >      | Big endian |
| @      | Native to local machine |

For example, to pack two long integers into a string of bytes, specifically using little-endian storage, use the following statement:

Click here to view code image

```
with open('junk.dat', 'wb') as f:
    bstr = struct.pack('<hhl', 1, 2, 100)
    datalen = f.write(bstr)
```

## 8.12 USING THE PICKLING PACKAGE

Exhausted yet? The pickling interface provides a much easier way to read and write data files.

Conceptually, a pickled data file should be thought of a sequence of Python objects, each existing in a kind of “black box,” which is read from or written to by pickling. You can’t go inside these objects as they exist on disk (or at least you can’t do so easily), but you shouldn’t need to. You just read and write them one at a time.

Figure 8.3 provides a conceptual picture of such a data-file arrangement.

![](img/6e45221a8147f2442c653a3d4a866570_520_0.png)

### Figure 8.3. A pickled data file

The beauty of this protocol is that when you read items back into your program, you read them as full-fledged objects. To inquire the type of each object read, you can use the `type` function or simply pass the object to the `print` function.

Pickling is supported by the following two functions:

Click here to view code image

```
import pickle
pickle.dump(value, file_obj)  # Write object to file.
value = pickle.load(file_obj)  # Load object from file.
```

With this approach, all you need to know is that you are reading and writing Python objects, one at a time—although these may include collections, so they can be very large. It’s not even necessary to know what types you’re reading and writing ahead of time, because you can find that out through inspection.

For example, the following block of code writes three Python objects, which in this case happen to be a list, a string, and a floating-point value.

Click here to view code image

```
import pickle
with open('goo.dat', 'wb') as f:
    pickle.dump([1, 2, 3], f)
    pickle.dump('Hello!', f)
    pickle.dump(3.141592, f)
```

The procedure is simple and reliable, assuming the file is meant to be read by another Python application using the pickle package. For example, the following block of code reads these three objects from the file goo.dat and prints out both the string representation and the type of each object.

Click here to view code image

```
with open('goo.dat', 'rb') as f:
    a = pickle.load(f)
    b = pickle.load(f)
    c = pickle.load(f)
    print(type(a), a)
    print(type(b), b)
    print(type(c), c)
```

This example prints

```
<class 'list'> [1, 2, 3]
<class 'str'> Hello!
<class 'float'> 2.3
```

Pickling is easy to use in part because—in contrast to reading simple sequences of bytes—the effect is to load a Python object in all its glory. You can do many things with the object, including taking its type and, if it’s a collection, its length.

> Click here to view code image

```
if type(a)==list:
    print('The length of a is ', a)
```

The only real limitation to pickling is that when you open a file, you may not know how many objects have been written. One solution is to load as many objects as you can until the program raises an EOFError exception. Here’s an example:

> Click here to view code image

```
loaded = []
with open('goo.dat', 'rb') as f:
    while True:
        try:
            item = pickle.load(f)
        except EOFError:
            print('Loaded', len(loaded), 'items.')
            break

        print(type(item), item)
        loaded.append(item)
```

## 8.13 USING THE “SHELVE” PACKAGE

The `shelve` package builds a filewide database on top of the `pickle` interface. The former contains the ability of the latter, so that you don’t have to import both at the same time.

```
import shelve
```

The interface to this package is simple. All you need to do is to open a file through `shelve.open`, which provides a direct entrée into the shelving interface. The object returned can then be used as a virtual dictionary.

Click here to view code image

```
shelf_obj = shelve.open(db_name)
```

You can choose any name you wish for `shelf_obj`, which is only a variable, of course. The `db_name` is the same as the file name, minus its `.db` extension, which is automatically added to the name of the disk file or device.

When this function call is successfully executed, the database file will be created if it does not exist; but in any case it will be opened for both reading and writing operations.

Further operations are then easy. Just treat the object returned (stored in a variable we’re representing by the placeholder `shelf_obj`) as you would any data dictionary (`dict` type). Here’s an example, in which `nums` is being used as the dictionary name in this case:

Click here to view code image

```python
import shelve
nums = shelve.open('numdb')
nums['pi'] = (3.14192, False)
nums['phi'] = (2.1828, False)
nums['perfect'] = (6, True)
nums.close()
```

Notice that the dictionary is referred to through the variable `num` in this case; but unlike other dictionaries, it’s finally closed with a simple call to its `close` method, which empties the buffer and writes out any pending operations to disk.

This dictionary, which now resides on disk, can be reopened at any time. For example, a simple loop prints out all the existing keys.

```python
nums = shelve.open('numdb')
for thing in nums:
    print(thing)
```

Given the data placed into this dictionary earlier, this loop would print the following keys:

```
pi
phi
perfect
```

You can also print individual values, of course.

```
print(nums['pi'])
```

This statement prints the value associated with the key, 'pi'.

```
3.14192
```

Finally, after you open the dictionary with the shelving interface, you must eventually close it, which forces any pending changes to be written out.

```
nums.close()
```

The following special rules apply to the shelving interface:

- Although the data dictionaries that result are **dicts** like any other, the keys must be strings. No other kinds of keys are supported.
- As usual, the associated values can be of any type, but they must be “pickleable.”
- Remember that the **dict** name must be a simple name; the interface will automatically place a file on disk by appending a .db extension. However, do not use this extension yourself in the Python code.

The beauty of this interface is that for very large data sets, it’s potentially far more fast and efficient than ordinarily picking, or almost any other access technique. The shelving interface will not, at least for large data sets, read in the entire dictionary; rather, it will look at an index to determine the location of a value, and then automatically seek to that location.

> Note
> By default, when you use a shelf to access, say, stuff['Brian'], what you get is a copy, and not the original data. So, for example, if my_item is a list, the following does not cause changes to the file:
> d[key].append(my_item)
> However, the following statements do cause changes:
> data = d[key]
> data.append(my_item)
> d[key] = data

# CHAPTER 8 SUMMARY

Python supports flexible, easy techniques for reading and writing to both text files and binary files. A binary file is a file that is not intended to express all data as printable characters but instead is used to store numeric values directly. Binary files have no universally recognized format. Determining a format, and writing data out in that format, is an important issue in working with binary. With Python, several high-level options are available.

The **struct** package enables you to read and write Python values by translating them into fixed-size, regular data fields. The **pickle** package enables you to read and write fully realized Python objects to disk. Finally, the **shelve** interface lets you treat the disk file as one large data dictionary, in which the keys must be strings.

Python also supports interaction with the file systems through the **os** package, which includes the **os.path** subpackage. These packages provide functions for finding and removing files, as well as reading the directory system. From within IDLE, you can use **help(os)** and **help(os.path)** to learn about the capabilities.

# CHAPTER 8 REVIEW QUESTIONS

1. Summarize the differences between text and binary files.
2. What are some situations in which the use of text files would be the best solution? In what situations might you want to use binary files instead?
3. What are some of the problems in reading and writing a Python integer directly to disk using binary operations?
4. Name an advantage of using the **with** keyword instead of opening a file directly.
5. When you read a line of text, does Python include the trailing newline? When you write a line of text, does Python append a newline?
6. What file operations enable random-access operations?

