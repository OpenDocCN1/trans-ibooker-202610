```
public __construct ( )
public count ( ) : int
abstract public getIterator ( ) : Traversable
public offsetExists ( object $object ) : bool
public offsetGet ( object $object ) : mixed
public offsetSet ( object $object , mixed $value ) :
void
public offsetUnset ( object $object ) : void
}
```

Just like SplObjectStorage, this new class appears as an array of objects. Because it implements IteratorAggregate, you can use the getIterator() method, to gain access to the inner iterator. Thus, the new class offers not only traditional array access, but OOP iterator access as well, the best of both worlds! Before getting into the details of how to use WeakMap, it's important for you to understand typical usage for SplObjectStorage.

# Implementing a container class using SplObjectStorage

A potential use for the SplObjectStorage class is to use it to form the basis of a dependency injection (DI) container (also referred to as a service locator or inversion of control container). DI container classes are designed to create and hold instances of objects for easy retrieval.

In this example, we load a container class with an array of expensive objects drawn from the Laminas\Filter\* classes. We then use the container to sanitize sample data, after which we unset the array of filters:

+   1. First, we define a container class based on SplObjectStorage. (Later, in the next section, we develop another container class that does the same thing and is based upon WeakMap.) Here is the UsesSplObjectStorage class. In the __construct() method, we attach configured filters to the SplObjectStorage instance:

```
// /repo/src/Php7/Container/UsesSplObjectStorage.php
namespace Php7\Container;
use SplObjectStorage;
class UsesSplObjectStorage {
    public $container;
```