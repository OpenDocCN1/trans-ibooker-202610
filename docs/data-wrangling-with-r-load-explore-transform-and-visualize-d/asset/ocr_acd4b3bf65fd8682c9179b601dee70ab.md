# Feature Correlation: Titanic

Linear feature correlation is based on Pearson's correlation. Numeric to categorical correlation is calculated by encoding the categorical features as the floating point numbers that best predict the numeric feature before calculating Pearson's correlation. Linear categorical to categorical correlation is not supported.

Numeric to numeric correlation is in the range [-1, 1] where 0 implies no correlation, 1 implies perfect correlation and -1 implies perfect inverse correlation.

Numeric to categorical and categorical to categorical correlations are in the range [0, 1] where 0 implies no correlation and 1 implies perfect correlation

Features that are not either numeric or categorical are ignored.

The table below lists for each feature what is the most correlated feature to it. We display a correlation matrix for a dataset with up to 20 columns

|  | Most correlated feature | Correlation |
|---|---|---|
| survived (numeric) | boat (categorical) | 0.950845 |
| boat (categorical) | survived (numeric) | 0.950845 |
| pclass (numeric) | boat (categorical) | 0.608416 |
| fare (numeric) | pclass (numeric) | -0.565255 |
| sex (numeric) | boat (categorical) | 0.556174 |
| age (numeric) | pclass (numeric) | -0.411391 |
| sibsp (numeric) | parch (numeric) | 0.374291 |
| parch (numeric) | sibsp (numeric) | 0.374291 |
| embarked (numeric) | boat (categorical) | 0.278391 |

Correlation matrix:

![](bbox=[0.28, 0.36, 0.78, 0.52])

Figure 4.5 – Data analysis report: feature correlation

# Data export

So far, we’ve looked at Data Wrangler capabilities that enable you to import data into Data Wrangler and perform analysis and transformations. SageMaker Data Wrangler enables you to export all or part of these transformations as a data flow. In most cases, data processing consists of a series of transformations. Each of these transformations can be referred to as a step in Data Wrangler. A Data Wrangler flow is made up of a series of nodes that represent the import of your data and the transformations that you’ve performed. As we covered earlier, one of the first steps in Data Wrangler is to import data from a supported data source. As such, the data source is the first node in your data flow. Following the previous step, the next node in the data flow is the Data Types node. This node signifies that Data Wrangler has executed a transformation to convert the dataset into a format that is suitable for further analysis and processing. Each transformation that is added to the data flow is represented as a new node, enabling a visual representation of the sequence of operations performed on the dataset. Think of this data flow as a code file that includes all the code and sequences of your