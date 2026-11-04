Here is the output from this code example running in PHP 8:

```
root@php8_tips_php8 [ /repo/ch10 ]#
php php8_weak_reference.php
object(class@anonymous)#1 (1) {
  ["name"]=>  string(4) "Fred"
}
object(class@anonymous)#1 (1) {
  ["name"]=>  string(4) "Fred"
}
object(class@anonymous)#2 (1) {
  ["name"]=>  string(4) "Fred"
}
NULL
```

The output tells us an interesting story! The second var_dump() operation shows us that even though $obj1 has been unset, it still lives on like a zombie because of the strong reference created with $obj2. If you are dealing with expensive objects and complex application code, in order to free up memory, you'll need to first hunt down and destroy all the references before memory is freed!

On the other hand, if you really need the memory, instead of making a direct object assignment, which in PHP is automatically by reference, create the reference using the WeakReference::create() method. A weak reference has all the power of a normal reference. The only difference is that if the object it refers to is destroyed or goes out of scope, the weak reference is automatically destroyed as well.

As you can see from the output, the result of the last var_dump() operation was NULL. This tells us that the object has truly been destroyed. When the main object is unset, all of its weak references go away automatically. Now that you have an idea of how to use weak references, and the potential problem they solve, it's time to take a look at a new class, WeakMap.

# Working with WeakMap

In PHP 8, a new class, WeakMap, has been added that leverages weak reference support. The new class is similar to SplObjectStorage in functionality. Here is the official class definition:

```
final WeakMap implements Countable,
    ArrayAccess, IteratorAggregate {
```