```
stocks.printSchema()
...
root
 |-- Date: date (nullable = true)
 |-- Open: string (nullable = true)
 |-- High: string (nullable = true)
 |-- Low: string (nullable = true)
 |-- Close: double (nullable = true)
 |-- Volume: string (nullable = true)
 |-- Symbol: string (nullable = true)
 |-- count: long (nullable = false)
```

Let's trim the time periods of instruments to align.

```
from datetime import datetime

stocks = stocks.filter(fun.col('Date') >= datetime(2009, 10,
23)).\
    filter(fun.col('Date') <= datetime(2014, 10, 23))
```

We will do convert the Date column's type and trim the timeperiod in our factors DataFrame too.

```
factors = factors.withColumn('Date',
fun.to_date(fun.to_timestamp(fun.col('Date'), 'dd-MM-yy')))

factors = factors.filter(fun.col('Date') >= datetime(2009, 10,
23)).\
    filter(fun.col('Date') <= datetime(2014, 10,
23))
```

The histories of a few thousand instruments and 3 factors are small enough to read and process locally. This remains the case even for larger simulations with hundreds of thousands of instruments and thousands of factors. Even though we have used PySpark for preprocessing out data so far, the need arises for a distributed system such as PySpark when we're actually running the simulations, which can require massive amounts of computation on each instrument. We can convert our PySpark DataFrame into a Pandas dataframe and still continue working with it easily by performing in-memory operations.