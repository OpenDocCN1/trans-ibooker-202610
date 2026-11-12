# See also

Refer to the following links to learn more about this recipe:

+   - https://www.studocu.com/en/document/university-of-pennsylvania/computer-vision-computational-photography/lecture-notes/cis581-morph-2018-morphing-lecture-notes/3317837/view
+   - https://www.seas.upenn.edu/~cse399b/Lectures/CSE399b-07-triangle.pdf
+   - http://alumni.media.mit.edu/~maov/classes/comp_photo_vision08f/lect/07_Image%20Morphing.pdf
+   - http://en.wikipedia.org/wiki/Bilinear_interpolation
+   - https://www.cs.toronto.edu/~mangas/teaching/320/slides/CSC320T12.pdf
+   - https://www.cs.princeton.edu/courses/archive/fall100/cs426/papers/beier92.pdf
+   - https://inst.eecs.berkeley.edu/~cs194-26/fa18/hw/proj4/index.html
+   - https://www.learnopencv.com/face-morph-using-opencv-cpp-python
+   - http://davis.wpi.edu/~matt/courses/morph/2d.htm
+   - https://www.youtube.com/watch?v=i_tG7XRLPmU

# Implementing an image search engine

In this recipe, you will learn how to implement a simple image search engine (SE). It will be a search by example system, relying only on the image contents, known as content-based image retrieval (CBIR) systems. The images along with the features extracted are stored so that the system can return similar images (based on the features) during a search. The following describes the four steps of any CBIR system:

+   1. Defining an image descriptor (descriptive features of an image)
+   2. Indexing search images (for quick retrieval of the images with similar descriptors to the query image. Use an efficient data structure for fast retrieval)
+   3. Defining the similarity metric to be used (Euclidean/cosine/chi-squared distance, and so on)
+   4. Searching (the user submits a query image to the SE, and the SE extracts features from this query image and uses the indexed features of the images stored to quickly return the most relevant images using the similarity metric— retrieval should be fast using the efficient data structure for indexing)

You will first learn how to compute the similarity of a query image with a set of search images and then you will extend the idea to implement a simple SE.