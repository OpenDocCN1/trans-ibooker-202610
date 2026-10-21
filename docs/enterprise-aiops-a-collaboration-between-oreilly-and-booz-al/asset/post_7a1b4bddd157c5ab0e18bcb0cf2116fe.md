
holistic data governance, focused not only on creating a structure to regulate the storage, handling, and responsible use of data but also to facilitate maximizing data’s overall usefulness.

An effective governance process ensures that subject matter experts—along with other technical, business, and regulatory compliance stakeholders—provide guidance to align efforts and meet your organization’s enterprise-wide strategic goals.

Data governance priorities include:

- Creating policies and procedures tailored to support data needs for AI at enterprise scale
- Addressing data acquisition policies and procedures to include legal processes, agreements, and data brokers/data services
- Empowering key stakeholders to facilitate and lead your organization’s data governance
- Developing data security plans and data privacy policies in accordance with appropriate regulatory frameworks (e.g., Health Insurance Portability and Accountability Act [HIPAA], Federal Information Security Modernization Act [FISMA], European Union’s General Data Protection Regulation [GDPR], etc.)
- Prioritizing metadata management to ensure compliance and troubleshooting support

1 For definitions on data availability and accessibility, see the University of Delaware’s page on managing data availability.

2 “Accuracy” term definition comes from a CDC report on data quality. Further definitions of timeliness can also be found in this report.

3 For further discussion on data usability’s importance for AI, see An important aspect of big data: Data usability" by J. Li and X. Lui.

4 Further definitions of quality can be found in this article from DataCleaner.org.

5 “Provenance” term definition comes from W2C Incubator Group Report, *Provenance XG Final Report*, December 8, 2010.

6 For further information on data security, see [Gartner’s definition](https://www.gartner.com).

7 [“DataOps”](https://www.gartner.com), Gartner, accessed July 20, 2021.

8 For further reading on Kappa and Lambda architectures, see [Iman Samizadeh’s article on TowardsDataScience](https://towardsdatascience.com).

9 Shorten, C., Khoshgoftaar, T.M. “A Survey on Image Data Augmentation for Deep Learning.” *J Big Data* 6, 60 (2019). https://doi.org/10.1186/s40537-019-0197-0

## Chapter 5. Machine Learning (ML)

AI relies on large volumes of historical data and advanced ML methods to generate insights. In the last chapter, we focused on data’s importance to your organization’s success when implementing AI. If data is the fuel that powers your AI applications, then ML is its engine.

The previous two chapters discuss how to coordinate AI analytic development with DataOps and key stakeholders, such as data owners and risk managers. This section will discuss AI development itself and how its tooling and process can evolve toward a more sustainable, scalable, and coordinated capability.

When considering how ML fits within the wider AI domain, it’s important to first understand the data analytics spectrum. Figure 5-1 illustrates the differentiation between conventional statistical analysis (i.e., forensic analytics, which examines past and present phenomena) and anticipatory analytics (i.e., forward-looking analysis).

![](img/53a28dd44bbae66341b0fe693bd59916_57_0.png)

Figure 5-1. Complexity versus explainability for analyses

Each analytic approach is associated with specific challenges, with more advanced approaches providing the decision support and cognitive analysis typically associated with robust AI capabilities. A key distinction is the interplay between complexity and explainability. In most cases, forensic analysis is far less complex and more explainable than anticipatory analytics, which is rapidly becoming incredibly complex and far more difficult to explain and can lead to lower explainability. That being said, industry continues to invest heavily and advance the capabilities to explain these complex models.

In practice, ML often serves as the primary mechanism driving AI capabilities, leveraging data at an enterprise-wide scale to build and validate complex models. Managing complexity and explainability, as discussed in Chapters 2 and 3, is a key design consideration with ML (that guides decisions like training algorithm, parameters, etc.).

Then again, many of the most popular AI applications we are beginning to see in our daily lives—smart assistants, real-time object tracking, and self-
driving cars, to name a few—are powered by ML, neural networks, and deep learning capabilities that are more difficult to explain and manage.

### Neural network

A series of algorithms that attempt to identify underlying relationships in a set of data by using a process that mimics the way the human brain operates.

### Deep learning

A specialized collection of machine learning approaches leveraging neural networks to enable AI capabilities such as computer vision and natural language processing.

In this section, we introduce the concept of an ML model and distinct ML frameworks to address specific data structures and analytic objectives. This section isn’t intended to be a functional “how-to” for ML, but a solid working knowledge will help you better lead AI adoption within your organization.

### What Is an ML Model?

Business analytics, often referred to as simply “analytics,” are business rules encoded into software by an analyst or developer that subsequently converts data into useful insights to inform decision making. By comparison, ML models use learning algorithms to generate business rules using prepared training data.

### Training data

An initial, usually large, dataset used to teach a ML model to be predictive or to learn a desired task. This may include inputs (raw data) and outputs (labels) or unlabeled data.

For supervised and reinforcement learning techniques, these learned rules are encoded in a special file known as a model. Because they are designed to be written and interpreted by machines, these models are much harder to read (explain) than business analytics.

Unlike conventional analytics, the act of feeding user data into the analytic and producing insights is known as inference. Updating ML analytics is accomplished with some combination of the original training data, new/updated data, and the trained model.

Traditionally, organizations treat ML models as black boxes where only their inputs and outputs required explanation. For all the reasons described in Chapter 3, mature organizations are abandoning this black box approach in favor of explainable outcomes.¹ From a leadership standpoint, this shift toward explainability will continue to change how results are documented in the short term and overall design processes in the long term.

ML models are also unique in that similar models and data can produce radically different results. Identical source code is compiled into identical applications, but identical training data produces a variety of models, depending on training parameters. Since it is difficult to predict the best configuration in advance, it is often best practice to train with many different configurations and then compare performance for each of the results during training.

This is especially true for more sophisticated ML approaches, such as neural networks, and accounts for part of these techniques’ reputation for large computational and data storage overhead. Additionally, even the smallest changes to training data can produce meaningful changes to the end result.

These trends result in the potential for hundreds or even thousands of individual ML model versions. Only a small number will be operational, but many (or all, depending on your goals) are worth preserving to provide additional insight into the training process. You should choose how much data and the number of models you’d like to retain based on how explainable the analysis must be and how tightly you can control the operational and training data inputs.

As with any other form of software, it is important for ML models to be documented, tested, and version-controlled to help the team maintain knowledge of its structure and functionality. Unlike conventional analytics, it’s not possible to break up, document, or independently test a model’s internal structure. Instead, we must rely on surrounding documentation, testing, and metadata. While not entirely unique to ML analytics, metadata plays a much larger role in describing a model’s behavior than it does with normal (or classic) software. MLOps (discussed in “Machine Learning Operations (MLOps)”) can help with this documentation, testing, and version control.

DataOps provides the data pipelines to develop and train ML models. Then, MLOps provides an integrated and automated process with a documented CI/CD process to integrate these models into larger software applications that leverage said models using DevSecOps. This approach ensures resulting software applications that utilize integrated models are secure and that data scientists or software engineers can rapidly update the software and models as required. These combined frameworks allow data science practitioners to continuously refine models and their overall approach to analytics.

### ML Methodologies

Much like selecting the right tool for a given job, it’s important to understand the algorithms supporting our analytic efforts. Particularly in the case of ML, we use these underlying processes to characterize and define each unique ML approach. By understanding these techniques, we can address any assumptions and identify appropriate performance evaluation metrics.
