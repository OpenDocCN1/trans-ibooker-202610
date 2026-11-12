Of course, this encoding forces the decision tree algorithm to consider the values of the underlying categorical features individually. Because features like soil type are broken down into many features, and because decision trees treat features individually, it is harder to relate information about related soil types.

For example, nine different soil types are actually part of the Leighcan family, and they may be related in ways that the decision tree can exploit. If soil type were encoded as a single categorical feature with 40 soil values, then the tree could express rules like “if the soil type is one of the nine Leighton family types” directly. However, when encoded as 40 features, the tree would have to learn a sequence of nine decisions on soil type to do the same, this expressiveness may lead to better decisions and more efficient trees.

However, having 40 numeric features represent one 40-valued categorical feature increases memory usage and slows things down.

What about undoing the one-hot encoding? This would replace, for example, the four columns encoding wilderness type with one column that encodes the wilderness type as a number between 0 and 3, like “Cover_Type”.

```
def unencode_one_hot(data):
    wilderness_cols = ['Wilderness_Area_' + str(i) for i in range(4)]
    wilderness_assembler = VectorAssembler().setInputCols(wilderness_cols).setOutputCol("wilderness")
    unhot_udf = udf(lambda v: v.toArray().tolist().index(1)) ➊
    with_wilderness = wilderness_assembler.transform(data).\
        drop(*wilderness_cols).\
        withColumn("wilderness", unhot_udf(col("wilderness"))) ➋
    soil_cols = ['Soil_Type_' + str(i) for i in range(40)]
    soil_assembler = VectorAssembler().setInputCols(soil_cols).setOutputCol("soil")
    with_soil = soil_assembler.transform(with_wilderness).drop(*soil_cols).withColumn("soil", unhot_udf(col("soil")))
    return with_soil
```

Note UDF definition Drop one-hot columns; no longer needed