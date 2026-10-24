
Figure 8-1 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_123_0.png)

## Figure 8-1. Visualizing an image

This is a color image. The Matplotlib library automatically detects that the image has multiple channels and shows it as a color image. However, it goofs up a little bit when we show grayscale images.

```
img2 = plt.imread("D:/Dataset/5.1.11.tiff")
plt.imshow(img2)
plt.show()
```

Figure 8-2 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_123_1.png)

## Figure 8-2. Visualizing a grayscale image

The image data is interpreted correctly, but there seems to be some problem with the color. For grayscale images, Matplotlib uses the default color map, so you have to manually specify the color map as follows:

```
plt.imshow(img2, cmap = 'gray')
plt.show()
```

Figure 8-3 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_124_0.png)

Figure 8-3. Visualizing a grayscale image with the correct color map

A color map is a matrix of values defining the colors for visualizations. Let’s try another color map for the image, as shown here:

```
plt.imshow(img2, cmap = 'cool')
plt.show()
```

Figure 8-4 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_125_0.png)

**Figure 8-4**. Visualizing a grayscale image with a cool color map

You can display a list of color maps in the current version of Matplotlib by using the following statement:

```
plt.colormaps()
```

The output is as follows:

```
['Accent',
'Accent_r',
........
'twilight_r',
'twilight_shifted',
'twilight_shifted_r',
'viridis',
'viridis_r',
'winter',
'winter_r']
```

I have removed a big portion of the output so that it will fit in the book. You can use any of these color maps for your visualization needs. As an exercise, try a few color maps with a grayscale image.

## Image Masking

You can mask the areas of an image with a circle as follows:

```
import matplotlib.patches as patches
fig, ax = plt.subplots()
im = ax.imshow(img1)
patch = patches.Circle((245, 200),
                    radius=200,
                    transform=ax.transData)
im.set_clip_path(patch)
ax.axis('off')
plt.show()
```

In this code example, we are creating a circle with the routine Circle() at the XY co-ordinates 245, 200. The radius is 200 pixels. Also, we are clipping the image with the circle using the routine set_clip_path() and showing it. Figure 8-5 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_126_0.png)

Figure 8-5. Clipping an image with a circle

## Interpolation Methods

You can show a simple NumPy Ndarray as an image as follows:

```
img3 = [[1, 2, 3, 4],
        [5, 6, 7, 8],
        [9, 10, 11, 12],
[13, 14, 15, 16]
plt.imshow(img3)
plt.show()
```

Figure 8-6 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_127_0.png)

*Figure 8-6.* NumPy Ndarray as an image

The image is using no interpolation method for visualization. We can demo interpolation methods as follows:

```
```python
methods = ['none', 'antialiased', 'nearest', 'bilinear',
           'bicubic', 'spline16', 'spline36', 'hanning',
           'hamming', 'hermite', 'kaiser', 'quadratic',
           'catrom', 'gaussian', 'bessel', 'mitchell',
           'sinc', 'lanczos', 'blackman']

fig, axs = plt.subplots(nrows=4, ncols=6, figsize=(9, 6),
                        subplot_kw={'xticks': [], 'yticks': []})

for ax, interp_method in zip(axs.flat, methods):
    ax.imshow(img3, interpolation=interp_method, cmap='hot')
    ax.set_title(str(interp_method))

plt.tight_layout()
plt.show()
```
```

## CHAPTER 8  |  IMAGE AND AUDIO VISUALIZATION

The output is as follows:

```
```text
<class 'numpy.ndarray'>
(2601617, 2)
2
int16
5203234
10406468
```
```

The audio data is retrieved and stored in the NumPy, as you have seen. It is stored in a 2D matrix. Suppose that there are N data points (also known as *sample points*) for the audio data; then the size of the NumPy array is N×2. As you can see, the audio has two channels, left and right. So, each channel is stored in a separate array of size N, and thus we have N×2. This is known as *stereo audio*. In this example, we have 2,601,617 points (samples). Each point or sample is represented using a pair of integers of 16 bits (2 bytes). Thus, each sample needs four bytes. So, we can compute the total raw memory required for storing the audio data by multiplying the sample size by 4. When we visualize audio, we show the value of both channels of the sample. Let’s visualize the first 2,000 data points as follows:

```
```python
plt.plot(data[:2000])
plt.show()
```
```

Figure 8-8 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_130_0.png)

**Figure 8-8.** *Visualization of an audio file*

You can check the number of audio samples as follows:

```
samples = data.shape[0]
print(samples)
```

The output is as follows:

2601617

You can create a different visualization of the data as follows:

```
plt.plot(data[:10*samplerate])
plt.show()
```

Figure 8-9 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_131_0.png)

Figure 8-9. Visualization of an audio file

Let’s separate the data for both channels as follows:

```
channel1 = data[:, 0]
channel2 = data[:, 1]
print(channel1, channel2)
```

The output is as follows:

[-204 -233 -191 ... 646 679 623] [ 23 32 34 ... 676 707 650]

Let’s visualize the data as follows:

```
plt.subplot(2, 1, 1)
plt.plot(channel1[:10*samplerate])
plt.subplot(2, 1, 2)
plt.plot(channel2[:10*samplerate], c='g')
plt.show()
```

Figure 8-10 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_132_0.png)

Figure 8-10. Visualization of both audio channels

## Audio Processing

The Fourier transform converts the data represented as waves in the time domain into the frequency domain. So, when you compute the Fourier transform and visualize it, you are seeing the representation in the frequency domain.

The fast Fourier transform (FFT) is an efficient method of computing a Fourier transform of the waveform data. FFT reduces the number of computations, which is why it is speedy; that’s why it is known as a fast Fourier transform. Let’s compute the fast Fourier transform of the audio signal as follows:

```
import scipy.fftpack
datafft = scipy.fftpack.fft(data)
fftabs = abs(datafft)
print(fftabs)
```

The output is as follows:

```
[[ 181.  227.]
[ 201.  265.]
[ 157.  225.]
...
[1322.   30.]
[1386.   28.]
[1273.   27.]]
```

Let's compute the frequency and plot the graph as follows:

```
freqs = scipy.fftpack.fftfreq(samples, 1/samplerate)
plt.plot(freqs, fftabs)
plt.show()
```

Figure 8-11 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_133_0.png)

Figure 8-11. Visualization of FFT

## Summary

In this chapter, you learned how to create visualizations for images and audio.

In the next chapter, you will learn how to visualize pie and polar charts.

You can even separate the parts of the pie a bit, as follows:

```
explode = [0.0, 0.05, 0.1, 0.15]
plt.pie(data,
         labels = mylabels,
         explode = explode)
plt.show()
```

The output will have the parts of the pie separated as per the values in the explode argument. Figure 9-3 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_136_0.png)

*Figure 9-3.* A simple pie chart with labels and explosion

You can also enable shadows as follows:

```
plt.pie(data,
         labels = mylabels,
         explode = explode,
         shadow = True)
plt.show()
```

You can add a title for the legend as follows:

```
plt.pie(data,
    labels = mylabels,
    explode = explode,
    shadow = True)
plt.legend(title='Data :')
plt.show()
```

Figure 9-6 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_138_0.png)

Figure 9-6. A simple pie chart with legends and a title for the legend

You’ve just learned how to create polar graphs.

## Polar Charts

You can also create polar graphs that are in the shape of pie charts. However, a fundamental difference from the Cartesian (X-Y) coordinate system is that in a polar chart the coordinate system is radially arranged, so you need the angle (theta) and distance from the origin (r is the radius) to visualize a point or set of points. Let’s create a dataset as follows:
