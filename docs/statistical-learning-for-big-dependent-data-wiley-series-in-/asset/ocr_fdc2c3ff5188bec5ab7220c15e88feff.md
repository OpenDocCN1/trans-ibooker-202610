| Layer | Output Shape | Params |
|---|---|---|
| flatten (Flatten) | (None, 4608) | 0 |
| dense (Dense) | (None, 128) | 589952 |
| dropout_1 (Dropout) | (None, 128) | 0 |
| dense_1 (Dense) | (None, 10) | 1290 |
| Total params: 600,810 | | |
| Trainable params: 600,810 | | |
| Non-trainable params: 0 | | |

```
> mod %>% compile(
+     loss = "categorical_crossentropy",
+     optimizer = optimizer_rmsprop(),
+     metrics = list("accuracy")
+ )
> history1 <- mod %>% fit(
+     X_train,
+     Y_train,
+     epochs = 5,
+     batch_size=32,
+     verbose=1,
+     validation_split=0.1
+ )
Train on 40000 samples, validate on 6000 samples
Epoch 1/5
54000/54000 [==============================] - 102s 2ms/sample -
loss: 0.1526 - acc: 0.9524 - val_loss: 0.0497 - val_acc: 0.9858
....
Epoch 5/5
54000/54000 [==============================] - 113s 2ms/sample -
loss: 0.0511 - acc: 0.9852 - val_loss: 0.0427 - val_acc: 0.9887
> plot(history1)
> mod %>% evaluate(X_train, Y_train, batch_size=32)
$loss
[1] 0.03316241
$acc
[1] 0.99025
> Yhat <- mod %>% predict_classes(X_test, batch_size=32)
> table(Y_test, Yhat)
> mean(Y_test == Yhat)
```

By comparing the two networks used in this section, we see that the convolution networks can be useful in classification. This seems reasonable in this particular application because the neighboring cells of the 28×28 matrices contain information concerning the handwritten digits. In fact, a majority of the cells in the matrix would be empty (or zero in the gray level), which carries no information.

**Remark.** The package kerasR is an interface between keras and users. It simplifies model specification in keras. Unfortunately, there exist some problems in reading integers with the subcommand input_shape in layer specification. We use keras in this chapter.