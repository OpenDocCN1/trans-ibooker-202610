# C++

```
1 // Import the aruco module in OpenCV
2 #include <opencv2/aruco.hpp>
3 
4 Mat markerImage;
5 // Load the predefined dictionary
6 Ptr<cv::aruco::Dictionary> dictionary =
7 aruco::getPredefinedDictionary(cv::aruco::DICT_6X6_250);
8 // Generate the marker
9 aruco::drawMarker(dictionary, 33, 200, markerImage, 1);
```

The preceding drawMarker function allows us to select a marker with a specific ID (the second parameter – 33) out of a set of 250 markers with IDs ranging from 0 to 249. The size of the marker produced is determined by the drawMarker function’s third parameter. It might produce a 200 × 200-pixel picture throughout the preceding instance. The object that will be used to preserve the created marker would be the fourth parameter (preceding markerImage). Eventually, a density parameter refers to the number of blocks that should be inserted as a boundary toward the formed binary pattern. Inside the illustration earlier, a 1-bit border would be inserted across the 66% produced shape, resulting in a 77% bit picture in a 200 × 200-pixel picture (Figure 6-2). The preceding code will produce a marker similar to the one shown in the following.

![](bbox=[0.34, 0.65, 0.62, 0.82])

# Figure 6-2. Sample Marker

For most cases, we’ll need to create several markers, print them out, as well as position them inside the scenario.