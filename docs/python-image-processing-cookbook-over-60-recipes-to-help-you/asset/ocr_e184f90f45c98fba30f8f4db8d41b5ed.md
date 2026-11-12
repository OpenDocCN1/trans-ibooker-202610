```
region_mask = np.zeros(im.shape[:2], dtype=np.uint8)
cv2.rectangle(region_mask, (start_x, start_y), (endX, \
endY), 255, -1)
hist = cv2.calcHist([im], [0, 1, 2], region_mask, \
bins, [0, 180, 0, 256, 0, 256])
hist = cv2.normalize(hist, np.zeros(hist.shape[:1], \
dtype="float")).flatten()
descriptor += list(hist)
return np.array(descriptor)
```

+   5. Implement the cosine_cdist() method to compute the cosine distances between the query image and the search images database:

```
def cosine_cdist(self, descriptor):
descriptor = descriptor.reshape(1, -1)
return cdist(self.search_ds, descriptor, 'cosine').reshape(-1)
```

+   6. Implement the store_ds() method for storing the search image descriptors in an efficient data structure:

```
def store_ds(self, descriptors):
#self.search_ds = BallTree(descriptors)
self.search_ds = descriptors
```

+   7. Implement the query_ds() method to query the search image data structure with the query image descriptor and return the top k most similar images according to the cosine distance measure:

```
def query_ds(self, descriptor, k):
img_distances = self.cosine_cdist(descriptor)
return np.argsort(img_distances)[:k].tolist()
```

+   8. Implement the build_index() method to build an index on the search image database, that is, create a data structure for efficient storage (with store_ds()) and retrieval (with query_ds()) of the search images according to the similarity to the query image:

```
def build_index(self):
t_before = time.time()
descriptors = np.array([])
search_pat = self.search_dir + '*.jpg'
i = 0
for imfile in glob(search_pat):
print(i, imfile)
im = self.read_preprocess(imfile)
descriptor = self.compute_descriptor(im)
descriptors = np.hstack((descriptors, descriptor)) if i=0
```