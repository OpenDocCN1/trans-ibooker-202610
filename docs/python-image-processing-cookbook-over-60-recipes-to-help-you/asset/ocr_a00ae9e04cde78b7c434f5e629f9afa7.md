![](bbox=[0.226, 0.106, 0.773, 0.376])

+   2. Extract the SIFT keypoints and descriptors for the query image and all of the search images using the following code:

```
sift = cv2.xfeatures2d.SIFT_create()
kp_1, desc_1 = sift.detectAndCompute(query, None)
kp_2, desc_2 = sift.detectAndCompute(search_image, None)
```

+   3. Use an approximate nearest neighbors based match for fast matching of the descriptors (you can use your own data structure for fast retrieval, for example, use kd-trees from scikit-learn):

```
index_params = dict(algorithm=FLANN_INDEX_KDTREE, trees=5)
search_params = dict()
flann = cv2.FlannBasedMatcher(index_params, search_params)
matches = flann.knnMatch(desc_1, desc_2, k=2)
```

+   4. Again, use the ratio test (as in the earlier recipes) to find good matches:

```
good_points = []
ratio = 0.6
for m, n in matches:
    if m.distance < ratio*n.distance:
        good_points.append(m)
num_good_points = len(good_points)
```