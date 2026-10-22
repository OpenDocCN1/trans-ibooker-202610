

- 1 https://redis.io/
- 2 Redis Cluster 的详细规范位于 https://redis.io/topics/cluster-spec
- 3 Emil Koutanov 在 https://towardsdatascience.com/redis-unsafe-at-any-speed-f2731f738a25 对 Redis 数据安全性的分析非常出色
- 4 MMAPv1 已在 MongoDB 4.0 版本中被弃用。您可以在 https://docs.mongodb.com/v4.0/core/mmapv1/ 找到其文档
- 5 可以在 https://www.percona.com/blog/2019/01/03/mongodb-engines-mmapv1-vs-wiredtiger/ 找到这两种文件系统的良好比较
- 6 这篇博文说明了如何通过使用更短的字段名称来实现 25% 的文档大小减少：https://www.percona.com/blog/2016/03/24/percona-how-to-field-names-and-document-size-in-mongodb/
- 7 这个简短的视频描述了 MongoDB 中事务的实现方式 - https://www.youtube.com/watch?v=iuj4Hh5EQvo
- 8 Siyuan Zhou, Shuai Mu: Fault-Tolerant Replication with Pull-Based Consensus in MongoDB, NSDI 2021 - https://www.usenix.org/conference/nsdi21/presentation/zhou
- 9 这份 Jepsen 报告提供了对 MongoDB 一致性的略显过时但仍然引人入胜的分析 - https://jepsen.io/analyses/mongodb-4.2.6
- 10 Decandia, G.; Hastorun, D.; Jampani, M.; Kakulapati, G.; Lakshman, A.; Pilchin, A.; Sivasubramanian, S.; Vosshall, P.; Vogels, W. (2007). “Dynamo: Amazon’s Highly Available Key-value Store”. *Proceedings of twenty-first ACM SIGOPS symposium on Operating systems principles - SOSP ’07*. p. 205.
- 11 有关按需容量如何工作 的更多详细信息，请参阅 https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html#HowItWorks.OnDemand
- 12 有关排序键强大功能的更多示例，请参阅 https://aws.amazon.com/blogs/database/using-sort-keys-to-organize-data-in-amazon-dynamodb/
- 13 有关 DynamoDB 事务的详细报道可在以下位置找到 - https://aws.amazon.com/blogs/database/optimize-amazon-dynamodb-transaction-resilience/
- 14 数据建模的最佳实践描述于 https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html
- 15 有关 DAX 的信息可在 https://aws.amazon.com/dynamodb/dax/ 找到
- 16 自适应容量和热键描述于 https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html#bp-partition-key-partitions-adaptive-boost
- 17 有关事务隔离级别的解释位于 https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transaction-apis.html
- 18 AWS 停机事件在此处描述
https://www.washingtonpost.com/business/2021/12/22/amazon-web-services-experiences-another-big-outage/

## 关于作者

**Ian Gorton** 拥有 30 年的软件架构师、作者、计算机科学教授和顾问经验。他自研究生时期起就专注于分布式技术，并曾在银行、电信、政府、医疗保健以及科学建模和仿真等领域从事大规模软件系统的工作。在此期间，他见证了软件系统演进到今天常规运行的庞大规模。

Ian 已撰写 3 本书籍，包括 *Essential Software Architecture* 和 *Data Intensive Computing*，并是关于软件架构和软件工程的 200 多篇科学和专业出版物的作者。在卡内基梅隆软件工程研究所，他领导了大数据和大规模可扩展系统方面的研发项目，并在 2015 年加入东北大学担任计算机科学教授后，继续在这些主题上工作、写作和演讲。他拥有英国谢菲尔德哈勒姆大学的博士学位，是 IEEE 计算机学会的高级会员。
