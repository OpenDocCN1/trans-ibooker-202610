```
>>> num_epochs = 10
>>> for t in range(num_epochs):
>>>     print(f"Epoch {t+1}\\n-----------------------------------")
>>>     train(model, loaders, verbose=False)
>>>     test(model, loaders, model)
>>> print("Done!")
Epoch 1
-----------------------------------
Test accuracy: 0.911
Epoch 2
-----------------------------------
Test accuracy: 0.943
Epoch 3
-----------------------------------
Test accuracy: 0.959
Epoch 4
-----------------------------------
Test accuracy: 0.967
Epoch 5
-----------------------------------
Test accuracy: 0.973
Epoch 6
-----------------------------------
Test accuracy: 0.974
Epoch 7
-----------------------------------
Test accuracy: 0.975
Epoch 8
-----------------------------------
Test accuracy: 0.977
Epoch 9
-----------------------------------
Test accuracy: 0.981
Epoch 10
-----------------------------------
Test accuracy: 0.980
Done!
```

By training for ten epochs, the accuracy now improves up to 98%!

# 4.4 More on generalization

Earlier, we mentioned that the SGD is widely used in deep neural network models due to its good generalization performance. We did not explicitly use any regularization in the process. Thus, the SGD algorithm seems to pose an implicit regularization effect, apart from the broad model class of neural networks and the particular input data type (i.e., structured image data). The implicit regularization from SGD would promise a further decrease in test error even when the training error reaches zero, thus leading to the “double descent” in the test loss curve. The effect of implicit regularization is recently studied by Smith et al. (2021) in their paper “On the Origin of Implicit Regularization in Stochastic Gradient Descent”, which we will discuss in a later chapter.

Now let us examine this property more closely. The code listing below defines a simple neural network with just one fully connected hidden layer. Setting a simple architecture makes it easy to focus on the performance impact when we change the model complexity,