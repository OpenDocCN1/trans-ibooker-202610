![](bbox=[0.226, 0.102, 0.725, 0.358])

Figure 8.15 Convergence of the RNN for the IMDB data set in the training subsample, which consists of the first 4000 reviews.

resulting LSTM network has 33,025 parameters, which is much smaller than that of the RNN. We use the same loss function and criterion, but only train the LSTM network for 10 epochs. Figure 8.16 shows the convergence of the LSTM network in the 10 epochs. In this case, both the in sample and cross-validation curves continue to decrease, indicating that we can either train the network a bit longer or use a larger cross validation rate. We experiment with a few runs, but the out-of-sample

![](bbox=[0.226, 0.575, 0.725, 0.845])

Figure 8.16 Convergence of the RNN for the IMDB data set in the training subsample, which consists of the first 4000 reviews.