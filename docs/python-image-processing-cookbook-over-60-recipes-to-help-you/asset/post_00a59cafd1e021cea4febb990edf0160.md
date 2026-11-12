
## Getting ready

We shall use the Lena RGB color image to demonstrate the steganography/steganalysis techniques using the stegano library. Let's import the required packages, modules, and functions at the very outset, using the following code block:

```python
# !pip install Stegano
import stegano
from PIL import Image, ImageChops
from stegano import lsb, lsbset
from stegano.steganalysis import statistics, parity
import matplotlib.pylab as plt
import pandas as pd
from stegano import lsbset
from stegano.lsbset import generators
from stegano import exifHeader
```

## How to do it...

Execute the following steps to hide a secret message inside the Lena image using LSB data hiding and use (non-blind) steganalysis to detect the secret message:

- 1. Read the (cover) image and hide a long message (by concatenating a message 10 times) inside the image. Save the stego image:

```python
cover = Image.open('images/lena.png')
stego = lsb.hide("images/lena.png", 10*"Python Image Processing \
Cookbook - LSB data hiding with Stegano")
stego.save("images/lena-secret.png")
```

- 2. Print the message hidden inside the stego image:

```python
print(lsb.reveal("images/lena-secret.png"))
```

- 3. Do parity steganalysis and extract the parity encoded cover and stego images. Then, use statistical steganalysis to retrieve the common cover and stego images using the following code snippet:

```python
parity_encoded_cover = parity.steganalyse(cover)
parity_encoded_stego = parity.steganalyse(stego)
_, cover_common = statistics.steganalyse(cover)
_, stego_common = statistics.steganalyse(stego)
```

If you plot all of the images along with the difference between the stego and cover image and the difference between the parity encoded stego and cover images, you will get a figure like the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_132_0.png)

As you can see from the preceding screenshot, by seeing the difference in the image, between the stego and the cover image, the existence of the secret message can't be detected. The parity-encoded stego and cover image are not the same and the difference reveals that there is some secret message embedded inside the stego image.

- 4. Define a `plot_freq()` function to plot the histograms of the common pixel frequencies where the cover and stego images differ (obtained earlier with statistical steganalysis):

```python
plot_freq(cover_common, stego_common)
```

You will get the output shown in the following screenshot, which clearly shows that the stego and the cover image were different:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_133_0.png)

- 5. Next, use the LSB technique with sets based on generators (Sieve of Eratosthenes) to hide the secret message:

```python
cover = Image.open("images/lena.png").convert('RGB')
secret_message = "Python Image Processing Cookbook – LSB data \
hiding with Stegano lsbset!"
n = 1000
stego = lsbset.hide("images/lena.png",
secret_message,
generators.eratosthenes(),
shift = n).convert('RGB')
stego.save("images/stego.png")
```

If you plot the difference between the stego and cover image versus the difference between the parity-encoded stego and cover image, you will get the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_133_1.png)

- 6. Now, try to retrieve the hidden message using the same generator (it will succeed) and a different one (it will fail):

```python
try:
    message = lsbset.reveal("images/stego.png",
                        generators.fibonacci())
except:
    print('Could not decode with the generator provided!')
    # Could not decode with the generator provided!

message = lsbset.reveal("images/stego.png", \
                        generators.eratosthenes())
message
# 'Python Image Processing Cookbook - LSB data hiding with Stegano
lsbset!'
```

- 7. Finally, for JPEG and TIFF images, you can hide a message in the exifHeader (and reveal it) using the following code:

```python
secret = exifHeader.hide("images/butterfly.jpg", \
                   "images/stego.png",
                   secret_message=5*"Python
Image Processing Cookbook - LSB data hiding with Stegano")
print(exifHeader.reveal("images/stego.png"))
```

## How it works...

The open() and save() methods from the PIL Image object were used to load and save an image, respectively. The lsb.hide() function was used for LSB data-hiding, which takes the cover image and the stego text as input parameters and returns the stego image. The lsb.reveal() function was used to get the stego text back from a stego image. parity.steganalyse() was used to obtain a parity-encoded image from an image (stego and cover).

The difference() function from PIL ImageChops was used to compute the difference image between two input images (for example, stego and cover). statistics.steganalyse() returns the pixel values with frequencies (histogram) where the cover and stego images differ in frequency.

## There's more...

There are many different types of steganography techniques. You can find one based on number systems (refer to the first link in the following See also section) and implement it on your own.

## See also

Refer to the following articles to further expand your skills in relation to this recipe:

- https://arxiv.org/pdf/1003.3672.pdf
- https://buildmedia.readthedocs.org/media/pdf/stegano/latest/stegano.pdf

## Binary Image Processing

Morphological image processing refers to the application of a set of non-linear operation(s) that is related to the shape (that is, morphology) of image features. These operations are particularly suited to the processing of binary images (where pixels are represented as 0 or 1 and, by convention, the foreground of the object = 1 or white, and the background = 0 or black) since they don't depend on the exact numerical values of the pixels; rather, they depend only on how the pixel values are relatively ordered—although these operations can be extended to grayscale images.

Hence, in this chapter, we will discuss binary image processing with morphological operations, although a few grayscale morphological operations will also be discussed, for the sake of completeness. In morphological operations, a structuring element (a small template image) is used to probe the input image. The Structuring Element (SE) is placed at all possible locations in the input image and then an algorithm uses a set operator to compare it with the corresponding pixels' neighborhood. Morphological operations test whether the SE fits within, or hits or intersects, the corresponding neighborhood.

In this chapter, you will cover the following recipes:

- Applying morphological operators to a binary image
- Applying morphological filters
- Morphological pattern matching
- Segmenting images with morphology
- Counting objects

## Applying morphological operators to a binary image

Dilation and erosion are two fundamental morphological operators. Erosion removes a pixel layer from the foreground (white) objects' boundaries and thereby shrinks the foreground in a binary image. The small-scale details get removed and the size of the regions of interest gets reduced by erosion in a binary image. On the other hand, dilation adds a pixel layer to the foreground objects' boundaries thereby expanding the foreground. The holes contained inside a single foreground object and gaps in between foreground objects (and boundaries) are reduced.

Many morphological operations can be obtained as combinations of erosion, dilation, and basic set operations (for example, complement). Morphological opening, closing, and hit-or-miss transform are the most popular ones. Opening is an idempotent operation (implemented with a dilation followed by erosion) that can disconnect the foreground objects connected by a thin layer of pixels, by keeping the surviving objects' size the same. Similarly, closing (dual operator of opening: implemented with an erosion followed by dilation) is another idempotent operation that can fill holes in the foreground regions while keeping the region sizes the same. The following screenshot shows how the fundamental and a few compound morphological operators are defined using the operators from set theory:

| Operator | Definition |
| :--- | :--- |
| Erosion | \( A \ominus B = \{z | (B)_z \subseteq A \} \) shrink the (foreground) object |
| Dilation | \( A \oplus B = \{z | (\hat{B})_z \cap A \neq \emptyset \} \) grow the (foreground) object |
| Opening | \( A \circ B = (A \ominus B) \oplus B \) |
| Closing | \( A \bullet B = (A \oplus B) \ominus B \) |
| Hit or Miss Transform | \( A * \{B_1, B_2\} = (A \ominus B_1) \cap (A^c \ominus B_2) \) |
| Duality | \( (A \bullet B)^c = (A^c \circ \hat{B}) \) |

In this recipe, you will learn how to apply a few morphological operators to a binary image.
