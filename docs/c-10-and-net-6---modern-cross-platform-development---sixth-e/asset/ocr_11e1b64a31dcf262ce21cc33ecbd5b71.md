If a new item (for example, Santiago) is inserted between London and Sydney, then the index of Sydney is automatically incremented. Therefore, you must be aware that an item's index can change after inserting or removing items, as shown in the following table:

| Index | Item |
|---|---|
| 0 | London |
| 1 | Paris |
| 2 | London |
| 3 | Santiago |
| 4 | Sydney |

# Dictionaries

Dictionaries are a good choice when each value (or object) has a unique sub value (or a made-up value) that can be used as a key to quickly find a value in the collection later. The key must be unique. For example, if you are storing a list of people, you could choose to use a government-issued identity number as the key.

Think of the key as being like an index entry in a real-world dictionary. It allows you to quickly find the definition of a word because the words (for example, keys) are kept sorted, and if we know we're looking for the definition of manatee, we would jump to the middle of the dictionary to start looking, because the letter M is in the middle of the alphabet.

Dictionaries in programming are similarly smart when looking something up. They must implement the interface IDictionary<TKey, TValue>, as shown in the following code:

```
namespace System.Collections.Generic
{
    [DefaultMember("Item")] // aka this indexer
    public interface IDictionary<TKey, TValue>
        : ICollection<KeyValuePair<TKey, TValue>>,
          IEnumerable<KeyValuePair<TKey, TValue>>, IEnumerable
    {
        TValue this[TKey key] { get; set; }
        ICollection<TKey> Keys { get; }
        ICollection<TValue> Values { get; }
        void Add(TKey key, TValue value);
        bool ContainsKey(TKey key);
        bool Remove(TKey key);
        bool TryGetValue(TKey key, [MaybeNullWhen(false)] out TValue value);
    }
}
```

Items in a dictionary are instances of the struct, aka the value type KeyValuePair<TKey, TValue>, where TKey is the type of the key and TValue is the type of the value, as shown in the following code: