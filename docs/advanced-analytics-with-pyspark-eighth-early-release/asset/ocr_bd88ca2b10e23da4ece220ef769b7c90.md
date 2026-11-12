recommendation. The principle is the same here, although the evaluation metric will be different: accuracy. The majority—90%—of the data will again be used for training, and later, we'll see that a subset of this training set will be held out for cross-validation (the CV set). The other 10% held out here is actually a third subset, a proper test set.

```
(train_data, test_data) = data.randomSplit([0.9, 0.1])
train_data.cache()
test_data.cache()
```

The data needs a little more preparation to be used with a classifier in MLlib. The input DataFrame contains many columns, each holding one feature that could be used to predict the target column. MLlib requires all of the inputs to be collected into one column, whose value is a vector. This class is an abstraction for vectors in the linear algebra sense, and contains only numbers. For most intents and purposes, they work like a simple array of double values (floating-point numbers). Of course, some of the input features are conceptually categorical, even if they're all represented with numbers in the input.

Fortunately, the VectorAssembler class can do this work:

```
from pyspark.ml.feature import VectorAssembler

input_cols = colnames[:-1] 🔒
vector_assembler =
VectorAssembler(inputCols=input_cols, outputCol="featureVector")

assembled_train_data = vector_assembler.transform(train_data)

assembled_train_data.select("featureVector").show(truncate = False)
...
+------------------------------+
...
| featureVector                 |
...
+------------------------------+
...
| (54, [0, 1, 2, 5, 6, 7, 8, 9, 13, 18], [1874.0, 18.0, 14.0, 90.0, 208.0, 209.0, ...|
| (54, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 13, 18], [1879.0, 28.0, 19.0, 30.0, 12.0, 95.0, |
...
...
```