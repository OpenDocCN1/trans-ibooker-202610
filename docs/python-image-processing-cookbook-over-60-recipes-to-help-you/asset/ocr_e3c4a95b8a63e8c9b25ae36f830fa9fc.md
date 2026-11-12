```
end_points*(1-percent), np.int32)
def weighted_average(img1, img2, percent=0.5):
    if percent <= 0: return img2
    elif percent >= 1: return img1
    else: return cv2.addWeighted(img1, percent, img2, \
    1-percent, 0)
```

# 5. Implement a bilinear_interpolate() function, which interpolates over
every image channel:

```
def bilinear_interpolate(image, coords):
    int_coords = coords.astype(np.int32)
    x0, y0 = int_coords
    dx, dy = coords - int_coords
    q11, q21, q12, q22 = image[y0, x0], image[y0, x0+1], \
    image[y0+1, x0], image[y0+1, x0+1]
    btm = q21.T * dx + q11.T * (1 - dx)
    top = q22.T * dx + q12.T * (1 - dx)
    interpolated_pixels = top * dy + btm * (1 - dy)
    return interpolated_pixels.T
```

# 6. Implement the get_grid_coordinates() function to generate an array of all
possible (x, y) grid coordinates within the Region Of Interest (ROI) of input
points:

```
def get_grid_coordinates(points):
    xmin, xmax = np.min(points[:, 0]), np.max(points[:, 0]) + 1
    ymin, ymax = np.min(points[:, 1]), np.max(points[:, 1]) + 1
    return np.asarray([(x, y) for y in range(ymin, ymax)
    for x in range(xmin, xmax)], np.uint32)
```

# 7. Implement the process_warp() function to warp the triangles
from src_img—the ones that are inside the ROI of result_img (corresponding
to the points in dst_points):

```
def process_warp(src_img, result_img, tri_affines, dst_points,
    delaunay):
    roi_coords = grid_coordinates(dst_points)
    roi_tri_indices = delaunay.find_simplex(roi_coords)
    for simplex_index in range(len(delaunay.simplices)):
    coords = roi_coords[roi_tri_indices == simplex_index]
    num_coords = len(coords)
    out_coords = np.dot(tri_affines[simplex_index],
    np.vstack((coords.T, np.ones(num_coords))))
    x, y = coords.T
    result_img[y, x] = bilinear_interpolate(src_img, \
```