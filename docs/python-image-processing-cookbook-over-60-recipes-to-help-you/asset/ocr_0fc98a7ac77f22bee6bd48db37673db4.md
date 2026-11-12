# Clustering handwritten digit images with SOM

Go through the following steps to cluster the digits images from the scikit-image dataset:

+   1. Load the digits and scale:

```
digits = datasets.load_digits(n_class=10)
data = digits.data # matrix where each row is a vector that
represent a digit.
data = scale(data)
num = digits.target # num[i] is the digit represented by data[i]
```

+   2. Create a SOM grid with 900 (30 x 30) neurons. Initialize the neurons with PCA weights:

```
som = MiniSom(30, 30, 64, sigma=4, learning_rate=0.5,
neighborhood_function='triangle')
som.pca_weights_init(data)
```

+   3. Train the SOM by choosing 5,000 randomly chosen digits using the following line of code:

```
som.train_random(data, 5000) # random training
```

+   3. Finally, plot the distance map of the weights (each cell is the normalized sum of the distances between a neuron and its neighbors) and overlay the images of the digits on the cells (neurons):

```
plt.figure(figsize=(15, 12))
pcolor(som.distance_map().T, cmap='coolwarm')
plt.colorbar()
wmap = defaultdict(list)
im = 0
for x, t in zip(data, num): # scatterplot
    w = som.winner(x)
    wmap[w].append(im)
    plt.text(w[0]+.5, w[1]+.5, str(t),
               color=plt.cm.Dark2(t / 10.), fontdict={'weight': '\
               'bold', 'size': 11})
im = im + 1
plt.axis([0, som.get_weights().shape[0], 0,
         som.get_weights().shape[1]])
plt.show()
```