# Welcome to
version 3.0.1
Using Python version 3.6.12 (default, Sep 8 2020 23:10:56)
SparkSession available as 'spark'.

You can run the :help command in the shell. This will prompt you to either start an interactive help mode or ask for help about specific Python objects. In addition to the note about :help, the Spark log messages indicated “SparkSession available as spark.” This is a reference to the SparkSession, which acts as an entrypoint to all Spark operations and data. Go ahead and type spark at the command line:

```
spark
...
<pyspark.sql.session.SparkSession object at DEADBEEF>
```

The REPL will print the string form of the object. For the SparkSession object, this is simply its name plus the hexadecimal address of the object in memory. (DEADBEEF is a placeholder; the exact value you see here will vary from run to run.) In an interactive Spark shell, the Spark driver instantiates a SparkSession for you, while in a Spark application, you create a SparkSession object yourself.

> In Spark 2.0, the SparkSession became a unified entry-point to all Spark operations and data. Previously used entry points such as SparkContext, SQLContext, HiveContext, SparkConf, and StreamingContext can be accessed through it too.

What exactly do we do with the spark variable? SparkSession is an object, so it has methods associated with it. We can see what those methods are in the PySpark shell by typing the name of a variable, followed by a period, followed by tab:

```
spark.[Tab]
...
```