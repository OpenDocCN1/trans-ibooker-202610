We recommend that you refer to the *Further reading* section for more information.

# Pricing

Cloud IoT Core is charged according to the data volume that's used per calendar month. Google recommends using the pricing calculator to estimate the price according to the volume of data that's exchanged.

# Data Fusion

Cloud Data Fusion is a fully managed and cloud-native data integration service for quickly building and managing data pipelines. Data Fusion uses Dataproc as the execution environment for these pipelines. The GUI caters to a variety of users, which means that business users, developers, and data scientists can easily build integration solutions that will cleanse, prepare, and transform data. Data fusion also offers a library of preconfigured plugins to extend its capabilities. It is also important to note that it is powered by an open source project called **Cask Data Application Platform (CDAP)**.

# Core concepts

Some core Data Fusion concepts are worth highlighting to understand the product and how to use it.

# Instances

To begin with, we must create a Cloud Data Fusion instance. Instances run as the Compute Engine service account and Data Fusion executes pipelines using a Dataproc cluster. Instances are a unique deployment of Data Fusion and are created from the GCP Console. At the time of creating, we can decide what type of instance we want to deploy, and the choice will be determined by requirements and cost. There are three options:

+   - **Developer** instances provide a full-featured edition with zonal availability and limitations on execution environments.
+   - **Basic** instances provide comprehensive integration capabilities but have limitations on simultaneous pipeline runs and are recommended for non-critical environments.
+   - **Enterprise** instances also offer comprehensive integration capabilities but there are no limitations on simultaneous pipeline runs and are recommended for critical environments.