# # How it works...

The `draw_box()` function does the following:

+   1. It draws a bounding box around a detected object.
+   2. It prints a label of the (most probable) class the object is assigned to.
+   3. It displays the label to the top-left corner of the bounding box.
+   4. Then, it resizes the mask, threshold, and color and applies it to the image.
+   5. Finally, it draws the contours on the image corresponding to the mask.

The `post_proces()` function extracts the bounding box along with the mask for every object detected for each image. Then, it chooses the right color (corresponding to the class label of the object) to draw the object bounding box and overlay the mask on the image.

# # There's more...

Train the Mask R-CNN network on the MS-COCO dataset (download the dataset from here: `http://cocodataset.org/`) and save the model trained (a GPU is highly recommended); use it to predict objects in your image.

# # See also

Refer to the following links to learn more about this recipe:

+   - `https://arxiv.org/pdf/1703.06870.pdf`
+   - `https://www.pyimagesearch.com/2018/11/19/mask-r-cnn-with-opencv/`
+   - `https://github.com/facebookresearch/Detectron`
+   - `https://arxiv.org/pdf/1405.0312.pdf`
+   - `https://www.youtube.com/watch?v=g7z4mkfRjI4`
+   - `https://www.youtube.com/watch?v=FR25P1lMBY8`
+   - `https://www.youtube.com/watch?v=FR25P1lMBY8`