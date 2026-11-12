The corner_harris() and corner_peaks() functions from skimage.feature are used to obtain the Harris Corner keypoints from the input images (it finds points with a large corner response function R, by picking the local maxima in R, as shown in the next screenshot):

![](bbox=[0.374, 0.202, 0.623, 0.35])

The skimage.feature module's BRIEF class was instantiated and its extract() method was used to compute the binary descriptors corresponding to the keypoints for both images. BRIEF is an efficient binary feature descriptor that is often very discriminative. Hamming distance is often used with a binary descriptor (such as BRIEF) for feature matching, leading to lower computational cost (no floating-point operation is needed) than the L2 norm.

> For the copyright of BRIEF, please refer to the following link:
> https://scikit-image.org/docs/dev/license.html

Again, the skimage.feature module's match_descriptors() function was used to match the corresponding keypoints and the matched keypoints are only returned. The skimage.feature module's ranac() function was used to fit a homography model for the transformation with the RANSAC algorithm.

The parameters of the ranac() function were the source/destination images, the model class (ProjectiveTransform), the number of min_samples (set to 4 since the minimum number of data points to estimate homography is 8, using the 8-point algorithm), residual_threshold (maximum distance for a data point to be classified as an inlier), and max_trials (maximum number of iterations for the algorithm).