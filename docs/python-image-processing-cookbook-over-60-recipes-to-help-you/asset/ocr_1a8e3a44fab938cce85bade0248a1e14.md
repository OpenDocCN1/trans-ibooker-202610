+   - minNeighbors: This is a parameter that specifies the number of neighbors each candidate rectangle needs to keep. This parameter affects the quality of the detected faces—a larger value enables detection with higher quality, but smaller in numbers.
+   - minSize and maxSize: These are the minimum and maximum possible object sizes, respectively. Objects of sizes beyond these values will be ignored.

When faces are detected, the positions the faces are returned by the function as a list of Rect(x, y, w, h).

Once a face bounding box is obtained, it defines the ROI for the face, and then the eye/smile detection on this ROI can be applied (since the eyes/smile are always to be found on the face).

# There's more...

Use the dlib HOG-based frontal face detector and OpenCV's Single Shot MuliBox Detector (SSD) pretrained deep learning model to detect faces in images. Compare different face detectors' performances (in terms of accuracy and time complexity). Try with face images with different angles/orientations and with eye glasses. Detect eyes with the dlib facial landmark detection.

# See also

Refer to the following links to learn more about this recipe:

+   - https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-IJCV-01.pdf
+   - https://www.learnopencv.com/face-detection-opencv-dlib-and-deep-learning-c-python/
+   - https://arxiv.org/abs/1512.02325
+   - https://github.com/opencv/opencv_extra/blob/master/testdata/dnn/
+   - https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_smile.xml
+   - https://docs.opencv.org/3.4/d1/de5/classcv_1_1CascadeClassifier.html#a90fe1b7778bed4a27aa8482e1eecc116
+   - Chapter 7 of the book, Hands-on Image Processing with Python