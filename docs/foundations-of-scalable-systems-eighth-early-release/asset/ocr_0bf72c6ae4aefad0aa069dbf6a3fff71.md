These are lists of strings, with operations to manipulate elements at the head, tail, and in the body of the list.

# Sets and Sorted Sets

Sets represent a collection of unique strings. Sorted sets associate a score value with each element and maintain the strings in ascending score order. This makes it possible to efficiently access elements in the set by score or rank order.

# Hashes

Like a Python map, a Redis Hash maps a key value represented as a string to one of more string values. Hashes are the primary Redis structure for representing application data objects such as user profiles, or stock inventory.

Operations on a single key are atomic. You can also specify a group of operations as requiring atomic execution using the multi and exec commands. All commands you place between multi and exec are called a Redis transaction, and are serialized and executed in order. An example of a Redis transaction is in the code example below. This defines a transaction with two operations. The first adds a string representing a new customer order to a neworders list. The second modifies the value of the key lastorder in the hashmap for the user. A Redis server queues these commands until it receives the exec command, and then executes them in sequence.

```
multi
lpush neworders "orderid 600066 customer 89788 item 788990 amount 11 date 12/24/21"
hmset user:89788 lastorder 600066
exec
```

Transactions are essentially the only way to perform operations that move or compute data across multiple types. They are however limited, in that they only provide atomicity when all commands succeed. If a command fails, there are no rollback capabilities. This means that even if one