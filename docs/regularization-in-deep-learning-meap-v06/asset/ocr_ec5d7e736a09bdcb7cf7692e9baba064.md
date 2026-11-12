![](bbox=[0.265, 0.138, 0.892, 0.478])

Figure 7.12 Training and test set MSE using the single-layer FCN model.

Now, let us activate the L2 penalty by setting it to 0.1 and observe the impact. Feel free to adjust this hyper-parameter and explore the results using the accompanying notebook at https://github.com/jackliu333/regularization_in_deep_learning/blob/main/Chapter_7.ipynb. As shown in figure 7.13, adding the L2 penalty has no obvious change to the training loss curve but results in a marked reduction in the test set MSE across different epochs. Having a controlled magnitude over the model weights thus helps the model generalize better to the test set.