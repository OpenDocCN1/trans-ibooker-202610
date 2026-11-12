# 2. Next, let's define the zero_crossing() function:

```
def zero_crossing(img):
    img[img > 0] = 1
    img[img < 0] = 0
    out_img = np.zeros(img.shape)
    for i in range(1, img.shape[0]-1):
        for j in range(1, img.shape[1]-1):
            if img[i,j] > 0 and any_neighbor_zero(img, i, j):
                out_img[i,j] = 255
    return out_img
```

# 3. Finally, invoke the zero_crossing() function on the LoG-convolved input image and plot the output:

```
img = rgb2gray(misc.imread('images/tiger.png'))
fig = plt.figure(figsize=(25,15))
plt.gray() # show the filtered result in grayscale
for sigma in range(2, 10, 2):
    plt.subplot(2, 2, sigma/2)
    result = ndimage.gaussian_laplace(img, sigma=sigma)
    result = zero_crossing(result)
    plt.imshow(result)
    plt.axis('off')
    plt.title('LoG with zero-crossing, sigma=' + str(sigma), size=30)
plt.tight_layout()
plt.show()
```

The following diagram shows the input image used for edge detection:

![](bbox=[0.353, 0.596, 0.645, 0.705])

If you run the preceding code snippet, you will get an output similar to the following (using the preceding input image):