![](bbox=[0.12, 0.09, 0.88, 0.4])

Figure 10.11: The data insights summary

Let’s go over the different sections of the report, starting with the top summary. The summary includes key statistics of your dataset and the feature types. Here, we can see the total number of rows in your dataset, the number of missing rows, and the number of duplicate rows, if any. In the SUMMARY section, you can also review the number of features in your dataset and their count. If your dataset has critical errors that will significantly affect the quality of your machine learning models, they will display in the High Priority Warnings section of the report. An example of a high-priority warning is target leakage. Target leakage occurs when a feature is directly influenced by the target, resulting in high accuracy on the training set but poor performance on test data. To prevent target leakage, it’s important to keep the target out of the training process, ensuring that we train the model only on independent features.

![](bbox=[0.12, 0.67, 0.88, 0.8])

Figure 10.12: Data insights anomalies

The next section of the report shows any anomalies that may be present in our data. Data Wrangler detects anomalous samples using the Isolation Forest algorithm after basic preprocessing. The isolation forest algorithm detects anomalies based on how far a data point is from the rest of the data.