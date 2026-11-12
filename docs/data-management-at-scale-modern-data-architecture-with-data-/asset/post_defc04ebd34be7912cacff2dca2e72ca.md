
steps, from data collection to data validation, model training, model validation, and model deployment.

Depending on your specific use case, your machine learning project might look different. For example, you might train, evaluate, and deploy multiple models in the same pipeline, or your project might be a one-off analysis, where analytical insights are shared with the business but no models are deployed in production. Your starting point may vary too; for example, if data isn’t already available, you first must go through the steps of data collection and onboarding.

To help you better understand what MLOps is about, I’m going to discuss each step in detail. While doing so, I’ll share best practices and describe the deliverables from each stage. At the end, I’ll discuss some variations and exceptions.

## Initiating a Project

The first phase of the MLOps project (the “initiate process” step in Figure 11-5) is devoted to understanding your business requirements and data needs, and planning the implementation and development work to be carried out in subsequent stages. It sets the foundation for the later stages. You start by defining your business objectives and formulating your success criteria, which define under what conditions you will move on to the next phase. For collaboration between team members, I recommend using DevOps tooling. For example, you could use a kanban board to manage and track your activities throughout the life cycle of your project. One of these activities might be to identify your data sources and determine whether data products already have been developed.

A good practice when starting a machine learning project is to create a new code repository in which all (future) artifacts will be stored and managed. A general best practice is to segment use cases from one another, so each time you create a new ML project, you should create a new repository. To do this efficiently, consider standardizing on a code template—this will allow for code reuse, and decrease the ramp-up time at project start or when a new team member joins the project.

When you initiate a new project, you typically produce several artifacts:

- A project document or wiki covering the business requirements, success criteria, ethical dilemmas, etc.
- A list of data sources and additional requirements that must be applied
- A team board, which holds the initial activities
- A new code repository using templates to ramp up the project development

These artifacts help your project team to track progress. They also help to share insights for other future projects. Once the business objectives are clear, you enter the next phase: experimentation and tracking.

## Experimentation and Tracking

After you’ve defined your goals and ambitions and set up your project, it’s time for experimentation. During this process, I recommend tracking all of your experiment results. Thus, each time you train and run a model, you should capture all the parameters, metrics, algorithms, and other artifacts, as well as the output. This enables you to do the following:

- Gather and organize all the elements needed to conduct the experiment.
- Reproduce any results using saved experiment data.
- Log iterative improvements across time, data, frameworks, models, users, etc.
- Prove to regulators how your models were developed, what algorithms were chosen, and what datasets were used as input.

For experiment tracking, there are many tools to choose from. A popular framework is MLflow; it’s free and open source and is used by many large vendors, including AWS, Databricks, Google, and Microsoft.

When performing experiments, the first step is to determine what compute infrastructure and environment you need.16 A general best practice is to start fresh, using a clean development environment. Keep track of everything you do in each experiment, versioning and capturing all your inputs and outputs to ensure reproducibility. Pay close attention to all data engineering activities. Some of these may be generic steps and will also apply for other use cases. Finally, you’ll need to determine the implementation integration pattern to use for your project in the production environment. This should be one of the following:

## Model as an API

In this approach, the model is deployed as a web service so it can be used by other applications and processes. An API call has to be made in order to get predictions from the model.

## Model as batches in/out

In this approach, the model works with batches of input and outputs mini-batches or batches of predictions. Chunks or subsets of the data with labels are used for training, predicting, and making recommendations. The input and output typically consists of a set of files (e.g., CSVs or Parquet files).

## Model as stream

In this approach, the model interacts reactively with a data stream, where the data segments arrive in increments. If extra data is needed, the model can be allowed to read the DDS's database directly. The model can generate and publish new events as well.

During the experimentation phase of your project, it's common to have manual processes. Your goal is to confirm your hypothesis by validating your assumptions. At the end of this stage, the project might end as a one-off exercise, where the learnings are shared. Alternatively, you might see sustainable business value. In this case, your next step will be to continuously deliver value. You'll do this by automating the entire process of data collection, data validation, model training, model validation, and deployment. You'll learn more about this after we've discussed the data engineering activities.

By the end of the experimentation stage, you'll have several new deliverables. These may include:

- A project document covering the outcome and criteria for model operationalization. This document also contains a model report: for each model that's tried, a standard, template-based report that provides details on the experiment is produced.
- New objects in the analytical workspace:17 environments, models, datasets, and experiments, including all logging and metrics.
- A feature report: a document that contains pointers to the code used to generate new features. You may want to classify what code is generic and can be used for other use cases, what features are trained on potentially personal sensitive data, etc.18
- An updated code repository or code templates for new projects.
- A list of other improvements that can be made (e.g., adjustments to environments or new compute infrastructure options).

17 A workspace is the top-level resource in which your artifacts are managed.

18 Features are the columns of data in your input data. For example, if you're trying to predict someone's age, your input features might be things like height and hair color. The label is the final output: 12 years, 78 years, and so on.

## Data Engineering

During experimentation and model development, you may have encountered data quality issues or missing data. It's important to report any such issues back to the source system or data owners, because you don't want to see these problems pop up in other places. Another recommendation is to develop data pipelines that automate the process of using data products to retrain models in production. For this, you may want to implement some controls for data drift detection (decreasing model performance) and schema changes. Once you're done, and your data pipeline is built, you should register your newly created dataset as an official artifact in your machine learning environment. This accelerates model development and takes away the pain of dealing with configuration, such as folder locations and user credentials.

Another part of the data engineering phase is developing a feature store. A feature store is a centralized repository where you standardize the definition, storage, and features for model training and deployment.19 This has a few important benefits:

- It makes the process of creating features much more streamlined and efficient, because developers can discover and reuse features, instead of re-creating the same or similar ones.
- It reduces costs by lowering complexity and improving performance. For example, suppose you have a feature that is computationally expensive and is used in multiple machine learning projects. Rather than using a transform function and storing the transformed feature in multiple training datasets, it's much more efficient and maintainable to store it in a shared repository. The same applies when preprocessing or building features takes a long time, or when features are time-dependent or depend on other features.

New deliverables added to the list of artifacts after you've finished the data engineering phase may include:

- An updated solution design. This is a diagram or description of your architecture, including sources, data pipeline(s), integration patterns, and so on.
- New or updated data pipelines for data collection and cleansing.
- An updated code repository that contains all the production-ready artifacts.
- A record of data quality issues or gaps that are reported back to data owners.
