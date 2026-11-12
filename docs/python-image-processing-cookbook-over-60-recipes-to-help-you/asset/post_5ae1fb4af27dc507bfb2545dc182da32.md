
## How to do it...

Run the following steps to implement image captioning using a pre-trained im2txt model on TensorFlow:

- 1. The downloaded pre-trained weights checkpoint file (`model.ckpt-2000000`) contains some issues and you will most likely fail to load it. In that case, fix the issue in the checkpoint by modifying it and creating a new checkpoint file (`model2.ckpt-2000000`) inside the `models` folder using code from `https://github.com/tensorflow/models/issues/466` or `https://github.com/PacktPublishing/Python-Image-Processing-Cookbook/blob/master/Chapter%2009/Chapter%2009.ipynb`.

- 2. Build the inference graph from the modified checkpoint file and then load the vocabulary for LSTM (a particular type of RNN that can learn long-term dependencies):

```
checkpoint_path = checkpoint_dir + 'model2.ckpt-2000000'
vocab_file = checkpoint_dir + 'word_counts.txt'

g = tf.Graph()
with g.as_default():
    model = inference_wrapper.InferenceWrapper()
    restore_fn = model.build_graph_from_config\
        (configuration.ModelConfig(), checkpoint_path)

g.finalize()

vocab = vocabulary.Vocabulary(vocab_file)
```

- 3. Load the pre-trained model from the model checkpoint file using the restore_fn() function. Get the caption generator initialized with the pre-trained model parameters. Here, we will use the default beam search parameters (you can always explicitly specify the parameter values if you want to experiment). Ignore the beginning and end words (the start and end markers of a sentence) and output the generated captions using the following code snippet:

```
filenames = glob('images/captioning/*.png')
tf.logging.info("Running caption generation on %d files matching
%s", len(filenames), filenames)
with tf.Session(graph=g) as sess:
    restore_fn(sess)
    generator = caption_generator.CaptionGenerator(model, vocab)
    for filename in filenames:
        with tf.gfile.GFile(filename, "rb") as f: image = f.read()
        captions = generator.beam_search(sess, image)
        print("Captions for image %s: " %
            os.path.basename(filename))
        for i, caption in enumerate(captions):
            sentence = " ".join([vocab.id_to_word(w) for w in \
            caption.sentence[1:-1]])
        print(" %d) %s (p=%f) " % (i, sentence, \
            math.exp(caption.logprob)))
```

If you run the preceding code block on the input images from the `images` and `captions` folders, you will generate the following captions:

## Automatic image captioning with deep learning pretrained model

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_0.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_1.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_2.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_3.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_4.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_5.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_6.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_7.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_8.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_9.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_10.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_11.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_12.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_13.png)

![](img/7e2c8bbde0ebcd14e54d2862adfee480_392_14.png)

As you can see, most of the captions generated are reasonably good and accurate.

## How it works...

The `build_graph_from_config()` function builds the inference graph from a configuration object. It takes a model `config` object containing the configuration for building the model and a `checkpoint` path containing the `checkpoint` file. It returns a function that can be used to load the model variables from the `checkpoint` file.

The `beam_search()` function runs beam search caption generation on a single image. It accepts a TensorFlow session object and a CNN-encoded image string as input and returns a list of captions sorted by descending score.

See `caption_generator.py` for a description of the available beam search parameters.

## See also

- https://github.com/tensorflow/models/tree/master/research/im2txt/im2txt
- https://edouardfouche.com/Fun-with-Tensorflow-im2txt/
- https://github.com/tensorflow/models/issues/466
- https://github.com/tensorflow/tensorflow/issues/30794
- https://arxiv.org/pdf/1609.06647.pdf
- https://www.youtube.com/watch?v=yk6XDFm3J2c

## Image generation with a GAN

A **generative adversarial network (GAN)** is a generative model that defines an adversarial net framework and is composed of a couple of models (both models are CNNs in general), namely a generator and a discriminator, with the goal of generating new realistic images when given a set of training images. These two models act as adversaries of each other: the generator learns to generate new fake images that look like real images (starting with random noise) while the discriminator learns to determine whether a sample image is a **real** or a **fake** image.

The generator plays the role of a counterfeiter that is trying to produce a fake image and fool the discriminator, whereas the discriminator plays the role of the police that tries to detect the fake image generated by the discriminator. We can think of this as a two-player game and competition in this game drives both teams to improve their methods until the fake images are indistinguishable from the real images (for example, the Nash equilibrium between the discriminator and generator adversaries). Once training is over, a new sample image (resembling the real image) can be generated just by running a forward propagation on the generator. Both CNN models try to optimize opposing objective (loss) functions in a **zero-sum** game. The following diagram describes the basic GAN framework and also how the GAN is trained (here, it is trained on human faces):

![](img/7e2c8bbde0ebcd14e54d2862adfee480_394_0.png)

In this recipe, you will learn how to train a GAN using PyTorch to generate realistic face images, given a set of real face images. Since it is likely to take quite a long time to train the GAN, using a GPU to train a GAN is highly recommended.

### Getting ready

First, download the anime face dataset from `https://www.kaggle.com/splcher/animefacedataset` (create a Kaggle account if you don't already have one) and extract the images into the `anime` sub-directory in the `images` directory (there are 63,565 images as of now). Import all the required packages using the following code block:

```
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
import pickle as pkl
import torch
import torch.nn as nn
from torchvision import datasets
from torchvision import transforms
from torch.utils.data.sampler import SubsetRandomSampler
import torch.nn.functional as F
import torch.optim as optim
```

### How to do it...

Run the following steps to train a GAN on the anime faces using PyTorch and to generate new realistic anime face images:

- 1. Define the following function to make the PyTorch data loader apply `transform` to resize the images of a given batch size from a given directory, and then return them using a Python generator:

```
def get_data_loader(batch_size, image_size, data_dir='anime/'):
    image_transforms = transforms.Compose(\
        [transforms.Resize(image_size), \
         transforms.ToTensor(), ])
    indices = np.random.choice(63565, 50000) # get 50k random samples
    data_loader = torch.utils.data.DataLoader( \
        datasets.ImageFolder(data_dir, \
        transform=image_transforms), \
        sampler=SubsetRandomSampler(indices), \
        batch_size=batch_size)
    return data_loader

batch_size = 128
image_size = 32
anime_train_loader = get_data_loader(batch_size, image_size)
images, _ = iter(anime_train_loader).next()
```

If you plot a few sample input images from the anime dataset and show them using matplotlib, you will get an output like this:
