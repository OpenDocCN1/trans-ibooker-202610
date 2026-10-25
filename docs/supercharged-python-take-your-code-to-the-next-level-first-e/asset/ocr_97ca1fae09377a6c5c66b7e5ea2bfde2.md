such as (1, 2), can.

+   • The values may be of any type; however, it is often a good idea to use the same type, if possible, for all the value objects.

There’s a caution you should keep in mind. If you attempt to get the value for a particular key and if that key does not exist, Python raises an exception. To avoid this, use the **get** method to ensure that the specified key exists.

![](bbox=[0.1484, 0.3281, 0.2539, 0.4047])

Click here to view code image

```
dictionary.get(key [,default_value])
```

In this syntax, the square brackets indicate an optional item. If the _key_ exists, its corresponding value in the dictionary is returned. Otherwise, the _default_value_ is returned, if specified; or **None** is returned if there is no such default value. This second argument enables you to write efficient histogram code such as the following, which counts frequencies of words.

Click here to view code image

```
s = (input('Enter a string: ')).split()
wrd_counter = {}
for wrd in s:
    wrd_counter[wrd] = wrd_counter.get(wrd, 0) + 1
```

What this example does is the following: When it finds a new