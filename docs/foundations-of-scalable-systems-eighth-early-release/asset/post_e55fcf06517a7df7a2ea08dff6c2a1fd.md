
- 1 https://redis.io/
- 2 The detailed specification for Redis Cluster is at https://redis.io/topics/cluster-spec
- 3 There’s an excellent analysis of data safety in Redis by Emil Koutanov at https://towardsdatascience.com/redis-unsafe-at-any-speed-f2731f738a25
- 4 MMAPv1 was deprecated in MongoDB version 4.0. You can find its documentation at https://docs.mongodb.com/v4.0/core/mmapv1/
- 5 A good comparison of the two file systems can be found at https://www.percona.com/blog/2019/01/03/mongodb-engines-mmapv1-vs-wiredtiger/
- 6 This blog post illustrates how a 25% reduction in document size can be achieved with shorter field names : https://www.percona.com/blog/2016/03/24/percona-how-to-field-names-and-document-size-in-mongodb/
- 7 This short video describes how transactions are implemented in MongoDB - https://www.youtube.com/watch?v=iuj4Hh5EQvo
- 8 Siyuan Zhou, Shuai Mu: Fault-Tolerant Replication with Pull-Based Consensus in MongoDB, NSDI 2021 - https://www.usenix.org/conference/nsdi21/presentation/zhou
- 9 A slightly out-of-date but still fascinating analysis of MongoDB consistency is provided by this Jepsen report - https://jepsen.io/analyses/mongodb-4.2.6
- 10 Decandia, G.; Hastorun, D.; Jampani, M.; Kakulapati, G.; Lakshman, A.; Pilchin, A.; Sivasubramanian, S.; Vosshall, P.; Vogels, W. (2007). “Dynamo: Amazon’s Highly Available Key-value Store”. *Proceedings of twenty-first ACM SIGOPS symposium on Operating systems principles - SOSP ’07*. p. 205.
- 11 For more details on how on-demand capacity works, see https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html#HowItWorks.OnDemand
- 12 For more examples of the power of sort keys, see https://aws.amazon.com/blogs/database/using-sort-keys-to-organize-data-in-amazon-dynamodb/
- 13 Detailed coverage of DynamoDB transactions can be found here - https://aws.amazon.com/blogs/database/optimize-amazon-dynamodb-transaction-resilience/
- 14 Best practices for data modeling are described at https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html
- 15 Information on DAX can be found at https://aws.amazon.com/dynamodb/dax/
- 16 Adaptive capacity and hot keys are described at https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html#bp-partition-key-partitions-adaptive-boost
- 17 An explanation of transaction isolation levels is at https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transaction-apis.html
- 18 The AWS outages is described here
https://www.washingtonpost.com/business/2021/12/22/amazon-web-services-experiences-another-big-outage/

## About the Author

**Ian Gorton** has 30 years experience as a software architect, author, computer science professor and consultant. He has focused on distributed technologies since his days in graduate school, and has worked on large scale software systems in areas such as banking, telecommunications, government, health care and scientific modeling and simulation. During this time, he has seen software systems evolve to the massive scale they routinely operate at today.

Ian has written 3 books, including *Essential Software Architecture* and *Data Intensive Computing*, and is the author of 200+ scientific and professional publications on software architecture and software engineering. At the Carnegie Mellon Software Engineering Institute he led R&D projects in big data and massively scalable systems, and has continued working, writing and speaking on these topics since joining Northeastern University as a Professor of Computer Science in 2015. He has a PhD from Sheffield Hallam University, UK and is a Senior Member of the IEEE Computer Society.
