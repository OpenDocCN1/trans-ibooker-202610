
## Model development

![](img/53a28dd44bbae66341b0fe693bd59916_75_0.png)

A united approach to model development enables reduced cycle time, increased automation, and consistent support for explainability, reproducibility, and the application of ethical artificial intelligence concepts

Figure 5-8. Model development steps

Once the project objectives are set and the business problem is understood, an exploratory analysis is necessary to identify and collect all relevant data and assess their ability to meet the intended business purpose. This process is designed to create a comprehensive view of the available data and use analytic techniques to explore important relationships in the data.

Exploratory analysis also serves to identify deficiencies in the data that may prevent the model from meeting the intended business purpose. Data exploration analyzes the data we have available and also informs the type of data needed to develop a useful model.

The data preparation stage transforms data to be used with ML algorithms. You’ll want to check for issues such as missing information, duplicate/invalid data, or imbalanced data.

This stage also includes techniques to help prepare data, including: removing duplicate/invalid records from the dataset; addressing class imbalance (when the total number of a class of data [positive] is far less than the total number of another class of data [negative]); enrichment using additional data sources; and selecting data features and executing feature engineering (to include combining multiple features for additional training benefits). This process is designed to help determine the required input variables when developing a predictive model. Your subject matter experts are invaluable during this phase of model development.

*Model training* begins once data is prepared. In this step, training and validation data is used to compute the model parameters that optimize performance. Prior to initializing the model training process, it is necessary to define the model to be trained and its corresponding configurations. A general process for model training can typically be used across different algorithms.

For most types of ML, the general training procedure begins by randomly initializing the model and its associated parameters. This model is then trained using data to fine-tune it, generating and comparing predictions for each entry in the training dataset. Using these predictions, the model training computes error and other performance metrics between model prediction and ground truth for each entry (e.g., case, record, image). The computed error, often referred to as “loss,” is then used to adjust model parameters to optimize for desired metrics. The training script will continue to loop through the aforementioned steps until it achieves the desired performance or until the maximum number of training steps has been reached.

The next step, *model versioning*, is crucial to ensure a workflow is explainable and can be replicated. In this step, models are versioned based on the data used for training, selected specifications (e.g., learning rate, batch size, number of epochs),⁶ and the resulting output. An identifier should be assigned to each model to provide context. Versioning enables your team to track model lineage and pedigree, which will improve results as your project progresses.

Here are some items we recommend tracking during your development efforts: software used to train and run the model; development environment; model architecture; training data used; model parameters; and the artifacts collected during training, such as reports and log files. This process should be standardized and conducted for each model training event with parameters stored in a common repository.

The final step in development, *model validation*, is vetting a model’s training by using a validation dataset to determine if any additional performance adjustments are necessary. It’s important that this validation dataset not include any data used in the model training stage to ensure validation metrics are fully independent from the training procedure.

In the validation stage, the model you’ve created is used to make predictions for every entry in the test dataset. Prediction error and other model performance metrics are calculated to ensure observed model performance meets user-defined requirements before deploying the model.

## Model Vetting

The next step in the ML life cycle is to vet your model, as shown in Figure 5-9. Vetting a trained ML model is a crucial, yet underappreciated, step in the path to operationalizing machine learning. Rigorous model vetting enables the local data science team to discover costly bugs and correct them while still in a low-risk environment. Once vetting is executed locally, it is then replicated in the production environment. Deploying properly working models will build organizational trust, not only in the deployed system but in AI projects as a whole.

![](img/53a28dd44bbae66341b0fe693bd59916_78_0.png)

Figure 5-9. Model vetting steps

Trained models that meet initial performance requirements are *deployed for testing* to a production-like environment for a rigorous test and evaluation stage. This step involves conducting computational and performance testing to ensure the model meets requirements. For the sake of consistency, deploying a model to the test environment should parallel the steps taken to deploy a model to an actual production environment. Your development team should work closely with subject matter experts and functional users to ensure the test environment mirrors actual production.

Similar to model development stages, it is important to *verify results*. This step ensures model performance is in line with expectations and has not suffered from performance degradation. *It’s important to note that, based on our experience, most preproduction model issues are discovered during this step.* As noted previously, it’s crucial that the data sent to the model during this stage be representative of the data the model will analyze while in a production environment.

Part of the results verification process is exploring how the model handles malformed or otherwise improper data. The model should be able to handle improper data and ensure that important data is not lost, an appropriate error notification is sent to the software development team, and the model remains functional after encountering the error.

*Result tracking* is critical to ensure your model is effectively reviewed and audited. The ability to store and review historical model output allows subject matter experts and functional users to conduct periodic reviews, ensuring the model is making acceptable and appropriate predictions.

The final step, *staging for deployment*, prepares the model to be moved to the production environment. Once the model is deployed to the production environment, the model is considered “live” and will be available to all authorized end users and systems. Data scientists, software engineers, and all appropriate experts should be consulted to ensure the model is deployed in a manner that matches expectations in the production environment.

## Model Operation

The operationalizing ML workflow’s ultimate goal is the successful deployment and smooth operation of a well-functioning model. The ideal model provides end users with predictions when requested in a timely manner, performs automatic monitoring of model performance and usage, and notifies people of relevant model activity through reporting and alert mechanisms. In short, a successful deployment means that your ML application performs as expected and alerts you when your attention is needed. Model operations (shown in Figure 5-10) will allow you to meet these needs.

![](img/53a28dd44bbae66341b0fe693bd59916_80_0.png)

Figure 5-10. Model operation steps

The first step in operationalizing a model is deploying the model to the production environment and verifying that the model has been successfully transferred and no issues exist with running the model in the intended environment. The deployment verification is typically done by closely monitoring the model immediately after deployment to ensure it’s receiving, analyzing, and returning appropriate data. This step also involves verifying input data formatting is correct and analyzing output for correctness to ensure that no model performance degradation has occurred. These tests are best performed on the production hosting environment by the software engineering team immediately after deployment.

The operation step involves executing the model when required. During this step, the software engineering team must ensure all aspects of the deployed model are working as intended. Any system issues should be fixed based on their order of importance to ensure the model continues to operate as intended. This process typically requires using regularly scheduled automated tests of the system, validating model responsiveness, ensuring models are being retrained when necessary, and verifying that models are resilient against ingesting malformed or improper data. These events result in
