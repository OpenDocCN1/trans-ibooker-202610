# It’s Not That Easy!

When I introduce the transaction script pattern in my domain-driven design classes, my students often raise their eyebrows, and some even ask, “Is it worth our time? Aren’t we here for the more advanced patterns and techniques?”

The thing is, the transaction script pattern is a *foundation* for the more advanced business logic implementation patterns you will learn in the forthcoming chapters. Furthermore, despite its apparent simplicity, it is the easiest pattern to get wrong. A considerable number of production issues I have helped to debug and fix, in one way or another, often boiled down to a misimplementation of the transactional behavior of the system’s business logic.

Let’s take a look at three common, real-life examples of data corruption that results from failing to correctly implement a transaction script.

# Lack of transactional behavior

A trivial example of failing to implement transactional behavior is to issue multiple updates without an overarching transaction. Consider the following method that updates a record in the Users table and inserts a record into the VisitsLog table:

```
01 public class LogVisit
02 {
03     ...
04
05     public void Execute(Guid userId, DataTime visitedOn)
06     {
07         _db.Execute("UPDATE Users SET last_visit=@p1 WHERE user_id=@p2",
08             visitedOn, userId);
09         _db.Execute(@"INSERT INTO VisitsLog(user_id, visit_date) VALUES(@p1, @p2)", userId, visitedOn);
10     }
11 }
12 }
```