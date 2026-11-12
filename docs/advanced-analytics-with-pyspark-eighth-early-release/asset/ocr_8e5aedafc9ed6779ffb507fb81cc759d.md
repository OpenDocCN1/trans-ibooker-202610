performed by the worker nodes, as depicted on the right in Figure 2-1.

+   3. Run local computations that operate on the results computed in a distributed fashion. These can help you decide what transformations and actions to undertake next.

It's important to remember that all of PySpark's higher-level abstractions still rely on the same philosophy that has been present in Spark since the very beginning: the interplay between storage and execution. Understanding these principles will help you make better use of Spark for data analysis.

Next, we will install and set up PySpark on our machine so that we can start performing data analysis. This is a one-time exercise that will help us run the code examples from this and following chapters.

# # Installing PySpark

The examples and code in this book assume you have Spark 3.1.1 available. For the purpose of following the code examples, install PySpark from the PyPi repository.

```
$ pip3 install pyspark
```

If you want SQL, ML, and/or MLlib as extra dependencies, that’s an option too. We will need these ahead.

```
$ pip3 install pyspark[sql,ml,mllib]
```

> Installing from PyPi skips the libraries required to run Scala, Java or R. Full releases can be obtained from the Spark project site. Refer to the Spark documentation for instructions on setting up a Spark environment, whether on a cluster or simply on your local machine.

Now we’re ready to launch the pyspark-shell, which is a REPL for the Python language that also has some Spark-specific extensions. This is similar to Python or IPython shell that you may have used. If you’re just running these examples on your personal computer, you can launch a local Spark cluster by specifying local[N], where N is the number of threads to run, or * to match