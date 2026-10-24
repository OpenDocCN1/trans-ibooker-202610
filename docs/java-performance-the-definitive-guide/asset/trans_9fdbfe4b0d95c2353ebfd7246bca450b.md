

## TRANSACTION_SERIALIZABLE

这是最昂贵的事务模式；它要求事务期间访问的所有数据都被锁定，直到事务结束为止。这既适用于通过主键访问的数据，也适用于通过 WHERE 子句访问的数据——并且当存在 WHERE 子句时，表会被锁定，使得在事务持续期间无法添加满足该子句的新记录。序列化事务每次发出查询时都会看到相同的数据。

## TRANSACTION_REPEATABLE_READ

这要求所有被访问的数据在事务期间都被锁定。然而，其他事务可以随时向表中插入新行。此模式可能导致幻读：事务重新执行带有 WHERE 子句的查询时，第二次执行查询时可能得到不同的数据。

## TRANSACTION_READ_COMMITTED

此模式仅锁定事务期间被写入的行。这会导致不可重复读：事务中某一时刻读取的数据可能与事务中另一时刻读取的数据不同。

## TRANSACTION_READ_UNCOMMITTED

这是最便宜的事务模式。不涉及任何锁，因此一个事务可能会读取另一个事务中已写入但未提交的数据。这被称为脏读；问题在于第一个事务可能会回滚（意味着写入实际上从未发生），因此第二个事务操作的是错误的数据。

数据库以默认的事务隔离模式运行：MySQL 默认使用 TRANSACTION_REPEATABLE_READ；Oracle 和 IBM Db2 默认使用 TRANSACTION_READ_COMMITTED；等等。这里有很多数据库特定的变体。Db2 将其默认事务模式称为 CS（游标稳定）并对其他三种 JDBC 模式使用不同的名称。Oracle 不支持 TRANSACTION_READ_UNCOMMITTED 或 TRANSACTION_REPEATABLE_READ。

当执行 JDBC 语句时，它使用数据库的默认隔离模式。或者，可以调用 JDBC 连接上的 setTransaction() 方法，让数据库提供必要的事务隔离级别（如果数据库不支持给定级别，JDBC 驱动程序将要么抛出异常，要么静默地将隔离级别升级到其支持的下一个最严格级别）。

## TRANSACTION_NONE 和 Autocommit

JDBC 规范定义了第五种事务模式，即 TRANSACTION_NONE。从理论上讲，此事务模式不能通过 setTransactionIsolation() 模式指定，因为如果事务已存在，其隔离级别不能设置为 none。一些 JDBC 驱动程序（尤其是 Db2 的）确实允许进行该调用（甚至默认使用该模式）。其他 JDBC 驱动程序允许在用于初始化驱动程序的属性中指定隔离级别为 none。

严格来说，从具有 TRANSACTION_NONE 语义的连接执行语句不能将数据提交到数据库：它必须是只读查询。如果写入数据，则必须进行某种锁定；否则，如果一个用户使用 TRANSACTION_NONE 语义向表中写入一个长字符串，第二个用户可能会看到表中写入的部分字符串。数据库可以在该模式下运行，尽管这种情况并不常见；至少，写入单个表的数据预计是原子写入的。因此，写入操作实际上至少具有 TRANSACTION_READ_UNCOMMITTED 语义。

TRANSACTION_NONE 查询不能被提交，但使用 TRANSACTION_NONE 的 JDBC 驱动程序如果启用了 autocommit，则可能允许写入查询。这意味着数据库将每个查询视为单独的事务。即使如此，由于数据库（很可能）不允许其他事务看到部分写入，因此实际上使用的是 TRANSACTION_READ_UNCOMMITTED 语义。

对于简单的 JDBC 程序，这就足够了。更常见的是——特别是在使用 JPA 时——程序可能希望在事务中的数据上混合隔离级别。在一个查询我的员工信息以最终给我大幅加薪的应用程序中，我的员工记录的访问必须受到保护：该数据需要被视为 TRANSACTION_REPEATABLE_READ。但该事务也可能访问其他表中的数据，例如保存我的办公室 ID 的表。锁定该数据在事务期间并没有真正必要，因此对该行的访问完全可以作为 TRANSACTION_READ_COMMITTED（或可能更低）操作。

JPA 允许您按实体指定锁定级别（当然，实体通常只是数据库中的一行）。由于正确设置这些锁定级别可能很困难，使用 JPA 比在 JDBC 语句中执行锁定更容易。尽管如此，仍然可以在 JDBC 应用程序中使用不同的锁定级别，采用 JPA 使用的相同悲观和乐观锁定语义（如果您不熟悉这些语义，此示例应作为一个很好的介绍）。

在 JDBC 级别，基本方法是设置连接的隔离级别为 TRANSACTION_READ_UNCOMMITTED，然后仅显式锁定事务期间需要锁定的数据：

```
try (Connection c = DriverManager.getConnection(URL, p)) {
    c.setAutoCommit(false);
    c.setTransactionIsolation(TRANSACTION_READ_UNCOMMITTED);
    try (PreparedStatement ps1 = c.prepareStatement(
        "SELECT * FROM employee WHERE e_id = ? FOR UPDATE")) {
        ... process info from ps1 ...
    }
    try (PreparedStatement ps2 = c.prepareStatement(
        "SELECT * FROM office WHERE office_id = ?")) {
        ... process info from ps2 ...
    }
    c.commit();
}
```

ps1 语句在员工数据表上建立显式锁：在此事务持续期间，没有其他事务能够访问该行。实现此目的的 SQL 语法是非标准的。您必须查阅数据库供应商的文档以了解如何实现所需的锁定级别，但常见的语法是包含 FOR UPDATE 子句。这种锁定称为悲观锁定。它积极防止其他事务访问相关数据。

锁定性能通常可以通过使用乐观锁定来改善。如果数据访问没有竞争，这将带来显著的性能提升。但是，如果数据即使有轻微竞争，编程也会变得更加困难。

在数据库中，乐观并发是通过版本列实现的。当从一行中选择数据时，选择必须包括所需数据加上版本列。要选择关于我的信息，我可以发出以下 SQL：

```
SELECT first_name, last_name, version FROM employee WHERE e_id = 5058;
```

此查询将返回我的姓名（Scott 和 Oaks）以及当前的版本号（比如 1012）。当事务完成时，事务会更新版本列：

```
UPDATE employee SET version = 1013 WHERE e_id = 5058 AND version = 1012;
```

如果相关行需要可重复读或序列化语义，即使数据在事务期间仅被读取，也必须执行此更新——这些隔离级别要求锁定事务中使用的只读数据。对于读已提交语义，仅当行中的其他数据也被更新时才需要更新版本列。

在这种方案下，如果两个事务同时使用我的员工记录，每个事务都会读取版本号 1012。第一个完成的事务将成功将版本号更新为 1013 并继续。第二个事务将无法更新员工记录——

来回的流量将在应用程序和数据库之间发生，因为结果集正在被处理。

通常，这里没有正确答案；在某些情况下，将大部分数据保留在数据库中并在需要时检索会更有效，而在其他情况下，一次性在查询执行时加载所有数据会更有效。要控制这一点，请使用 PreparedStatement 对象上的 setFetchSize() 方法，让 JDBC 驱动程序知道每次应传输多少行。

此默认值因 JDBC 驱动程序而异；例如，在 Oracle 的 JDBC 驱动程序中，默认值是 10。当在前面所示的循环中调用 executeQuery() 方法时，数据库将返回 10 行数据，这些数据将由 JDBC 驱动程序在内部缓冲。对 next() 方法的前 10 次调用将处理其中一个缓冲行。第 11 次调用将返回到数据库以检索另外 10 行，依此类推。

## Other Ways to Set the Fetch Size

我在这里建议在（准备好的）语句对象上使用 setFetchSize() 方法，但该方法也存在于 ResultSet 接口上。在任何一种情况下，大小都只是一个提示。JDBC 驱动程序可以自由忽略该值，将其舍入为另一个值，或做任何它想做的事情。没有保证，但如果在查询执行前设置该值（即在语句对象上），提示更有可能被遵守。

一些 JDBC 驱动程序还允许您在创建连接时通过向 DriverManager 的 getConnection() 方法传递属性来设置默认提取大小。如果这种方式更容易管理，请查阅供应商的文档。

尽管值各不相同，但 JDBC 驱动程序通常会将默认提取大小设置为相当小的数字。这种方法在大多数情况下是合理的；特别是，它不太可能导致应用程序中的任何内存问题。如果 next() 方法（或结果集上的第一个 getter 方法）的性能偶尔特别缓慢，请考虑增加提取大小。

