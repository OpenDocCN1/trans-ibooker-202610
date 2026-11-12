2. Extract the object and background seed pixels from the mask image with the following code and create a new marker image in the way that the random_walker function expects (that is, mark the seed pixels with distinct positive labels for each segment and leave the unlabeled pixels as zero labels):

```
markers = np.zeros(img.shape[:2], np.uint8)
markers[(mask[..., 0] >= 200) & (mask[..., 1] <= 20) & (mask[..., 2] <= 20)] = 1
markers[(mask[..., 0] <= 20) & (mask[..., 1] >= 200) & (mask[..., 2] <= 20)] = 2
```

3. Run the random walker algorithm to segment the images in order to obtain the binary segmentation and the full probabilities assigned to the pixels using the following lines of code:

```
labels = random_walker(img, markers, beta=9, mode='bf',
multichannel=True)
labels2 = random_walker(img, markers, beta=9, mode='bf',
multichannel=True, return_full_prob = True)
```

4. Finally, plot the input images and the results obtained:

```
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(20,
                             18), sharex=True, sharey=True)
fig.subplots_adjust(0, 0, 1, 0.95, 0.01, 0.01)
ax1.imshow(mask, interpolation='nearest'), ax1.axis('off')
ax1.set_title('Original Image with Markers', size=25)
ax2.imshow(img, interpolation='nearest'), ax2.contour(labels,
                     linewidths=5, colors='r'), ax2.axis('off')
ax2.set_title('Segmentation Contour', size=25)
ax3.imshow(labels, cmap='gray', interpolation='nearest'),
ax3.axis('off')
ax3.set_title('Segmentation', size=25)
prob = ax4.imshow(labels2[1, ...], cmap='inferno',
                  interpolation='nearest')
ax4.axis('off'), ax4.set_title('Segmentation Probabilities',
                   size=25)
fig.colorbar(prob, ax=ax4)
plt.show()
```