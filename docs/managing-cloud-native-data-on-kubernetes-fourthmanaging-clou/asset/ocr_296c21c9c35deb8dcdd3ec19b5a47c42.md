# Introduction to Analytics

Analytic workloads and the accompanying infrastructure operations are much different from other workloads. Analytics isn’t just another containerized system to orchestrate. The typical stateful applications like databases we have examined in previous chapters have many similar characteristics but tend to stay static or predictably slow-growing once deployed. There is one aspect of analytic workloads that strikes fear in many administrators: volume. While persistent data stores like databases can consume gigabytes to terabytes of storage, analytic volumes can easily soar into petabytes, creating an entirely new class of problems to solve. They don’t call it “Big Data” for nothing.

The Oxford Dictionary defines analytics as “the systematic computational analysis of data or statistics.” Wikipedia adds “It is used for the discovery, interpretation, and communication of meaningful patterns in data.” Combine those definitions with large volumes of data and what sort of outcome should we expect for cloud native applications? Below is a breakdown of the different types of analytics workflows and methodologies.

# Batch Analytics

In computer science terms, a batch is a series of instructions that are applied to data with little or no user interaction. The idea of running batch jobs is as old as general-purpose computing. In distributed systems such as Apache Hadoop or Apache Spark, each individual job consists of a program that can operate on smaller bits of data in parallel and in stages or pipelines. The smaller results are combined into a single, final result at the end of a job. An example of this is MapReduce discussed later in the book. In most cases, statistical analysis is done, such as count, average, and percentile measurement. Batch analytics is the focus of this chapter.

# Stream Analytics

As Discussed previously in Chapter 8, stream analytics is about what is happening, whereas batch analytics is about what happened. Many of