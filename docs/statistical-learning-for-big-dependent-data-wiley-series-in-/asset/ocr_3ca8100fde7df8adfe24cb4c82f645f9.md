Non-trainable params: 0

```
> ## compile the model
> mod1 %>% compile(
+   loss="binary_crossentropy",
+   optimizer=optimizer_rmsprop(lr=0.00025),
+ )
```

```
> ## fit
> history2 <- mod1 %>% fit(
+   Z_train, R_train, batch_size=32, epochs=30, verbose=1,
+   validation_split=0.1
+ )
```

Train on 3600 samples, validate on 400 samples
Epoch 1/30
3600/3600 [==============================] - 2s 529us/sample - loss: 0.6861 - val_loss: 0.6797
.....
Epoch 30/30
3600/3600 [==============================] - 1s 337us/sample - loss: 0.0144 - val_loss: 0.7202

```
> plot(history2)
`geom_smooth()` using formula 'y ~ x'
```

```
> mod2 %>% evaluate(Z_train, R_train, batch_size=32)
4000/4000 [==============================] - 0s 58us/sample - loss: 0.0748
loss
0.07476683
```

```
> Rhat <- mod1 %>% predict_classes(Z_test, batch_size=32)
> table(R_test, Rhat)
> mean(R_test == Rhat)
```

# ## Network specification: LSTM network

```
> mod2 <- keras_model_sequential()
> mod2 %>
+   layer_embedding(500, 32, input_length=100) %>
+   layer_reshape(target_shape=c(100, 32), input_shape=c(100)) %>
+   layer_dropout(rate=0.25) %>
+   layer_lstm(units=32) %>
+   layer_dense(units=256, activation="relu") %>
+   layer_dense(units=1, activation="sigmoid")
```

```
> summary(mod2)
Model: "sequential_10"
```

| Layer (type) | Output Shape | Param # |
|---|---|---|
| embedding_10 (Embedding) | (None, 100, 32) | 16000 |
| reshape_9 (Reshape) | (None, 100, 32) | 0 |
| dropout_18 (Dropout) | (None, 100, 32) | 0 |
| lstm (LSTM) | (None, 32) | 8320 |
| dense_19 (Dense) | (None, 256) | 8448 |