# # There is more

For feature-based similarity comparison between two images, you could use the proportion (percentage) of matches as the ranking criterion too (for example, have the percentage of a match in the keypoints as key in the matched_images dictionary). Also, for scalability and reusability, you should serialize the descriptors for all of the search images you have in your search directory (for example, use pickle) and load/deserialize all of the descriptors when you want to match with a query image descriptor—it is also left for you as an exercise. You can use SURF, KAZE, or any other feature for matching and any distance/similarity metric between the descriptors. For production, use an algorithm for fast computation of the similarities between millions of images (for example, try Annoy Index, which is simple to use and pretty fast—searching 1,000,000 images takes ~2 ms).

Use different features (SIFT/SURF/KAZE), different similarity metrics (cosine/Euclidean), and different storage data structures (kd-tree/ball-tree) to implement your search engine and compare the top 10 search results. If you have a ground truth available (in terms of which top 10 images should be returned), you can compare different search engines' performance with precision10 (defined as out of the top 10 search results, what proportion was in the top 10 ground truth) / recall10 (defined as out of top 10 ground truth, what proportion was returned in the top 10 search results). Also, compare the speed of retrieval.

# ## See also

Refer to the following links to learn more about this recipe:

+   - https://www.pyimagesearch.com/2014/12/01/complete-guide-building-image-search-engine-python-opencv/
+   - https://www.pyimagesearch.com/2019/08/26/building-an-image-hashing-search-engine-with-vp-trees-and-opencv/
+   - https://medium.com/machine-learning-world/feature-extraction-and-similar-image-search-with-opencv-for-newbies-3c59796bf774