performance can be improved over time.

# MPM with Explainability for Full Coverage

A high-performing ML model with around 80–90% accuracy might still have hidden biases and underrepresented groups in the data. In order to detect hidden biases in data, teams need to analyze the features being used and the distribution of data in those features. This should take place during the feature selection and model training stages, and usually leads to these issues being caught early on. However, as time goes on and models are set up to be automatically retrained, bias can start to creep in as certain groups of data become more or less prevalent. In order to account for this, an MPM framework needs to monitor data distributions against a baseline to catch any arising drift that might cause bias.

Drift can occur due to the data being used to make the predictions changing or the data being used to train the model changing. In either case, MPM needs to track the distribution of data and account for the changes between what the model is expecting and what it is seeing. Any deviation above what is normal should be flagged for investigation and debugging. Chapter 2 will go into explainability in more detail and talk about the philosophy of Explainable AI.

# Benefits of Using an MPM Framework

As mentioned previously, the MPM framework aims to bring visibility into the machine learning life cycle. It addresses several of the operational challenges in MLOps, such as inconsistent performance, loss of control, and lack of a feedback loop. But what does that mean for a business? Let’s take a look at some key business benefits that the MPM framework provides.

# MPM validates models before pushing them live

Before deploying a model, it is crucial to validate it, whether required by law or not. This ensures that at launch, you have a high-performing, well-tested, and robust model. Since the model will have been built around a