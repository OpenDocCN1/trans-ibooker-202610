```
test_image = os.listdir(input_dir_cnn)[0]
test_image = os.path.join(input_dir_cnn, test_image)
**print**(test_image)
display(Image.open(test_image))
...
cars_data/images/input_images_cnn/01994.jpg
```

![](bbox=[0.356, 0.219, 0.64, 0.428])

Figure 10-3. Randomly picked car image from our dataset

First, we will need to convert the input image into a vector format. We use our Img2VecResnet18 class to do that.

```
img2vec = Img2VecResnet18()
I = Image.open(test_image)
test_vec = img2vec.getVec(I)
I.close()
```

```
**print**(len(test_vec))
**print**(test_vec)
```

...

```
512
[5.64089492e-02 2.70972490e+00 2.15519500e+00 1.43926993e-01
 2.47581363e+00 1.36641121e+00 1.08204508e+00 7.62105465e-01
 [...]
 5.62133253e-01 4.33687061e-01 3.95899676e-02 1.47889364e+00
 2.89110214e-01 6.61322474e-01 1.84713617e-01 9.42268595e-02]
...
```

```
test_vector = Vectors.dense(test_vec)
```

Now we perform an approximate nearest neighbor search.