# Architecture

ML algorithms can take many forms, which are easier or harder to interpret; they range from simple logistic regression and decision trees to advanced neural networks for deep learning.

# Variety

ML models come in many varieties (tabular, time series, text, image, video, and audio), and often these modalities are mixed for certain domains, further adding to the complexity of tracking the performance of the model.

# Volume

The rise of cloud computing has enabled teams to train multiple models with more data in parallel, which makes it harder to keep track of the training data.

Teams must be able to monitor sudden changes in data distributions that might lead to bias. Other issues might arise from the model being trained on too little or too much data, making it underperform. Tracking all the parameters that can affect a model’s performance is much harder than tracking code changes in a repository.

The metrics often used to track model performance are:

# Accuracy

The percentage of total predictions that were actually correct

# Precision

The percentage of positive cases that were correctly identified

# Negative predictive value

The percentage of negative cases that were correctly identified