# There's more...

In the example of this recipe, we used the `complete()`, `get()`, `join()`, `supplyAsync()`, `thenApplyAsync()`, `thenAcceptAsync()`, and `allOf()` methods of the `CompletableFuture` class. However, this class has a lot of useful methods that help increase the power and flexibility of this class. These are the most interesting ones:

+   - Methods to complete a `CompletableFuture` object: In addition to the `complete()` method, the `CompletableFuture` class provides the following three methods:
+     - `cancel()`: This completes `CompletableFuture` with a `CancellationException` exception.
+     - `completeAsync()`: This completes `CompletableFuture` with the result of the `Supplier` object passed as a parameter. The `Supplier` object is executed in a different thread by the default executor.
+     - `completeExceptionally()`: This method completes `CompletableFuture` with the exception passed as a parameter.

+   - Methods to execute a task: In addition to the `supplyAsync()` method, the `CompletableFuture` class provides the following method:
+     - `runAsync()`: This is a static method of the `CompletableFuture` class that returns a `CompletableFuture` object. This object will be completed when the `Runnable` interface is passed as a parameter to finish its execution. It will be completed with a void result.

+   - Methods to synchronize the execution of different tasks: In addition to the `allOf()`, `thenAcceptAsync()`, and `thenApplyAsync()` methods, the `CompletableFuture` class provides the following methods to synchronize the execution of tasks:
+     - `anyOf()`: This is a static method of the `CompletableFuture` class. It receives a list of `CompletableFuture` objects and returns a new `CompletableFuture` object. This object will be completed with the result of the first `CompletableFuture` parameter that is completed.