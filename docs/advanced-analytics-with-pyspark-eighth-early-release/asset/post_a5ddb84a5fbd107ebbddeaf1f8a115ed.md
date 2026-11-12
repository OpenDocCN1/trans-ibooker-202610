

- 1. Left join with tss_df created earlier
- 2. Get the closest TSS distance

This final DF is computed in each pass of the loop over the cell lines. Finally, we union each DF from each cell line, and cache this data in memory in preparation for training models off of it:

```
preTrainingData = data_by_cellLine.union(...)
preTrainingData.cache()

preTrainingData.count()
preTrainingData.filter(fun.col("label") == true).count()
```

At this point, the data in preTrainingData can be normalized and converted into a DataFrame for training a classifier, as described in “Random Forests”. Note that you should perform cross-validation, where in each fold, you hold out the data from one of the cell lines.

## Where to Go from Here

Many computations in genomics fit nicely into the PySpark computational paradigm. When you’re performing ad hoc analysis, the most valuable contribution that projects like ADAM provide is the set of Avro schemas that represents the underlying analytical objects (along with the conversion tools). We saw how once data is converted into the corresponding Avro schemas, many large-scale computations become relatively easy to express and distribute.

While there may still be a relative dearth of tools for performing scientific research on PySpark, there do exist a few projects that could help avoid reinventing the wheel. We explored the core functionality implemented in ADAM, but the project already has implementations for the entire GATK best-practices pipeline, including BQSR, indel realignment, and deduplication. In addition to ADAM, the Broad Institute is now developing major software projects using Spark, including the newest version of the GATK4 and a project called Hail for large-scale population genetics computations. All of these tools are open source, so if you start using them in your own work, please consider contributing improvements!

## Chapter 10. Image similarity detection with Deep Learning and PySpark LSH

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 10th chapter of the final book.

Whether you encounter them on social media or e-commerce stores, images are integral to our digital lives. In fact, it was an image dataset - ImageNet - which was a key component for sparking the current deep learning (DL) revolution. A remarkable performance by a classification model in the ImageNet 2012 challenge was an important milestone and led to widespread attention. It is no wonder then that you are likely to encounter image data at some point as a data science practitioner.

In this chapter, you will gain experience scaling a deep learning workflow for a visual task, namely image similarity detection, with PySpark. The task of identifying images that are similar to each other comes intuitively to humans but it is a complex computational task. At scale, it becomes even more difficult. In this chapter, we will introduce an approximate method for finding similar items called Locality Sensitive Hashing or LSH and apply it to images. We will use deep learning to convert image data into a numerical vector representation. PySpark’s LSH algorithm will be applied to the resulting vectors which will allow us to find similar images given a new input image.

On a high-level, this example mirrors one of the approaches used by photo-sharing apps such as Instagram and Pinterest for image similarity detection. This helps their users make sense of the deluge of visual data that exists on their platforms. This also depicts how a deep learning workflow can benefit from PySpark’s scalability.

We will start by briefly introducing PyTorch, a deep learning framework. It has gained prominence in recent years for its relatively easier learning curve compared to other major low-level DL libraries. We will then download and prepare our dataset. The dataset being used for our task is the Cars dataset released in 2013 by Stanford AI Lab. PyTorch will be used for image preprocessing. This will be followed by conversion of our input image data into a vector representation (image embeddings). We will then import the resulting embeddings into PySpark and transform them using the LSH algorithm. We will finish off by taking a new image and performing a nearest neighbors search using our LSH-transformed dataset to find similar images.

Let’s start by introducing and setting up PyTorch.

### PyTorch

PyTorch is a library for building deep learning projects. It emphasizes flexibility and allows deep learning models to be expressed in idiomatic Python. It found early adopters in the research community. Recently, it has grown into one of the most prominent deep learning tools across a broad range of applications due its ease of use. Along with Tensorflow, it is the most popular library for deep learning as of now.

PyTorch’s simple and flexible interface enables fast experimentation. You can load data, apply transforms, and build models with a few lines of code. Then, you have the flexibility to write customized training, validation, and test loops and deploy trained models with ease. It is consistently being used in professional contexts for real-world, mission-critical work. Being able to use GPUs (graphical processing units) for training resource-intensive models has been a big factor for making deep learning popular. PyTorch provides great GPU support although we won’t need that for our task.

### Installation

On the PyTorch website, you can easily obtain the installation instructions based on your system configuration, as shown in the Figure 10-1. We will use . Execute the provided command and follow the instructions for your configuration.

| PyTorch Build | Stable (1.10) | Preview (Nightly) | LTS (1.8.2) |
|---|---|---|---|
| Your OS | Linux | Mac | Windows |
| Package | Conda | Pip | LibTorch | Source |
| Language | Python | C++ / Java | | |
| Compute Platform | CUDA 10.2 | CUDA 11.3 | ROCm 4.2 (beta) | CPU |
| Run this Command: | pip3 install torch torchvision torchaudio | | |

Figure 10-1. PyTorch installation - CPU support

```
$ pip3 install torch torchvision
```

We will not be relying on a GPU and hence, will choose CPU as a compute platform. In case you have a GPU setup that you want to utilise, choose options accordingly to obtain required instructions. We will not be needing torchaudio for this chapter either so we skip its installation.

### Preparing the data

We will be using the Stanford Cars dataset. It was released as part of the ICCV 2013 paper “3D Object Representations for Fine-Grained Categorization” by Jonathan Krause, Michael Stark, Jia Deng and Li Fei-Fei.

You can download the images from Kaggle or using the source link provided by Stanford AI Lab.

```
wget http://ai.stanford.edu/~jkrause/car196/car_ims.tgz
```

Once downloaded, unzip the train and test image directories. Place them in a directory called cars_data.

```
data_directory = "cars_data"
train_images = "cars_data/cars_train/cars_train"
```

You can get CSV file containing labels for the training dataset here. Download it, rename it to cars_train_data.csv and place it in the data directory. Let us have a look at it.

```
import pandas as pd
train_df = pd.read_csv(data_directory+"/cars_train_data.csv")
train_df.head()
...
```

| Unnamed: 0 | x1 | y1 | x2 | y2 | Class |
|---|---|---|---|---|---|
| image | | | | | |
| 0 | 00001.jpg | 0 | 39 | 116 | 569 | 375 | 14 |
| 1 | 00002.jpg | 1 | 36 | 116 | 868 | 587 | 3 |
| 2 | 00003.jpg | 2 | 85 | 109 | 601 | 381 | 91 |
| 3 | 00004.jpg | 3 | 621 | 393 | 1484 | 1096 | 134 |
| 4 | 106 | 00005.jpg | 4 | 14 | 36 | 133 | 99 |

Ignore all columns other than Class and image. They are related to the original research project that this dataset was derived from and will not be used for our task.

### Resizing images using PyTorch

Before we head further, we'll need to preprocess our images. Preprocessing data is very common in machine learning since deep learning models (neural networks) expect the input to meet certain requirements.

We need to apply a series of preprocessing steps, called transforms, to convert input images into the proper format for the models. In our case, we need them to be 224 x 224-pixel JPEG-formatted images since that is a requirement for the Resnet-18 model that we will use in the next section. We perform this transformation using PyTorch’s Torchvision package in the following code:

```
import os
from PIL import Image
from torchvision import transforms

# needed input dimensions for the CNN
input_dim = (224,224)
input_dir_cnn = data_directory + "/images/input_images_cnn"

os.makedirs(input_dir_cnn, exist_ok = True)

transformation_for_cnn_input = transforms.Compose([transforms.Resize(input_dim) ])

for image_name in os.listdir(train_images):
    I = Image.open(os.path.join(train_images, image_name))
    newI = transformation_for_cnn_input(I)
    newI.save(os.path.join(input_dir_cnn, image_name))

    newI.close()
    I.close()
```

Here, we use a single transformation that resizes the image to fit within the neural networks. However, we can use the Compose() transform to define a

