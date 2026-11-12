+   1. Left join with tss_df created earlier
+   2. Get the closest TSS distance

This final DF is computed in each pass of the loop over the cell lines. Finally, we union each DF from each cell line, and cache this data in memory in preparation for training models off of it:

```
preTrainingData = data_by_cellLine.union(...)
preTrainingData.cache()

preTrainingData.count()
preTrainingData.filter(fun.col("label") == true).count()
```

At this point, the data in preTrainingData can be normalized and converted into a DataFrame for training a classifier, as described in “Random Forests”. Note that you should perform cross-validation, where in each fold, you hold out the data from one of the cell lines.

# Where to Go from Here

Many computations in genomics fit nicely into the PySpark computational paradigm. When you’re performing ad hoc analysis, the most valuable contribution that projects like ADAM provide is the set of Avro schemas that represents the underlying analytical objects (along with the conversion tools). We saw how once data is converted into the corresponding Avro schemas, many large-scale computations become relatively easy to express and distribute.

While there may still be a relative dearth of tools for performing scientific research on PySpark, there do exist a few projects that could help avoid reinventing the wheel. We explored the core functionality implemented in ADAM, but the project already has implementations for the entire GATK best-practices pipeline, including BQSR, indel realignment, and deduplication. In addition to ADAM, the Broad Institute is now developing major software projects using Spark, including the newest version of the GATK4 and a project called Hail for large-scale population genetics