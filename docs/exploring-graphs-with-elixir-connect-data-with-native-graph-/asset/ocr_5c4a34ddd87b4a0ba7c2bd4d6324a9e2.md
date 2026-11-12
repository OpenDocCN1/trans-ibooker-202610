the use of `super` may appear superfluous, but it's required. That's how the Java compiler knows if we're referring to a `default` method (when `super` is used) or a `static` method in the interface. In Java, interfaces can optionally have `default` methods and `static` methods, possibly with the same name.

To see the behavior of the `default` methods in action, let's create an instance of `SeaPlane` and invoke the methods on it.

```
SeaPlane seaPlane = new SeaPlane();
seaPlane.takeOff();
seaPlane.turn();
seaPlane.cruise();
seaPlane.land();
```

Before running the code on the computer, we'll run it mentally; let's go over the code to ensure we've understood the rules.

The call to the `takeOff` method should go to the implementation in the `FastFly` interface (`rules 1 and 2`). The implementation of the `turn` method in `Vehicle` should be picked for the call to the `turn` method on the `SeaPlane`, even though these are available on the interfaces (`rule 3`). Since we were forced to implement the `cruise` method on the `SeaPlane`, that specific implementation of the method should be invoked for the call to `cruise` (`rule 4`). Finally, the call to the `land` method will land on the implementation in the `Fly` interface (`rule 1`).

We can now compare the output we got from the mental run of the code with the output from the run on the computer:

```
FastFly::takeOff
Vehicle::turn
SeaPlane::cruise currently cruise like: Sail::cruise
```