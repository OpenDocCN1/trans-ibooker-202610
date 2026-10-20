Spark job definitions handle processing of data. One of the best features here is the option to enable the Native Execution Engine (NEE), which will accelerate the speed of notebook execution. You can also configure standard Spark pools usage and setup, upload your own custom Spark libraries, and configure which standard libraries can be used in this specific workspace.

# Jobs

Here, you can configure the setup of execution in Spark jobs. This setting allows you to reserve the maximum number of cores for a Spark job to run, which will lower the number of parallel jobs available to run. You can also configure the timeout of the Spark pools, which will help you stop the Spark pools if they are not in use for the specified amount of time.

# High concurrency

Here, you can configure the option of running multiple notebooks on the same Spark session, which will help you optimize the exertion of parallel processes. You can do the same for notebooks running inside a pipeline (more on notebooks and pipelines later in this book).

# Automatic log

Here, you can enable the option to track machine learning models and experiments from notebooks on their metrics, parameters, and models.

# Data Factory

Here, you can configure the runtime settings for Apache Airflow, which is an orchestrator for execution of processes built using code. (Note that it’s out of scope for this book.)