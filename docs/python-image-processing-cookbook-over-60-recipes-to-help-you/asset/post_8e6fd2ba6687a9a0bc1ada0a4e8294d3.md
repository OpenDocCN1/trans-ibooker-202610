
```python
faces = np.empty((len(detected), img_size, img_size, 3))
for i, d in enumerate(detected):
    x1, y1, x2, y2, w, h = d.left(), d.top(), d.right() + 1, \
    d.bottom() + 1, d.width(), d.height()
    faces[i, :, :, :] = cv2.resize(img[y1:y2+1, \
    x1:x2+1, :], (img_size, img_size))
results = loaded_model.predict(faces)
predicted_genders = results[0]
ages = np.arange(0, 101).reshape(101, 1)
predicted_ages = results[1].dot(ages).flatten()
```

If you run the preceding code block and draw the detected faces along with the ages and genders of the people in the images, you will get an output as in the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_383_0.png)

The keras.models.model_from_json() function was used to parse the JSON model configuration file and get a model instance.

The keras.models.predict() function was used to run a forward pass and generate the output age and gender predictions for the input faces.

## There's more...

Similarly, use the other pre-trained model to predict the emotion in a face in a given image (use the face detector function to detect the faces first). You should get an output like this if you draw the predicted emotions of Charlie Chaplin's face:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_384_0.png)

You may want to download the five celebrities' datasets from https://www.kaggle.com/dansbecker/5-celebrity-faces-dataset and try different face images for emotion, gender, and age recognition.

## See also

- https://github.com/yu4u/age-gender-estimation
- https://github.com/jalajthanaki/Facial_emotion_recognition_using_Keras
- https://stackoverflow.com/questions/53859419/dlib-get-frontal-face-detector-gets-faces-in-full-image-but-does-not-get-in-c
- https://www.vision.ee.ethz.ch/en/publications/papers/articles/eth_biwi_01299.pdf

## Image colorization with deep learning

In this recipe, you will learn how to use a pre-trained deep learning model to convert a grayscale image into a plausible color version. Zhang et al. propose a fully automatic image-colorization model that produces realistically colored images given a grayscale input image. The model was practiced on over a million target color images. In the testing phase, we just need to run a forward pass on the CNN to predict the output colored image when given a grayscale input. The algorithm was evaluated using a colorization Turing test, where the human participants were asked to choose between a model-generated and a ground-truth color image (which resulted in the model successfully fooling the humans in 32% of the trials). The following diagram shows the architecture of the deep CNN:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_385_0.png)

## Getting ready

First, download the pre-trained Caffe model from https://github.com/richzhang/colorization/tree/master/colorization/models (for example, the colorization_deploy_v2.prototxt and .caffemodel files) and https://github.com/richzhang/colorization/blob/master/colorization/resources/pts_in_hull.npy. Import all the necessary packages using the following code block:

```python
import numpy as np
import cv2
import matplotlib.pyplot as plt
import imutils
```

## How to do it...

- 1. Read the model into memory from the Caffe `prototxt` and the model `weights` files:

```python
proto_file = "models/colorization_deploy_v2.prototxt"
weights_file = "models/colorization_release_v2.caffemodel"
net = cv2.dnn.readNetFromCaffe(proto_file, weights_file)
```

- 2. Load the bin centers and populate the cluster centers as 1 x 1 convolution kernels using the following code snippet:

```python
pts_in_hull = np.load('models/pts_in_hull.npy')
pts_in_hull = pts_in_hull.transpose().reshape(2, 313, 1, 1)
net.getLayer(net.getLayerId('class8_ab')).blobs = [pts_in_hull.astype(np.float32)]
net.getLayer(net.getLayerId('conv8_313_rh')).blobs = [np.full([1, 313], 2.606, np.float32)]
```

- 3. For each of the input images, read the image from the disk, convert it to a Lab color space, and pull out the `L` channel using the following code snippet:

```python
width = height = 224
for f in glob('images/tocolorize/*.png'):
    image = cv2.cvtColor(cv2.cvtColor(cv2.imread(f), cv2.COLOR_BGR2GRAY), cv2.COLOR_GRAY2BGR)
    img_rgb = (image[:,:,[2, 1, 0]] * 1.0 / 255).astype(np.float32)
    img_lab = cv2.cvtColor(img_rgb, cv2.COLOR_RGB2Lab)
    img_l = img_lab[:,:,0]
```

- 4. Resize the lightness (L) channel and the image to the network input size. Run the forward pass to get the predicted output, as shown here:

```python
img_l_rs = cv2.resize(img_l, (width, height))
img_l_rs -= 50 # subtract 50 for mean-centering
net.setInput(cv2.dnn.blobFromImage(img_l_rs))
ab_dec = net.forward()[0,:,:,:].transpose((1,2,0))
```

- 5. Reshape the predicted output to the input image size and convert from `Lab` to `BGR` space to get the colored output image:

```python
(orig_height, orig_width) = img_rgb.shape[:2]
ab_dec_us = cv2.resize(ab_dec, (orig_width, orig_height))
img_lab_out = np.concatenate((img_l[:, :, np.newaxis], \
                              ab_dec_us), axis=2)
img_bgr_out = np.clip(cv2.cvtColor(img_lab_out, \
                              cv2.COLOR_Lab2BGR), 0, 1)
```

If you run the preceding code blocks and plot the output color images obtained from the input grayscale images, you will get an output as in the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_387_0.png)

The `readNetFromCaffe()` function from OpenCV-Python, downloaded in step 1, was used to load a pre-trained deep neural network model stored in the Caffe framework's format. It accepts a path to the `.prototxt` file (a text file describing the architecture of the neural net) and a path to the `.caffemodel` file (a file storing the pre-trained weights for the model).

## See also

- https://arxiv.org/pdf/1603.08511.pdf
- https://github.com/richzhang/colorization/blob/master/colorization/
- https://github.com/opencv/opencv/blob/master/samples/dnn/colorization.py
- http://richzhang.github.io/colorization/
- https://www.youtube.com/watch?v=M4WOPN3qjVI
- https://www.youtube.com/watch?v=vEoAGeu3NLQ

## Automatic image captioning with a CNN and an LSTM

Automatic captioning of an image is a popular problem in AI that connects image processing and computer vision with NLP. In this recipe, you will learn how to use a pre-trained generative model (known as Show and Tell) based on a deep recurrent neural network architecture that can be used to generate captions (complete sentences in a natural language describing the contents of an image). The model was trained with the objective to maximize the likelihood of the input caption texts given the input training images.

im2txt is a TensorFlow implementation of the Show and Tell model that can take images as input and generate human-like captions that describe the image. The model was tested on more than 300,000 images. The model is an end-to-end deep neural network consisting of a CNN (used to learn the implicit features of an input image) followed by an RNN (used to generate captions when given the CNN features). The implementation uses BeamSearch to iteratively select the best k captions for an input image. The following diagram shows the architecture of the Show and Tell model:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_388_0.png)

## Getting ready

First, clone the repository (using `https://github.com/tensorflow/models/`) to a folder in your local machine. Then, copy the `models/tree/master/research/im2txt/im2txt` folder to the `models` folder used in this chapter. Create a sub-folder, called `cpt`, and download the pre-trained model checkpoint file (`model.ckpt-2000000`) in `cpt` from `https://drive.google.com/file/d/0Bw6m_66JSYLlRFVKQ2tGcUJaWjA/`. Create an empty `__init__.py` file in `im2txt` to mark this folder as a Python package directory. The folder structure should look like this:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_389_0.png)

Let's import the required libraries using the following code block:

```python
import os, sys
import tensorflow as tf
from glob import glob
from skimage.io import imread
import matplotlib.pyplot as plt
from im2txt import configuration
from im2txt import inference_wrapper
from im2txt.inference_utils import caption_generator
from im2txt.inference_utils import vocabulary

checkpoint_dir = 'models/im2txt/cpt/'
sys.path.append(os.path.join(os.getcwd(), 'models/'))
```
