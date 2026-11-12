Clusters are interesting insofar as they define regions of normal connections; everything else outside is unusual and potentially anomalous.

# KDD Cup 1999 Data Set

The KDD Cup was an annual data mining competition organized by a special interest group of the Association for Computing Machinery (ACM). Each year, a machine learning problem was posed, along with a data set, and researchers were invited to submit a paper detailing their best solution to the problem. In 1999, the topic was network intrusion, and the data set is still available at the KDD website. We will need to download the kddcupdata.data.gz and kddcup.names files from the website. The remainder of this chapter will walk through building a system to detect anomalous network traffic using Spark, by learning from this data.

> Don't use this data set to build a real network intrusion system! The data did not necessarily reflect real network traffic at the time—even if it did, it reflects traffic patterns from 22 years ago.

Fortunately, the organizers had already processed raw network packet data into summary information about individual network connections. The data set is about 708 MB in size and contains about 4.9 million connections. This is large, if not massive, and is certainly sufficient for our purposes here. For each connection, the data set contains information like the number of bytes sent, login attempts, TCP errors, and so on. Each connection is one line of CSV-formatted data set, containing 38 features. Feature information and ordering can be found in the kddcup.names file.

Unzip the kddcup.data.gz data file and copy it into your storage. This example, like others, will assume the file is available at data/kddcup.data. Let us see the data in its raw form:

```
head -n 1 data/kddcup.data
```

```
...
```