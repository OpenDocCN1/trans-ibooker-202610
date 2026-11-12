| Name | Description | Data Type | Measurement |
| :--- | :--- | :--- | :--- |
| Elevation | Elevation in meters | quantitative | meters |
| Aspect | Aspect in degrees azimuth | quantitative | azimuth |
| Slope | Slope in degrees | quantitative | degrees |
| Horizontal_Distance_To_Hydrology | Horz Dist to nearest surface water features | quantitative | meters |
| Vertical_Distance_To_Hydrology | Vert Dist to nearest surface water features | quantitative | meters |
| Horizontal_Distance_To_Roadways | Horz Dist to nearest roadway | quantitative | meters |
| Hillshade_9am | index Hillshade index at 9am, summer solstice | quantitative | 0 to 255 |
| Hillshade_Noon | index Hillshade index at noon, summer solstice | quantitative | 0 to 255 |
| Hillshade_3pm | index Hillshade index at 3pm, summer solstice | quantitative | 0 to 255 |
| Horizontal_Distance_To_Fire_Points | Horz Dist to nearest wildfire ignition points | quantitative | meters |
| Wilderness_Area (4 binary columns) | or 1 (presence) Wilderness area designation | qualitative | 0 (absence) |
| Soil_Type (40 binary columns) | or 1 (presence) Soil Type designation | qualitative | 0 (absence) |
| Cover_Type (7 types) | Forest Cover Type designation | integer | 1 to 7 |

Looking at the column information, it’s clear that some features are indeed numeric. “Elevation” is an elevation in meters; “Slope” is measured in degrees. However, “Wilderness_Area” is something different, because it is said to span four columns, each of which is a 0 or 1. In reality, “Wilderness_Area” is a categorical value, not a numeric one.

These four columns are actually a one-hot or 1-of-n encoding. When this form of encoding is performed on a categorical feature, one categorical feature that takes on N distinct values becomes N numeric features, each taking on the value 0 or 1. Exactly one of the N values has value 1, and the others are 0. For example, a categorical feature for weather that can be cloudy, rainy, or clear would become three numeric features, where cloudy is represented by 1, 0, 0; rainy by 0, 1, 0; and so on. These three numeric features might