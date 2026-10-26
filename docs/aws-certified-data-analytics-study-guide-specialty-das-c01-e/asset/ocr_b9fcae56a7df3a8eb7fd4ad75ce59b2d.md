![](bbox=[0.12, 0.11, 0.33, 0.26])

# Introduction

In this chapter, you will learn about how to process and analyze the data stored on the AWS platform. Data processing is one of the most critical phases within a data and analytics pipeline and is an important exam topic. From an exam perspective, data processing, analysis and visualization make up forty two percent of the exam questions, and hence understanding the concepts and related technologies are going to be critical if you are looking to do well during the exam. The AWS platform provides multiple technologies to let you process the data, including AWS Glue, Amazon EMR, Amazon Redshift, and Amazon Athena. We’ll primarily cover data processing and data analysis; however, data visualization will be covered in Chapter 5, “Data Visualization.”

As has been the theme throughout the book, we would like to look at the various options provided by the AWS platform to process and analyze the data. The available options are dependent on the type of processing needed, which is directly linked to the type of use case.

The complexity of processing and analyzing the data revolves around various factors like the size of the data, the structure, the processing speed demands, the SLA requirements, and the skill level. There is more than one way to process and analyze the same dataset, but choosing the right solution depends on your understanding of why one particular set of solutions and architecture is preferable over the other. The AWS Data Analytics Specialty Certification exam expects you to not only know the various options but also the key reasons that make one choice better than the other in a given scenario.

Data processing and analysis is the process of identifying, cleaning, transforming, and structuring the data in a way to make it suitable to identify useful information, draw conclusions, and support decision-making. Let’s look at the different types of analytical workloads that you may come across when working with data within your organization.

# Types of Analytical Workloads

There are five different analytical workloads:

Batch Workload Batch processing typically involves querying large amounts of “cold” data, and the turnaround time can vary from minutes to many hours. A typical