2. Read the input image and convert it into grayscale. Blur the input grayscale image using box-kernel (done in the frequency domain) and degrade the image using White Gaussian Noise (WGN) with a given standard deviation (σ):

```
x = rgb2gray(imread('images/building.png'))
M, N = x.shape
h = np.ones((4,4))/16 # blur filter
h = np.pad(h, [(M//2-2, M//2-2), (N//2-2, N//2-2)], \
mode='constant')
sigma = 0.05
Xf = fp.fft2(x)
Hf = fp.fft2(fp.ifftshift(h))
Y = Hf*Xf
y = fp.ifft2(Y).real + sigma*np.random.normal(size=(M,N))
```

3. Apply the pseudo_inverse filter on the degraded image to get back the restored image along with the frequency response of the pseudo-inverse filter kernel:

```
epsilon = 0.25
pix, F_pseudo = pseudo_inverse_filter(y, h, epsilon)
```

4. Apply the wiener filter with the balance parameter as 0.25 to get back the restored image:

```
wx = wiener(y, h, balance=0.25)
```

5. Next, apply the unsupervised_wiener filter to get back the restored image:

```
uwx, _ = unsupervised_wiener(y, h)
```

6. Construct the high-pass filter (2D Laplacian) and convert it into the frequency domain, to use it as the constraint-kernel. Apply the CLS filter on the degraded image using the constraint kernel created, along with a regularization parameter (λ) value:

```
c = np.array([[0,1/4,0],[1/4,-1,1/4],[0,1/4,0]])
c = np.pad(c, [(M//2-1, M//2-2), (N//2-2, N//2-1)], \
mode='constant')
Cf = fp.fft2(fp.ifftshift(c))
lambd = 20
clx, F_restored = cls_filter(y, h, c, lambd)
print(r'Restored (CLS, $\lambda$=${}) PSNR: {}'.format(lambd, \
      np.round(compare_psnr(x, clx),3))
# Restored (CLS, $\lambda$=20) PSNR: 28.924
```