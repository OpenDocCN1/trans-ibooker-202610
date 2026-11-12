# # Data Engineering

During experimentation and model development, you may have encountered data quality issues or missing data. It's important to report any such issues back to the source system or data owners, because you don't want to see these problems pop up in other places. Another recommendation is to develop data pipelines that automate the process of using data products to retrain models in production. For this, you may want to implement some controls for data drift detection (decreasing model performance) and schema changes. Once you're done, and your data pipeline is built, you should register your newly created dataset as an official artifact in your machine learning environment. This accelerates model development and takes away the pain of dealing with configuration, such as folder locations and user credentials.

Another part of the data engineering phase is developing a feature store. A feature store is a centralized repository where you standardize the definition, storage, and features for model training and deployment.19 This has a few important benefits:

+   * It makes the process of creating features much more streamlined and efficient, because developers can discover and reuse features, instead of re-creating the same or similar ones.
+   * It reduces costs by lowering complexity and improving performance. For example, suppose you have a feature that is computationally expensive and is used in multiple machine learning projects. Rather than using a transform function and storing the transformed feature in multiple training datasets, it's much more efficient and maintainable to store it in a shared repository. The same applies when preprocessing or building features takes a long time, or when features are time-dependent or depend on other features.

New deliverables added to the list of artifacts after you've finished the data engineering phase may include:

+   * An updated solution design. This is a diagram or description of your architecture, including sources, data pipeline(s), integration patterns, and so on.
+   * New or updated data pipelines for data collection and cleansing.
+   * An updated code repository that contains all the production-ready artifacts.
+   * A record of data quality issues or gaps that are reported back to data owners.