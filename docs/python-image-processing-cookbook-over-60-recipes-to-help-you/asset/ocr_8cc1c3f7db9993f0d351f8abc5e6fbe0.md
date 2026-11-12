```
plt.suptitle('PCA reconstruction with {} components
(eigenfaces)'.format(n_components), size=25)
plt.show()
```

5. Use FFT lowpass filter (LPF) by discarding all but the thirty lowest-frequency components and reconstruct a noisy image with these basis vectors. Display the reconstructed images:

```
n_components = 30
plt.figure(figsize=(20, 4))
for i in range(len(indices)):
    freq = fftshift(fft2((np.reshape(faces[indices[i], :],
    image_shape)).astype(float)))
    freq[:freq.shape[0]//2 - n_components//2, :] =
    freq[freq.shape[0]//2 + n_components//2:, :] = 0
    freq[:, :freq.shape[1]//2 - n_components//2] = freq[:,
    freq.shape[1]//2 + n_components//2:] = 0
    plt.subplot(1, 5, i+1),
    plt.imshow(ifft2(ifftshift(freq)).real), plt.axis('off')
    plt.suptitle('FFT LPF reconstruction with {} basis
vectors'.format(n_components), size=25)
plt.show()
```

6. Use DWT and discard the seventh band to reconstruct an image. Display the reconstructed images:

```
plt.figure(figsize=(20, 4))
wavelet = pywt.Wavelet('haar')
for i in range(len(indices)):
    wavelet_coeffs = pywt.waverec2((np.reshape(faces[indices[i], :],
    image_shape)).astype(float), wavelet)
    plt.subplot(1, 5, i+1),
    plt.imshow(pywt.waverec2(wavelet_coeffs[:-1], wavelet)),
    plt.axis('off')
    plt.suptitle('Wavelet reconstruction with {
subbands'.format(len(wavelet_coeffs)-1), size=25)
plt.show()
```

# How it works...

The `decomposition.PCA()` function from scikit-learn was used to instantiate a PCA object. The `fit_transform()` function was first used to project the images to a lower dimension (from 4096 to 50 dimensions), and then the `inverse_transform()` function was used to reconstruct the images from the lower-dimensional representations.