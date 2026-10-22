Redis server to better exploit multicore nodes and provide higher throughput.

To provide data safety, the in-memory data structure maintained by a single Redis server can be made durable using two approaches. In one, you can configure a periodic background thread to dump the memory contents to disk. This snapshot process uses the fork() system call, and hence can be expensive if the memory contents are large. In high throughput systems, snapshots are typically configured at intervals of tens of seconds. Snapshots can also be triggered after a configurable number of writes to provide a known bound of potential data loss.

The other approach is to configure Redis to log every command to an Append Only File (AOF). This is essentially an operation log, and is persisted by default every second. Using both approaches, namely snapshots and operation logging, provides the greatest data safety guarantees. In the event of a server crash, the AOF can be replayed against the latest snapshot to recreate the server data contents in memory.

# Data Model and API

Redis is a Key-Value store. It offers a small collection of data structures that applications can use to create data objects associated with unique keys. Each data structure has a set of defined commands that applications use to create, manipulate and delete data objects. Commands are simple and operate on a single object identified by the key.

The core Redis structures are:

# Strings

Strings are versatile in Redis, and are able to store both text and binary data, with a maximum of 512MB in length. For example, you can use strings as a random access vector using get() and set() operations on specified subranges. Strings can also be used to represent and manipulate counters.

# Linked Lists