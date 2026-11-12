# Defining a Functional Interface

```
@FunctionalInterface
public interface TailCall<T> {
    TailCall<T> apply();

    default boolean isComplete() { return false; }

    //...
}
```

A functional interface must have one abstract— unimplemented—method. It may have zero or more default or implemented methods. It may also have static methods.