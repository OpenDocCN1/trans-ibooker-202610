![](bbox=[0.15, 0.08, 0.88, 0.36])

Figure 7-3. Sharding the event store

# Deleting Data

The event store is an append-only database, but what if I do need to delete data physically; for example, to comply with GDPR?³

This need can be addressed with the forgettable payload pattern: all sensitive information is included in the events in encrypted form. The encryption key is stored in an external key–value store: the key storage, where the key is a specific aggregate’s ID and the value is the encryption key. When the sensitive data has to be deleted, the encryption key is deleted from the key storage. As a result, the sensitive information contained in the events is no longer accessible.

# Why Can’t I Just…?

Why can’t I just write logs to a text file and use it as an audit log?

Writing data both to an operational database and to a logfile is an error-prone operation. In its essence, it’s a transaction against