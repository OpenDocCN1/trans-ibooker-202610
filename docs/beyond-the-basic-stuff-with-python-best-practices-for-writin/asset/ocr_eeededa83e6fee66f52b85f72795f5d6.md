The variables spam and eggs refer to the same dictionary object ①, so their identities and values are the same. But bacon refers to a separate dictionary object ②, even though it contains data identical to spam and eggs. The identical data means bacon has the same value as spam and eggs, but they’re two different objects with two different identities.

# Items

In Python, an object that is inside a container object, like a list or dictionary, is also called an item or an element. For example, the strings in the list ['dog', 'cat', 'moose'] are objects but are also called items.

# Mutable and Immutable

As noted earlier, all objects in Python have a value, data type, and identity, and of these only the value can change. If you can change the object’s value, it’s a mutable object. If you can’t change its value, it’s an immutable object. Table 7-2 lists some mutable and immutable data types in Python.

Table 7-2: Some of Python’s Mutable and Immutable Data Types

| Mutable data types | Immutable data types |
|---|---|
| List | Integer |
| Dictionaries | Floating-point number |
| Sets | Boolean |
| Bytearray | String |
| Array | Frozen set |
|  | Bytes |
|  | Tuple |

When you overwrite a variable, it might look like you’re changing the object’s value, as in this interactive shell example:

```
>>> spam = 'hello'
>>> spam
'hello'
>>> spam = 'goodbye'
>>> spam
'goodbye'
```

But in this code, you haven’t changed the 'hello' object’s value from 'hello' to 'goodbye'. They’re two separate objects. You’ve only switched spam from referring to the 'hello' object to the 'goodbye' object. You can check whether this is true by using the id() function to show the two objects’ identities:

```
>>> spam = 'hello'
>>> id(spam)
```