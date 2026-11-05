The most common types in .NET for working with collections are shown in the following table:

| Namespace | Example type(s) | Description |
|---|---|---|
| System.Collections | IEnumerable, IEnumerable<T> | Interfaces and base classes used by collections. |
| System.Collections.Generic | List<T>, Dictionary<T>, Queue<T>, Stack<T> | Introduced in C# 2.0 with .NET Framework 2.0. These collections allow you to specify the type you want to store using a generic type parameter (which is safer, faster, and more efficient). |
| System.Collections.Concurrent | BlockingCollection, ConcurrentDictionary, ConcurrentQueue | These collections are safe to use in multithreaded scenarios. |
| System.Collections.Immutable | ImmutableArray, ImmutableDictionary, ImmutableList, ImmutableQueue | Designed for scenarios where the contents of the original collection will never change, although they can create modified collections as a new instance. |

# Common features of all collections

All collections implement the ICollection interface; this means that they must have a Count property to tell you how many objects are in them, as shown in the following code:

```
namespace System.Collections
{
    public interface ICollection : IEnumerable
    {
        int Count { get; }
        bool IsSynchronized { get; }
        object SyncRoot { get; }
        void CopyTo(Array array, int index);
    }
}
```

For example, if we had a collection named passengers, we could do this:

```
int howMany = passengers.Count;
```

All collections implement the IEnumerable interface, which means that they can be iterated using the foreach statement. They must have a GetEnumerator method that returns an object that implements IEnumerator; this means that the returned object must have MoveNext and Reset methods for navigating through the collection and a Current property containing the current item in the collection, as shown in the following code:

```
namespace System.Collections
{
    public interface IEnumerable
    {

```