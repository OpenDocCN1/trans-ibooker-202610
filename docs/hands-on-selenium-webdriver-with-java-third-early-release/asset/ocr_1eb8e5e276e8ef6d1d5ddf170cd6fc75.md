```
// Other listeners
}
```

A common way for registering a test listener in JUnit 4 is to create a custom runner and use that runner in test classes. Example 8-36 shows a custom test runner registering the previous listener. Then, Example 8-35 shows a test skeleton using this runner.

# Example 8-36. Test listeners using JUnit 4

```
public class MyTestRunner extends BlockJUnit4ClassRunner { ①

    public MyTestRunner(Class<?> clazz) throws InitializationError
    {
        super(clazz);
    }

    @Override
    public void run(RunNotifier notifier) {
        notifier.addListener(new MyTestListener()); ②
        super.run(notifier); ③
    }
}
```

+   ① We extend Blockjunit4classrunner, the default test runner in JUnit 4.
+   
+   ② We register our custom test listener.
+   
+   ③ We call the parent to continue using the default test runner.

# Example 8-37. Test listeners using JUnit 4

```
@RunWith(MyTestRunner.class) ①
public class ListenersJUnit4Test {

    // Test logic

}
```