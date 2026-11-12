
public String toString() { return "quite heavy"; }
}

This class represents a hypothetical heavyweight resource. In its constructor, we print a message to tell us when it's created. Let's use an instance of this class in the first trial version of the Holder class, named HolderNaive.

## lazy/fpij/HolderNaive.java

```java
public class HolderNaive {
    private Heavy heavy;

    public HolderNaive() {
        System.out.println("Holder created");
    }

    public Heavy getHeavy() {
        if (heavy == null) {
            heavy = new Heavy();
        }

        return heavy;
    }

    //...
}
```

At first glance, this code appears simple. We created a null reference, heavy, and assigned it to a proper instance on the first call to the getHeavy method. Let's use this class to create an instance of HolderNaive and see if it postpones the creation of the Heavy instance.

## lazy/fpij/HolderNaive.java

```java
final HolderNaive holder = new HolderNaive();
System.out.println("deferring heavy creation...");
System.out.println(holder.getHeavy());
System.out.println(holder.getHeavy());
```

This is the code's output:

Holder created
deferring heavy creation...
Heavy created
quite heavy
quite heavy

That appears to work. The solution is familiar, but it's also a rather simplistic solution that fails thread safety. Let's work through it.

## Providing Thread Safety

For an instance of `HolderNaive`, the dependent instance of `Heavy` is created on the first call to the `getHeavy` method. On subsequent calls to this method, the already created instance will be returned. That's exactly what we want, but there's a catch. This code suffers from a race condition.

If two or more threads call the `getHeavy` method at the same time, then we could end up with multiple `Heavy` instances, potentially one per thread. This side effect is undesirable. Let's fix it.

```java
public synchronized Heavy getHeavy() {
    if (heavy == null) {
        heavy = new Heavy();
    }
    return heavy;
}
```

We marked `getHeavy` with the `synchronized` keyword to ensure mutual exclusion. If two or more threads call this method concurrently, due to mutual exclusion, only one will be allowed to enter and the others will queue up for their turn. The first one to enter into the method will create the instance.

When subsequent threads enter this method, they will see that the instance already exists, and will simply return it.

We averted the race condition, but the solution created another negative impact. Every call to the `getHeavy` method now has to endure the synchronization overhead; the calling threads have to cross the memory barrier (see Brian Goetz’s *Java Concurrency in Practice* [Goe06]) even if there are no concurrently competing threads.

In fact, the possibility of the race condition is so short-lived it can happen only when the heavy reference is first being assigned, and the synchronization approach is a rather heavy-handed solution. We need thread safety until the reference is first created, and free unhindered access to the reference after that. We’ll achieve this by using David Wheeler’s advice: “Any problem in computer science can be solved with another level of indirection.”[14]

## Adding a Level of Indirection

The indirection we’ll add in this example comes from a `Supplier<T>` class. This is a functional interface in the JDK, with one abstract method named `get` that returns an instance. In other words, this is a factory that keeps on giving without expecting anything as input, kind of like a mother’s love.

In the most rudimentary form, a `Supplier` will return an instance. For example, we could implement `Supplier<Heavy>` to return an instance of `Heavy`, like so:

```java
Supplier<Heavy> supplier = () -> new Heavy();
```

Alternatively, we could use a constructor reference instead of the traditional `new` syntax to instantiate an instance. A constructor reference is much like a method reference, except it's a reference to a constructor instead of a method. We can use a constructor reference anywhere a lambda expression does nothing more than instantiate an instance. Let's look at an example with a constructor reference.

```java
Supplier<Heavy> supplier = Heavy::new;
```

We took a look at what a Supplier can do for us, but we need something more than this simple form. We need to postpone and cache the instance. We can do that by moving the instance creation to another function, as we see next, in the final version of the Holder class.

lazy/fpij/Holder.java

```java
public class Holder {
    private Supplier<Heavy> heavy = () -> createAndCacheHeavy();

    public Holder() {
        System.out.println("Holder created");
    }

    public Heavy getHeavy() {
        return heavy.get();
    }

    //...
}
```

The field heavy in this version is an instance of the Supplier<Heavy>. We assign it to a lambda expression, and the Java compiler synthesizes from it an instance with the expected get method. The implementation simply routes the call to a createAndCacheHeavy method, which we'll implement soon. The getHeavy method returns the same thing the Supplier's get method returns.

When an instance of Holder is created, as we can see, an instance of Heavy is not created. This design achieves the goal of lazy initialization. We also need a non-draconian solution to thread safety. This is where the createAndCacheHeavy method comes in.

Let's first look at the code for this method.

lazy/fpij/Holder.java

```java
private synchronized Heavy createAndCacheHeavy() {
    class HeavyFactory implements Supplier<Heavy> {
        private final Heavy heavyInstance = new Heavy();

        public Heavy get() { return heavyInstance; }
    }

    if (!HeavyFactory.class.isInstance(heavy)) {
        heavy = new HeavyFactory();
    }

    return heavy.get();
}
```

We'll mark this method synchronized so threads calling this method concurrently will be mutually exclusive. But within this method, on the first call, we quickly replace the Supplier reference, heavy, with a direct supplier, HeavyFactory, that will return an instance of Heavy. Let's see how this adequately solves thread safety.

Let's consider a scenario in which a new instance of Holder has just been created. Let's assume two threads invoke the getHeavy method concurrently, followed by a third thread calling this method much later. When the first two threads call the default supplier's get method in the Holder, the createAndCacheHeavy method will let one of them through and make the other wait. The first thread to enter will check if heavy is an instance of the HeavyFactory. Since it's not the default Supplier, this thread will replace heavy with an instance of HeavyFactory. Finally, it returns the Heavy instance that this HeavyFactory holds.

The second concurrent thread to enter will again check if heavy is an instance of HeavyFactory, and will bypass the creation. It would simply return the same instance the first thread returned. Here we assume Heavy itself is thread-safe, and we're only focusing on the thread safety of Holder.

We've taken care of the race condition, but since the instance has been created lazily, we no longer need to be so protective. Now that heavy has been replaced with HeavyFactory, subsequent calls to the getHeavy method will go directly to the HeavyFactory's get method and will not incur any synchronization overhead.

We designed lazy initialization and, at the same time, avoided null checks. We also ensured the thread safety of the lazy instance creation. This is a simple, lightweight implementation of the virtual proxy pattern. Next, we'll use lambda expressions to postpone function evaluations.

## Lazy Evaluations

In the previous section, we delayed the creation of heavyweight objects to make code execution faster. We’ll explore that further in this section to delay running methods and use that approach to improve our designs. The main objective is to reduce the execution of code to the bare minimum—especially the expensive code—and speed up the execution.

Java already uses lazy execution when evaluating logical operations. For example, in fn1() || fn2(), the call fn2 is never performed if fn1 returns a boolean true. Likewise, if we replace the || with &&, the call to fn2 never happens if fn1 returns a boolean false. Programs benefit from this short-circuiting; we avoid unnecessary evaluation of expressions or functions, and that can help improve performance. Often we rely on such short-circuiting for code correctness, as well.

While Java uses lazy or normal order when evaluating logical operators, it uses eager or applicative order when evaluating method arguments. All the arguments to methods are fully evaluated before a method is invoked. If the method doesn’t use all of the passed arguments, the program has wasted time and effort executing them. We can use lambda expressions to postpone the execution of select arguments.

The Java compiler evaluates lambda expressions and method references in the argument list at the called location. The invocation of these is postponed until they’re explicitly called from within the methods they’re passed to. We can take advantage of this behavior to delay or even avoid method invocation by embedding calls to them within lambda
