# Handling CSV Files with Python

Let’s learn a few things about the comma-separated file (CSV) format. CSV files store data in plain-text format, and the data items are either a fixed length or separated by a delimiter such as a comma (,), a pipe (|), or a colon(:). The most common CSV format uses a comma as the delimiter, and many times the first line is used to store the names of the columns.

In this section, you will learn how to handle a CSV file with Python 3. Python 3 comes with a built-in library to handle CSV files. You do not have to install anything. You can import the library as follows:

```
import csv
```

You can open the file as a plain-text file in read mode as follows:

```
file = open('test.csv', 'r')
print(file)
```

Once you open the file, you can pass the file handle to the routine csv.reader() as follows:

```
csvfile = csv.reader(file, delimiter=',')
print(csvfile)
```

This prints the value of the object as follows:

```
<_csv.reader object at 0x0590AC68>
```

You can retrieve the data line by line as follows:

```
for row in csvfile:
    print(row)
```

This produces the following output:

```
['Banana', 'Yellow', '250']
['Orange', 'Orange', '200']
['Grapes', 'Green', '400']
['Tomato', 'Red', '100']
['Spinach', 'Green', '40']
['Potatoes', 'Gray', '400']
```