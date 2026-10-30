# The System.Collections.ObjectModel Namespace

Now that you understand how to work with the major generic classes, we will briefly examine an additional collection-centric namespace, System.Collections.ObjectModel. This is a relatively small namespace, which contains a handful of classes. Table 10-7 documents the two classes that you should most certainly be aware of.

# Table 10-7. Useful Members of System.Collections.ObjectModel

| System.Collections.ObjectModel Type | Meaning in Life |
|---|---|
| ObservableCollection<T> | Represents a dynamic data collection that provides notifications when items get added, when items get removed, or when the whole list is refreshed |
| ReadOnlyObservableCollection<T> | Represents a read-only version of ObservableCollection<T> |

The ObservableCollection<T> class is useful, in that it has the ability to inform external objects when its contents have changed in some way (as you might guess, working with ReadOnlyObservableCollection<T> is similar but read-only in nature).

# Working with ObservableCollection<T>

Create a new Console Application project named FunWithObservableCollections and import the System.Collections.ObjectModel namespace into your initial C# code file. In many ways, working with ObservableCollection<T> is identical to working with List<T>, given that both of these classes implement the same core interfaces. What makes the ObservableCollection<T> class unique is that this class supports an event named CollectionChanged. This event will fire whenever a new item is inserted, a current item is removed (or relocated), or the entire collection is modified.

Like any event, CollectionChanged is defined in terms of a delegate, which in this case is NotifyCollectionChangedEventHandler. This delegate can call any method that takes an object as the first parameter and takes a NotifyCollectionChangedEventArgs as the second. Consider the following code, which populates an observable collection containing Person objects and wires up the CollectionChanged event:

```
using System;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using FunWithObservableCollections;

// Make a collection to observe
//and add a few Person objects.
ObservableCollection<Person> people = new ObservableCollection<Person>()
{
    new Person{ FirstName = "Peter", LastName = "Murphy", Age = 52 },
    new Person{ FirstName = "Kevin", LastName = "Key", Age = 48 },
};

// Wire up the CollectionChanged event.
people.CollectionChanged += people_CollectionChanged;
```