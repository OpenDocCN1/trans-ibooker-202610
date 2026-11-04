5. We then serialize it, and restore it to a variable, $a2:

```
$str = serialize($a1);
$a2 = unserialize($str);
var_dump($a2);
```

6. From the var_dump() output below, you can see that the object has been fully restored. In addition, we know that the custom serialize() method was invoked because the u property is initialized with a date and time value. Here is the output:

```
object(A)#3 (3) {
  ["a":"A":private]=> string(1) "A"
  ["b":"A":private]=> string(1) "B"
  ["u":"A":private]=> object(DateTime)#4 (3) {
    ["date"]=> string(26) "2021-02-12 05:35:10.835999"
    ["timezone_type"]=> int(3)
    ["timezone"]=> string(3) "UTC"
  }
}
```

Let's now have a look at issues with the serialization process for objects that implement the Serializable interface.

# Examining PHP serializable interface issues

There is an overall problem with the earlier approach to serialization. If a class to be serialized has defined a __wakeup() magic method, it's not invoked immediately upon unserialization. Rather, any defined __wakeup() magic methods are first queued up, the entire chain of objects is unserialized, and only then are methods in the queue executed. This can result in a mismatch between what is seen by an object's unserialize() method compared to what is seen by its queued __wakeup() method.

This architectural flaw can result in inconsistent behavior and ambiguous results when dealing with objects that implement the Serializable interface. Many developers consider the Serializable interface to be severely broken due to the need to create back references when the serialization of nested objects occurs. This need arises in situations where nested serialization calls occur.