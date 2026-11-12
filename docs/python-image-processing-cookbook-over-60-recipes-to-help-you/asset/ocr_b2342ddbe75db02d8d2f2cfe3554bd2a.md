+   3. You just need to provide four pairs of matching points between the source and destination images to estimate the homography matrix, H, automatically for you. Here, the four corners of the destination image and the four corners of the input hummingbird are provided as matching points, as shown in the following code block:

```
src = np.array([[ 295., 174.],
   [ 540., 146. ],
   [ 400., 777.],
   [ 60., 422.]])
dst = np.array([[ 0., 0.],
   [height-1, 0.],
   [height-1, width-1],
   [ 0., width-1]])
```

+   4. Obtain the source pixel index corresponding to each pixel index in the destination:

```
x, y = np.mgrid[:height, :width]
dst_indices = np.hstack((x.reshape(-1, 1), y.reshape(-1,1)))
src_indices = np.round(pt(np.inverse(dst_indices), 0).astype(int)
valid_idx = np.where((src_indices[:,0] < height) &
                   (src_indices[:,1] < width) &
                   (src_indices[:,0] >= 0) & (src_indices[:,1] >=
                   0))
dst_indices_valid = dst_indices[valid_idx]
src_indices_valid = src_indices[valid_idx]
```

+   5. Copy pixels from the source to the destination images:

```
im_dst[dst_indices_valid[:,0],dst_indices_valid[:,1]] =
im_src[src_indices_valid[:,0],src_indices_valid[:,1]]
```

If you run the preceding code snippets, you will get an output like the following screenshot:

![](bbox=[0.2, 0.71, 0.79, 0.89])