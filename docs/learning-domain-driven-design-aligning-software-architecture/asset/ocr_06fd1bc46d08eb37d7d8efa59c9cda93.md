two storage mechanisms: the database and the file. If the first one fails, the second one has to be rolled back. For example, if a database transaction fails, no one cares to delete the prior log messages. Hence, such logs are not consistent, but rather, eventually inconsistent.

Why can’t I keep working with a state-based model, but in the same database transaction, append logs to a logs table?

From an infrastructural perspective, this approach does provide consistent synchronization between the state and the log records. However, it is still error prone. What if the engineer who will be working on the codebase in the future forgets to append an appropriate log record?

Furthermore, when the state-based representation is used as the source of truth, the additional log table’s schema usually degrades into chaos quickly. There is no way to enforce that all required information is written and that it is written in the correct format.

Why can’t I just keep working with a state-based model but add a database trigger that will take a snapshot of the record and copy it into a dedicated “history” table?

This approach overcomes the previous one’s drawback: no explicit manual calls are needed to append records to the log table. That said, the resultant history only includes the dry facts: what fields were changed. It misses the business contexts: why the fields were changed. The lack of “why” drastically limits the ability to project additional models.

# Conclusion

This chapter explained the event sourcing pattern and its application for modeling the dimension of time in the domain model’s