
# Apache Hadoop Overview

The Apache Hadoop project develops open-source software for reliable, scalable, distributed computing. Hadoop is a framework with the ability to store large data sets. The data sets are distributed across clusters of computers using simple programming models and written in Java to run on a single computer or large clusters of commodity hardware computers. The software derives from papers published by Google and incorporates the features of the Google File System and `MapReduce` paradigm, reflected in the names Hadoop Distributed File System and Hadoop `MapReduce`.

Hadoop technology was designed to eliminate the data handling problems on big data in companies and achieved great success. It can process huge amounts of data quickly from a variety of sites such as Facebook, Twitter, and the like, and automated sensors.

## Hadoop Terminology

*   **`Open-source software`**: Works on the open network of developers that create and manage the programs.
*   **`Framework`**: Everything that enables users to develop and run software applications and is done through programs, tool sets, connections, and the like.
*   **`Distributed data`**: Divided and stored on multiple computers, and computations can run simultaneously on multiple connected machines.
*   **`Massive storage`**: The Hadoop framework can store enormous amounts of data in blocks for storage on clusters of low-cost commodity hardware.
*   **`Faster processing`**: Processes large amounts of data in parallel across clusters of tightly connected low-cost computers for quick results.

## Knowledge Check: Hadoop Description

7. Which of these describes Hadoop?

    1. It is proprietary.
    2. It is open source.
    3. It is proprietary but available for reduced costs to nonprofit organizations.
    4. It is proprietary and must run in a Unix environment.

## History of Hadoop

Larger data demands have resulted in users wanting quicker searches and faster processing time. Doug Cutting and Mike Caferella worked on these issues with an open-source web search engine project called `Nutch`. They used distributed data and calculations across low-cost computers just to accomplish multiple tasks simultaneously. During the same period, Google was working on a similar project to achieve data storage and data processing in a distributed fashion to return faster and more relevant searches. In 2006, Cutting moved to Yahoo and continued with the `Nutch` project that was divided into two projects — the web crawler portion and the distributed processing portion (which became known as Hadoop).

Hadoop was released in 2008 as an open-source project that is managed and maintained by a nonprofit `ASF`. The project is developed by a global community of software developers and contributors.

## Hadoop Core Components

![](images/ufig5-30.png)

Source: “Big Data Basics,” MSSQLTIPS, [www.mssqltips.com/sqlservertip/3262/big-data-basics--part-6--related-apache-projects-in-hadoop-ecosystem/](http://www.mssqltips.com/sqlservertip/3262/big-data-basics--part-6--related-apache-projects-in-hadoop-ecosystem/)

These are the core components of `ASF`.

*   **`HDFS`**: Java-based distributed file system that stores data such as structured, unstructured, and the like, without prior organization.
*   **`MapReduce`**: Software model that allows large sets of data to be processed in parallel.
*   **`YARN`**: Resource management framework to schedule and handle resource requests from distributed apps.
*   **`Pig`**: Platform for manipulating data stored in `HDFS`. All this is done through a compiler called `Pig Latin`, for `MapReduce` programs and a high-level language. The user can avoid writing `MapReduce` programs to perform data extractions, transformations, loading, and basic analysis.
*   **`Hive`**: Similar to database programming, it creates data in the form of tables. `Hive` is data warehousing and a query language.
*   **`HBase`**: Runs on top of Hadoop and serves as input and output for `MapReduce` jobs. It is a nonrelational distributed database.
*   **`Zookeeper`**: Application meant for coordination of distributed processes.
*   **`Ambari`**: Web interface meant for managing, configuring, and testing in the Hadoop environment.
*   **`Flume`**: Software that collects, aggregates, and streams data into `HDFS`.
*   **`Sqoop`**: Transfer mechanism for data moved between Hadoop and its relational database.
*   **`Oozie`**: Hadoop job scheduler.

## Knowledge Check: Hive

8. What is `Hive`?

    1. Data cleansing program.
    2. DA tool.
    3. Data warehousing and a query language.
    4. An application meant for coordination of distributed processes.

## Practice Questions

1. Based on the IBM survey, list several capabilities of big data.
2. What is the purpose of Hadoop?
3. What is the purpose of `MapReduce`?
4. What is “R”?

## Notes

1.  [1](#c05.xhtml#ic05-fn-01) Mathieu Dumolin, “Better Complex Event Processing at Scale Using a Microservices-Based Streaming Architecture (Part 1) | MapR,” January 9, 2017, [https://mapr.com/blog/better-complex-event-processing-scale-usingmicroservices-based-streaming-architecture-part-1/](https://mapr.com/blog/better-complex-event-processing-scale-usingmicroservices-based-streaming-architecture-part-1/).
2.  [2](#c05.xhtml#ic05-fn-02) Wayne Eckerson, “Predictive Analytics,” TDWI, May 10, 2007, [https://tdwi.org/articles/2007/05/10/predictiveanalytics.aspx](https://tdwi.org/articles/2007/05/10/predictiveanalytics.aspx), accessed April 7, 2020.
3.  [3](#c05.xhtml#ic05-fn-03) “Hadoop is here to stay,” Actian, accessed April 7, 2020, [www.actian.com/products/analytics-platform/vortex-sqlhadoop-analytics/.](http://www.actian.com/products/analytics-platform/vortex-sqlhadoop-analytics/)
4.  [4](#c05.xhtml#ic05-fn-04) “About AWS,” Amazon Web Services, accessed April 7, 2020, [http://aws.amazon.com/about-aws](http://aws.amazon.com/about-aws).
5.  [5](#c05.xhtml#ic05-fn-05) “FAQs About Cloudera,” Cloudera, accessed April 7, 2020, [www.cloudera.com/content/cloudera/en/about/faqs.html#one](http://www.cloudera.com/content/cloudera/en/about/faqs.html#one).
6.  [6](#c05.xhtml#ic05-fn-06) “Maximize the Value of Data at Rest,” Hortonworks, [https://hortonworks.com/products/data-platforms/hdp/](https://hortonworks.com/products/data-platforms/hdp/). Accessed April 7, 2020.
7.  [7](#c05.xhtml#ic05-fn-07) “Big Data at the Speed of Business,” IBM, [www-01.ibm.com/software/data/bigdata/](http://www-01.ibm.com/software/data/bigdata/), Accessed April 7, 2020.
8.  [8](#c05.xhtml#ic05-fn-08) “Ignite’s Infobright DB–Architecture Overview,” accessed April 7, 2020, [https://cdn2.hubspot.net/hubfs/4281917/Ignite_Technologies](https://cdn2.hubspot.net/hubfs/4281917/Ignite_Technologies) March2018 Theme/Docs/Ignite l White Paper l Infobright DB-Architecture Overview-1.pdf?t=1528843828245.
9.  [9](#c05.xhtml#ic05-fn-09) “The Kognitio Analytic Platform,” Kognitio, accessed March 18, 2016, [kognitio.com/analyticalplatform/](http://kognitio.com/analyticalplatform/).
10. [10](#c05.xhtml#ic05-fn-10) “Data Lake,” TechTarget, accessed April 7, 2020, [http://searchaws.techtarget.com/definition/data-lake](http://searchaws.techtarget.com/definition/data-lake).
11. [11](#c05.xhtml#ic05-fn-11) “What Is Apache Hadoop?” MapR, accessed April 7, 2020, [www.mapr.com/products/apache-hadoop](http://www.mapr.com/products/apache-hadoop).
12. [12](#c05.xhtml#ic05-fn-12) “Understanding Microsoft Big Data Solutions,” accessed April 7, 2020, [https://msdn.microsoft.com/enus/library/dn749804.aspx](https://msdn.microsoft.com/enus/library/dn749804.aspx).
13. [13](#c05.xhtml#ic05-fn-13) Pivotal Software, “Pivotal Cloud-Native Platform,” Text/html, January 17, 2017, [https://pivotal.io/](https://pivotal.io/).
14. [14](#c05.xhtml#ic05-fn-14) “SAP Software Solutions | Business Applications and Technology,” SAP, accessed April 7, 2020, [https://www.sap.com/products/hana.html](https://www.sap.com/products/hana.html)
