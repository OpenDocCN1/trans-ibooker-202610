# 4. Apply soft-thresholding to further increase the percentage of zeros in the transformed image:

```
imw = soft_threshold(imw, 12)
f3 = np.mean(imw==0)
print ("Fraction of zeros in wavelet transform (after division \\
 by 8 & soft thresholding): {:}".format(f3))
# Fraction of zeros in wavelet transform (after division by 8 \\
 & soft thresholding): # 0.945472172851562
im3 = mahotas.wavelet_decenter(mahotas.daubechies(imw, 'D8'), \\
 im.shape)
im3 = (255 * (im3 - np.min(im3)) / (np.max(im3) - \\
 np.min(im3))).astype(np.uint8)
```

A much higher compression ratio can be achieved with this image, enabling faster transmission over communication channel and less bandwidth consumption, and after transmission, you will be able to reconstruct the image with a quality higher than the one reconstructed from the compressed image using baseline compression, created by just keeping every fourth pixel and low-order bits.

# How it works...

If you execute the preceding code and plot the images, you will get a screenshot like the following one:

![](bbox=[0.293, 0.561, 0.706, 0.884])