

```python
0, 1)),
    transforms.Lambda(lambda tensor:tensor_round(tensor))
 ])
```

- 4. Download the lfw dataset and create a dataloader with a batch size of 8:

```python
batch_size = 8 # 16
dataset = fetch_lfw_people(min_faces_per_person=70,
   resize=0.4).images / 255
dataloader = DataLoader(dataset, batch_size=batch_size,
   shuffle=True)
```

- 5. Now, implement the autoencoder class, the encoder and decoder members, and the forward() method of the class:

```python
class autoencoder(nn.Module):
    def __init__(self):
        super(autoencoder, self).__init__()
        self.encoder = nn.Sequential(
            nn.Linear(50 * 37, 512),
            nn.ReLU(True),
            nn.Linear(512, 128),
            nn.ReLU(True))
        self.decoder = nn.Sequential(
            nn.Linear(128, 512),
            nn.ReLU(True),
            nn.Linear(512, 50 * 37),
            nn.Sigmoid())
    def forward(self, x):
        x = self.encoder(x)
        x = self.decoder(x)
        return x
```

- 6. Instantiate the autoencoder class, and then define the loss function as binary cross-entropy (BCE) loss and the optimizer as the Adam optimizer using the following code snippet:

```python
learning_rate = 1e-3
cuda = False #True
model = autoencoder()
if cuda:
    model = model.cuda()
criterion = nn.BCELoss()
optimizer = torch.optim.Adam(model.parameters(),
    lr=learning_rate, weight_decay=1e-5)
```

- 7. Train the autoencoder for 100 epochs. In every epoch, use forward propagation for autoencoder prediction (and loss function computation) and backpropagation to update the weights in different layers:

```python
num_epochs = 100
for epoch in range(1, num_epochs+1):
    for data in dataloader:
        img = data
        #...
        if cuda: noisy_img = noisy_img.cuda()
        img = Variable(img)
        if cuda: img = img.cuda()
        output = model(noisy_img)     # forward-prop
        loss = criterion(output, img)
        MSE_loss = nn.MSELoss()(output, img)
        optimizer.zero_grad()
        loss.backward()   # back-prop
        optimizer.step()
        print('epoch [{}/{}] , loss::{:.4f}, MSE_loss::{:.4f}'
              .format(epoch, num_epochs, loss.data.item(), MSE_loss.data.item()))
        if epoch % 10 == 0:
            x = to_img(img.cpu().data)
            x_hat = to_img(output.cpu().data)
            x_noisy = to_img(noisy_img.cpu().data)
            weights = to_img(model.encoder[0].weight.cpu().data)
```

- 8. Print the model to see the net architecture:

```python
print (model)
# autoencoder(
#   (encoder): Sequential(
#     (0): Linear(in_features=1850, out_features=512, bias=True)
#     (1): ReLU(inplace)
#     (2): Linear(in_features=512, out_features=128, bias=True)
#     (3): ReLU(inplace)
#   )
#   (decoder): Sequential(
#     (0): Linear(in_features=128, out_features=512, bias=True)
#     (1): ReLU(inplace)
#     (2): Linear(in_features=512, out_features=1850, bias=True)
#     (3): Sigmoid()
#   )
# )
```

The following diagram shows the structure of the network and how the preceding denoising autoencoder works with the lfw faces dataset:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_63_0.png)

## How it works...

We used `nn.Sequential()` from PyTorch to build the autoencoder by sequentially specifying the building blocks (`nn.Module`). Each pixel from a noisy image forms an input node, hence the input has 50*37 nodes. The output also has the same number of nodes.

A fully connected layer of size 1,850 x 512 was instantiated with `nn.Linear(50*37, 512)`. `nn.ReLU(True)` was used to apply in-place ReLU non-linearity in between the input/hidden layers and `nn.Sigmoid()` before the final layer to apply the sigmoid function at the output layer. Binary cross-entropy loss was used with `nn.BCELoss()`. The `backward()` function was used to backpropagate and recompute the weights in the neural net.

We can run the code on a GPU by setting cuda = True to run it much faster.

The following screenshot shows the original image, the noisy image, the reconstructed output image with the autoencoder, and a few learned features in the first hidden layer of the encoder at the end of two different epochs:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_64_0.png)

As you can see in the following screenshot, with more epochs, the denoised images generated as output by the autoencoder become more and more similar to the corresponding original image:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_65_0.png)

## There's more...

Repeat the preceding denoising process with colored images (pass color=True to the fetch_lfw_people() function). Train an overcomplete autoencoder (where the number of hidden layer nodes is greater than the number of input nodes, with a single hidden layer) using the noisy images. What happens if you train this with the original images both at input and output? Now, train a sparse overcomplete autoencoder with original images both at input and output, but using an L1 regularizer at the hidden layer.

## See also

For more information, refer to the following links:

- http://vis-www.cs.umass.edu/lfw/
- https://scikit-learn.org/0.15/datasets/labeled_faces.html
- https://pytorch.org/docs/stable/_modules/torch/nn/modules/linear.html
- https://www.youtube.com/watch?v=B6YO5_RLA1A

## Image denoising with PCA/DFT/DWT

Principal component analysis (PCA), discrete Fourier transform (DFT), and discrete wavelet transform (DWT) are traditional machine learning techniques that can be used to denoise images as well. Each of these techniques will learn a representation (an approximation) of the image space and will retain mostly the information content in the images and remove the noise.

## Getting ready

We will use the Olivetti faces dataset for this recipe. The dataset contains a total of 400 grayscale face images (each of size 64 x 64), 10 per each of the 40 objects. As usual, let's start by importing the required libraries:

```python
import numpy as np
from numpy.random import RandomState
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_olivetti_faces
from sklearn import decomposition
from skimage.util import random_noise
from skimage import img_as_float
```

```python
from time import time
import scipy.fftpack as fp
import pywt
```

## How to do it...

Perform the following steps to implement the filters:

- 1. Download the faces data first:

```python
dataset = fetch_olivetti_faces(shuffle=True, random_state=rng)
original = img_as_float(dataset.data)
faces = original.copy()
print(faces.shape)
# (400, 4096)
```

- 2. Add random Gaussian noise to the image with a variance of 0.005:

```python
image_shape = (64, 64)
rng = RandomState(0)
n_samples, n_features = faces.shape
faces = random_noise(faces, var=0.005)
```

- 3. Use PCA and reconstruct the images using only 50 dominant principal components:

```python
n_components = 50 # 256
estimator = decomposition.PCA(n_components=n_components,
                              svd_solver='randomized', whiten=True)
print("Extracting the top %d PCs..." % (n_components))
t0 = time()
faces_recons = estimator.inverse_transform(estimator.fit_transform(faces))
train_time = (time() - t0)
print("done in %0.3fs" % train_time)
```

- 4. Randomly select five image indices and display the original image, the noisy image, and the PCA reconstructions:

```python
indices = np.random.choice(n_samples, 5, replace=False)
plt.figure(figsize=(20, 4))
for i in range(len(indices)):
    plt.subplot(1,5,i+1),
    plt.imshow(np.reshape(faces_recons[indices[i],:], image_shape)),
    plt.axis('off')
plt.suptitle('PCA reconstruction with {} components (eigenfaces)'.format(n_components), size=25)
plt.show()
```

- 5. Use FFT lowpass filter (LPF) by discarding all but the thirty lowest-frequency components and reconstruct a noisy image with these basis vectors. Display the reconstructed images:

```python
n_components = 30
plt.figure(figsize=(20, 4))
for i in range(len(indices)):
    freq = fftshift(fft2((np.reshape(faces[indices[i], :],
    image_shape)).astype(float)))
    freq[:freq.shape[0]//2 - n_components//2, :] = 0
    freq[freq.shape[0]//2 + n_components//2:, :] = 0
    freq[:, :freq.shape[1]//2 - n_components//2] = 0
    freq[:, freq.shape[1]//2 + n_components//2:] = 0
    plt.subplot(1, 5, i+1),
    plt.imshow(ifft2(ifftshift(freq)).real), plt.axis('off')
    plt.suptitle('FFT LPF reconstruction with {} basis vectors'.format(n_components), size=25)
plt.show()
```

- 6. Use DWT and discard the seventh band to reconstruct an image. Display the reconstructed images:

```python
plt.figure(figsize=(20, 4))
wavelet = pywt.Wavelet('haar')
for i in range(len(indices)):
    wavelet_coeffs = pywt.waverec2((np.reshape(faces[indices[i], :],
    image_shape)).astype(float), wavelet)
    plt.subplot(1, 5, i+1),
    plt.imshow(pywt.waverec2(wavelet_coeffs[:-1], wavelet)),
    plt.axis('off')
    plt.suptitle('Wavelet reconstruction with {} subbands'.format(len(wavelet_coeffs)-1), size=25)
plt.show()
```

## How it works...

The `decomposition.PCA()` function from scikit-learn was used to instantiate a PCA object. The `fit_transform()` function was first used to project the images to a lower dimension (from 4096 to 50 dimensions), and then the `inverse_transform()` function was used to reconstruct the images from the lower-dimensional representations.

