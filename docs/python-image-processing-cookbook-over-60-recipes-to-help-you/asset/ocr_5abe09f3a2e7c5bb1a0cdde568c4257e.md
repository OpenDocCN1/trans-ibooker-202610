# Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: https://static.packt-cdn.com/downloads/ 9781789537147_ColorImages.pdf.

# Conventions used

There are a number of text conventions used throughout this book.

CodeInText: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "Implement a bilinear_interpolate() function, which interpolates over every image channel."

A block of code is set as follows:

```
def get_grid_coordinates(points):
    xmin, xmax = np.min(points[:, 0]), np.max(points[:, 0]) + 1
    ymin, ymax = np.min(points[:, 1]), np.max(points[:, 1]) + 1
    return np.asarray([(x, y) for y in range(ymin, ymax)
                       for x in range(xmin, xmax)], np.uint32)
```

Any command-line input or output is written as follows:

```
$ pip install mtcnn
```

**Bold**: Indicates a new term, an important word, or words that you see onscreen. For example, words in menus or dialog boxes appear in the text like this. Here is an example: "Face alignment is a data normalization process—an essential preprocessing step for many facial recognition algorithms."

![](bbox=[0.135, 0.663, 0.205, 0.722])

Warnings or important notes appear like this.

![](bbox=[0.135, 0.753, 0.205, 0.812])

Tips and tricks appear like this.