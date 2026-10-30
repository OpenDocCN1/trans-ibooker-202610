# Table 10-5. Classes of System.Collections.Generic

| Generic Class | Supported Key Interfaces | Meaning in Life |
|---|---|---|
| Dictionary<TKey, TValue> | ICollection<T>, IDictionary<TKey, TValue>, IEnumerable<T> | This represents a generic collection of keys and values. |
| LinkedList<T> | ICollection<T>, IEnumerable<T> | This represents a doubly linked list. |
| List<T> | ICollection<T>, IEnumerable<T>, IList<T> | This is a dynamically resizable sequential list of items. |
| Queue<T> | ICollection (Not a typo! This is the nongeneric collection interface.), IEnumerable<T> | This is a generic implementation of a first-in, first-out list. |
| SortedDictionary<TKey, TValue> | ICollection<T>, IDictionary<TKey, TValue>, IEnumerable<T> | This is a generic implementation of a sorted set of key-value pairs. |
| SortedSet<T> | ICollection<T>, IEnumerable<T>, ISet<T> | This represents a collection of objects that is maintained in sorted order with no duplication. |
| Stack<T> | ICollection (Not a typo! This is the nongeneric collection interface.), IEnumerable<T> | This is a generic implementation of a last-in, first-out list. |

The System.Collections.Generic namespace also defines many auxiliary classes and structures that work in conjunction with a specific container. For example, the LinkedListNode<T> type represents a node within a generic LinkedList<T>, the KeyNotFoundException exception is raised when attempting to grab an item from a container using a nonexistent key, and so forth. Be sure to consult the .NET Core documentation for full details of the System.Collections.Generic namespace.

In any case, your next task is to learn how to use some of these generic collection classes. Before you do, however, allow me to illustrate a C# language feature (first introduced in .NET 3.5) that simplifies the way you populate generic (and nongeneric) collection containers with data.

# Understanding Collection Initialization Syntax

In Chapter 4, you learned about object initialization syntax, which allows you to set properties on a new variable at the time of construction. Closely related to this is collection initialization syntax. This C# language feature makes it possible to populate many containers (such as ArrayList or List<T>) with items by using syntax similar to what you use to populate a basic array. Create a new .NET Core Console application named FunWithCollectionInitialization. Clear out the generated code in Program.cs and add the following using statements:

```
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
```

> Note You can apply collection initialization syntax only to classes that support an Add() method, which is formalized by the ICollection<T>/ICollection interfaces.