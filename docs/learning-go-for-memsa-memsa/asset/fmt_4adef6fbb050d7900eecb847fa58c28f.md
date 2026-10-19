
```
func TestCache_Clean(t *testing.T) {
    c := cachev2.New[string, string](2*time.Second, 1*time.Second)
    c.Set("key", "hello")
    time.Sleep(4 * time.Second)
    val, ok = c.Get("key")
    if ok {
        t.Fatal("expected key to not be present")
    }
    if c.Stats.RemovedByClean != 1 {
        t.Error("expected removed by Sweep to be 1")
    }
}
```

If you run this test, it will pass, but it takes 4 seconds for the test to complete. Wrapping it in `synctest.Test` to speed up the test produces a surprising result:

```
=== RUN   TestCache_Clean_synctest
--- FAIL: TestCache_Clean_synctest (0.00s)
panic: deadlock: main bubble goroutine has exited but blocked goroutines remain [recovered, repanicked]
...
```

The `synctest` bubble ensures that every goroutine created within the bubble exit before the bubble ends. (I wish the Go team had called this “popping”, but the software industry is not in a whimsical moment.) If any goroutines continue to run, the test panics. Looking at the implemmentation of `clean`, there’s no way to signal to the goroutine when to exit. Without `synctest`, problems like this are very hard to detect.

The following changes fix this. First we update our cache implementation:

```
func (c *Cache[K, V]) clean(interval time.Duration) {
    ticker := time.NewTicker(interval)
    defer ticker.Stop()
    for {
        select {
        case <-ticker.C:
            c.mu.Lock()
            now := time.Now()
            for k, e := range c.items {
                if !now.Before(e.expiresAt) {
                    delete(c.items, k)
                    c.Stats.RemovedBySweep++
                }
            }
            c.mu.Unlock()
        case <-c.done:
            return
        }
    }
}

func (c *Cache[K, V]) Done() {
    c.mu.Lock()
    defer c.mu.Unlock()
    select {
    case <-c.done:
        return
    default:
        close(c.done)
    }
}
```

Next, we fix the test to call `c.Done`:

```
func TestCache_Clean_synctest(t *testing.T) {
    synctest.Test(t, func(t *testing.T) {
        c := cachev2.New[string, string](2*time.Second, 1*time.Second)
        t.Cleanup(c.Done)
        c.Set("key", "hello")
        time.Sleep(4 * time.Second)
        synctest.Wait()
        val, ok = c.Get("key")
        if ok {
            t.Error("expected key to not be present")
        }
        if c.Stats.RemovedBySweep != 1 {
            t.Error("expected removed by Sweep to be 1")
        }
    })
}
```

With the new `clean` implementation and the addition of the `Done` method, the test passes when it includes a call to `defer c.Done()`.

This brings us to the third problem addressed by `synctest`. You might have noticed the call to `synctest.Wait()` in the fixed code and wondered what it does. Before I answer that question, think about this: how do you *know* that sleeping for 4 seconds is enough? You know that the background cleaner goroutine will receive a tick from its timer within 4 seconds, but how do you know that it will finish looking for expired cache entries to remove and get back to waiting for the next tick of its timer?

The fact is that you don’t know how long to wait; you are taking for granted that it should be enough time. If you write tests this way and run them on a machine with lots of load (like a shared CI/CD server), you might find enough isn’t enough. Unless you use an absurdly big value for `time.Sleep`, tests fail randomly. In a `synctest` bubble where time “passes” instanteously, it becomes more likely that the work won’t complete.

The `synctest.Wait` function provides a solution for this problem. When you call it from inside a closure that’s passed to `synctest.Test`, the test pauses until all other goroutines run to completion, are waiting to read from or write to a channel, sleeping with a `time.Sleep`, waiting on a `WaitGroup.Wait`, or waiting on a `Cond.Wait`. The synctest spec calls this being “durably blocked”.

To make our background cleaner test correct, we need to add a `synctest.Wait` call after the `time.Sleep(4 * time.Second)`. This guarantees that the `clean` goroutine not only gets a chance to run, but that it runs until it is waiting to read the next tick from the ticker channel. This ensures that the entry was removed from the cache.

The `synctest` package solves many of the problems in testing concurrent code, but it does have some limitations. For more details, look at the [package documentation](https://pkg.go.dev/testing/synctest) and [the blog post from the Go team](https://go.dev/blog/testing-time) that introduced `synctest`.

# Exercises

Now that you’ve seen the more advanced testing tools provided by Go, these exercises will give you a chance to try them out. They are found in the *exercises* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercises). Solutions are in the *exercise-solutions* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercise-solutions).

1. In the *ex4* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex4), there is a file called *wealth.go* with a function called `CalcWealth` and a test for this function in the file *wealth_test.go*. Check the code coverage from this test. Examine the code and see if there’s a bug that wasn’t detected by the test. If so, add a test to demonstrate the bug, then fix the code.

2. In the *ex5* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex5), there is a file called *count.go*. It contains three functions that perform the same task, `CountWithFields`, `CountWithFieldsSeq`, and `CountManual`. Write tests to validate that these functions work, then write benchmarks to compare their performance and amount of memory used.

3. In the *ex6* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex6), there is a file called *tracker.go* with a type called `Tracker`. There is a test for this function in *tracker_test.go*. See if the test passes. If it does not, use the data race detector to figure out why, and fix the code to make it pass. Hint: fixing a concurrent bug might require you to think about lock acquisition and release lifetimes.

# Using Stubs in Go

So far, you’ve written tests for functions that didn’t depend on other code. This is not typical, as most code is filled with dependencies. As you saw in Chapter 7, Go allows you to abstract function calls in two ways: defining a function type and defining an interface. These abstractions help you write not only modular production code but also unit tests.

###### Tip

When your code depends on abstractions, it’s easier to write unit tests!

Let’s take a look at an example in the *sample_code/solver* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/solver). Define a type called `Processor`:

```
type Processor struct {
    Solver MathSolver
}
```

It has a field of type `MathSolver`:

```
type MathSolver interface {
    Resolve(ctx context.Context, expression string) (float64, error)
}
```

You’ll implement and test `MathSolver` in a bit.

`Processor` also has a method that reads an expression from an `io.Reader` and returns the calculated value:

```
func (p Processor) ProcessExpression(ctx context.Context, r io.Reader)
                                    (float64, error) {
    curExpression, err := readToNewLine(r)
    if err != nil {
        return 0, err
    }
    if len(curExpression) == 0 {
        return 0, ErrNoExpression
    }
    answer, err := p.Solver.Resolve(ctx, curExpression)
    return answer, err
}
```

Let’s write the code to test `ProcessExpression`. First, you need a simple implementation of the `Resolve` method to write your test:
