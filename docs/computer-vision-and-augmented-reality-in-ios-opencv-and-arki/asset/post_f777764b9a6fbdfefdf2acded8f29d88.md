
## Kernelized Correlation Filter (KCF)

The ideas discussed in the previous two trackers are expanded upon in the Kernelized Correlation Filter tracker. This tracker incorporates MIL tracker’s multiple positive samples that include wide interacting areas. This concurrent research defines some interesting geometric parameters, which this tracker uses to make tracking both quicker and much more precise.

Advantages: It outperforms BOOSTING and MIL in terms of accuracy and speed as well as reporting tracking failure. I suggest using this for most applications if you’re using OpenCV 3.1 or higher.

Disadvantages: After a complete obstruction, it doesn’t restore.

## MedianFlow Tracker

From within, the detector calculates the differences among the object’s forward and backward trajectories in a period. It can accurately detect tracking errors and pick accurate trajectories in clip sequences by reducing the forward/backward error. In my experiments, I discovered that this tracker works when the motion is small and predictable. This tracker can detect when the tracking has failed and stops accordingly, however other trackers continue running even if the tracking has failed.

Advantages: Outstanding reporting on monitoring failures. If a motion is predictable and there is no constriction, this technique works extremely well.

Disadvantages: When there is a lot of movement, it stalls.

## GOTURN Tracker

It is the only tracking algorithm throughout the tracker class that uses a Convolutional Neural Network (CNN). We understand that OpenCV is “reliable to prospective modifications, illumination changes, and defects” with its documents.

To be considered: The Caffe model is used by GOTURN, which is a CNN-based tracker.

## MOSSE Tracker

MOSSE stands for minimum output sum of squared error. While initialized with a single frame, an adaptive correlation is used for object tracking, resulting in balanced association filtering. Variations in illumination, length, and posture and non-rigid deformations do not affect the MOSSE tracker.

The maximum average ratio of the squared error is often used to identify deformation, allowing the tracker to stop and restart in which it leaves whenever the object shows up. It is indeed simple to use, reliable, as well as much quicker than other complicated trackers. However, it falls short of deep learning-based trackers in terms of accuracy.

## CSRT Tracker

The spatial reliability map is used in the Discriminative Correlation Filter with Channel and Spatial Reliability (DCF-CSR) to change the filter aid to the portion of the specified location through a framework for tracking. It only makes use of two standard features (HoGs and Color Names). It also has a lower frame rate (25 fps) but has better object tracking reliability.

## C) Image-to-Text Analysis

### Using the C++ OCR Library, Convert an Image to Text

OCR (Optical Character Recognition) technology allows you to interpret and translate text from images or scanned documents into human-understandable text. Reading codes from coupons, creating text format-able, self-service shops, translating print information into digital versions, and so on are only a few of the applications for OCR. There seems to be a variety of OCR software and libraries on the market, but the accuracy of the OCR tests is crucial. In this section, I’ll show you how to use C++ to build our own OCR software and transform images to text.

### Using C++ to Convert an Image Page to Text

Let’s start with a scenario in which the picture includes several lines of text. Maybe this is the case if you have a scanned book with a large number of text lines per page. In this case, the steps to transform an image to text are as follows.

- In a string variable, store the image’s direction.
- Make a buffer to keep the OCR results in.
- The aspose::ocr::page(const char *image path, wchar_t *buffer, size_t buffer size) function is used to perform OCR.
- The results can be printed or saved to a file.
- The following program example demonstrates how to use C++ to conduct OCR and transform an image to text:

```
std::string image_path = "sample.png";

// Prepare buffer for result (in symbols, len_byte = len * sizeof(wchar_t))
const size_t len = 4096;
wchar_t buffer[len] = { 0 };

// Perform OCR
size_t size = aspose::ocr::page(image_path.c_str(), buffer, len);

//Print result
std::wcout << buffer << L"\n";
```

### Using C++, Perform Single-Line OCR on an Image

We transformed an image with multiple text lines in the previous example. When the picture only has a single line of text, such as a caption or a slogan, this may be the case. In such instances, the steps for performing OCR are as follows:

- To set the image's direction, use a string variable.
- To save the OCR data, create a buffer.
- The aspose::ocr::line(const char *image path, wchar_t *buffer, size_t buffer size) function is used to perform OCR.
- The OCR findings can be saved or printed.

The following code sample demonstrates how to use C++ to implement OCR on a picture with a short piece of text:

```
std::string image_path = "sample_line.jpg";

// Prepare buffer for result (in symbols, len_byte = len * sizeof(wchar_t))
const size_t len = 4096;
wchar_t buffer[len] = { 0 };

// Perform OCR
size_t size = aspose::ocr::line(image_path.c_str(), buffer, len);

//Print result
std::wcout << buffer << L"\n";
```

### In C++, Convert a Specific Area of an Image into Text

You can also configure the API to restrict the OCR to a specific area of the image. Throughout this scenario, you can access the desired area by drawing a rectangle on the image. The steps for extracting text from a specific area of an image are as follows:

- In a variable, store the image’s direction.
- Make a buffer to keep OCR results in.
- The aspose::ocr::page_rect(const char *image path, wchar_t *buffer, size_t buffer size, int x, int y, int w, int h) function is used to perform OCR.
- The OCR results should be printed.

The following C++ code example demonstrates how to transform a specific area of an image to text:

```
std::string image_path = "sample_line.jpg";

// Prepare buffer for result (in symbols, len_byte = len * sizeof(wchar_t))
const size_t len = 4096;
wchar_t buffer[len] = { 0 };

int x = 138, y = 352, w = 2033, h = 537;

// Perform OCR or selected area
size_t size = aspose::ocr::page_rect(image_path.c_str(), buffer, len, x, y, w, h);

//Print result
std::wcout << buffer << L"\n";
```

## CHAPTER 5

## Custom Object Tracking

In this chapter how to track moving and stationary objects is covered in real time using downscaled picture details through measuring the period averaging background scene.

### Overview

While working through a clip stream rather than single static pictures, we frequently have a specific object or items in mind to track across a visual field.

The basic issue of tracking in computer vision occurs in one of the two types; you will either be tracking previously identified objects or tracking unknown objects, across many scenarios, recognizing them depending on their movement.

There’s also the issue of modeling moreover in tracking. At their finest, tracking strategies send us messy estimates of an object’s actual location from frame to frame (Figure 5-1). Modeling helps us overcome this. In measuring a trajectory of an object evaluated messily, several effective mathematic methodologies have been made. Such strategies may be used to create two-dimensional or three-dimensional representations of objects as well as their positions.

![](img/7d8e54ee4578088e252f4c75b5cd4c88_96_0.png)

Figure 5-1. Object Tracking

### Real-Time Object Tracking

The topic illustrates how to use a conventional web cam to track moving or fixed objects in real time. As objects begin to display, a simple method of tracking would be to compare the pre-specified background picture to an identical background frame. Such a situation operates within fixed environments in which you can understand a background picture despite having to track any foreground objects, for example, indoors, landings, workplaces, markets, workshops, and so on. If a paper moves or stays fixed in such a scenario across a longer length of time, one should treat it as if it is already a portion of a scenario as well as superimpose the picture toward the background to prevent additional detections. If a background object is eliminated by a scenario, the same applies. A lack of lighting and the need to adjust the cam location are both disadvantages of this method. You’ll have to measure the latest background again in this scenario. A further option is to utilize edge operators for reducing adjustments in illumination. If you want to describe the object’s body rather than only the outlines, you’ll have to write some extra code.
