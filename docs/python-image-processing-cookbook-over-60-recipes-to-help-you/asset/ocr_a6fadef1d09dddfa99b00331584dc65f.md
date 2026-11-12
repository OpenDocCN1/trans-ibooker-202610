```
graph_def = None
with gfile.FastGFile(model_path, 'rb') as f:
    graph_def = tf.GraphDef()
    graph_def.ParseFromString(f.read())
if graph_def is None:
    raise RuntimeError('Cannot find inference graph in tar \\
    archive.')
with graph.as_default():
    tf.import_graph_def(graph_def, name='')

sess = tf.Session(graph=graph)
width, height = image.size
resize_ratio = 1.0 * input_size / max(width, height)
target_size = (int(resize_ratio * width), \\
                int(resize_ratio * height))
resized_image = image.convert('RGB').resize(target_size, \\
    Image.ANTIALIAS)
batch_seg_map = sess.run(
    output_tensor_name,
    feed_dict={input_tensor_name: [np.asarray(resized_image)]})
seg_map = batch_seg_map[0]
return resized_image, seg_map
```

# 2. Define the following function to create a pascal label, colormap:

```
def create_pascal_label_colormap():
    colormap = np.zeros((256, 3), dtype=int)
    ind = np.arange(256, dtype=int)
    for shift in reversed(range(8)):
        for channel in range(3):
            colormap[:, channel] |= ((ind >> channel) & 1) << shift
        ind >>= 3
    return colormap
```

# 3. Next, define a function to convert the labels of the segmented image to the colors from the desired colormap:

```
def label_to_color_image(label):
    colormap = create_pascal_label_colormap()
    if np.max(label) > len(colormap):
        raise ValueError('label value too large.')
    return colormap[label]
```