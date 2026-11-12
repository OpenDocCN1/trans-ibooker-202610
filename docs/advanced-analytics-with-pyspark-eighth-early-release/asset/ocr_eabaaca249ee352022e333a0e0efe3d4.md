CSV files contain fundamentally tabular data, organized into rows of columns. Sometimes these columns are given names in a header line, although that's not the case here. The column names are given in the companion file, covtype.info. Conceptually, each column of a CSV file has a type as well—a number, a string —but a CSV file doesn't specify this.

It's natural to parse this data as a data frame because this is PySpark's abstraction for tabular data, with a defined column schema, including column names and types. PySpark has built-in support for reading CSV data. Let us read our dataset as a DataFrame using the in-built CSV reader:

```
data_without_header = spark.read.option("inferSchema",
True).option("header", False).csv("data/covtype.data")
data_without_header.printSchema()
...
root
|-- _c0: integer (nullable = true)
|-- _c1: integer (nullable = true)
|-- _c2: integer (nullable = true)
|-- _c3: integer (nullable = true)
|-- _c4: integer (nullable = true)
|-- _c5: integer (nullable = true)
...
```

This code reads the input as CSV and does not attempt to parse the first line as a header of column names. It also requests that the type of each column be inferred by examining the data. It correctly infers that all of the columns are numbers, and more specifically, integers. Unfortunately it can only name the columns “_c0” and so on.

We can look at the covtype.info file for the column names.

```
$ cat data/covtype.info
```

...
[...]
7. Attribute information:
Given is the attribute name, attribute type, the measurement unit
and
a brief description. The forest cover type is the classification
problem. The order of this listing corresponds to the order of
numerals along the rows of the database.