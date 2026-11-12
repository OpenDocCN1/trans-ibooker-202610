be thought of as `is_cloudy`, `is_rainy`, and `is_clear` features.
Likewise, 40 of the columns are really one `Soil_Type` categorical feature.
This isn’t the only possible way to encode a categorical feature as a number.
Another possible encoding simply assigns a distinct numeric value to each
possible value of the categorical feature. For example, `cloudy` may become
1.0, `rainy` 2.0, and so on. The target itself, “Cover_Type”, is a categorical
value encoded as a value 1 to 7.

> Be careful when encoding a categorical feature as a single numeric feature. The original
> categorical values have no ordering, but when encoded as a number, they appear to. Treating
> the encoded feature as numeric leads to meaningless results because the algorithm is
> effectively pretending that `rainy` is somehow greater than, and two times larger than,
> `cloudy`. It’s OK as long as the encoding’s numeric value is not used as a number.

We have seen both types of encodings of categorical features. It would have,
perhaps, been simpler and more straightforward to not encode such features
(and in two ways, no less), and instead simply include their values directly like
“Rawah Wilderness Area.” This may be an artifact of history; the data set was
released in 1998. For performance reasons or to match the format expected by
libraries of the day, which were built more for regression problems, data sets
often contain data encoded in these ways.

In any event, before proceeding, it is useful to add column names to this
DataFrame in order to make it easier to work with:

```
from pyspark.sql.types import DoubleType
from pyspark.sql.functions import col

colnames = ["Elevation", "Aspect", "Slope",
"Horizontal_Distance_To_Hydrology",
"Vertical_Distance_To_Hydrology", "Horizontal_Distance_To_Roadways",
"Hillshade_9am", "Hillshade_Noon", "Hillshade_3pm",
"Horizontal_Distance_To_Fire_Points"] + \
[f"Wilderness_Area_{i}" for i in range(4)] + \
[f"Soil_Type_{i}" for i in range(40)] + \
["Cover_Type"]

data = data_without_header.toDF(*colnames).withColumn("Cover_Type",
col("Cover_Type").cast(DoubleType()))
```