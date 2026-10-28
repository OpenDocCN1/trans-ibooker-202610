4. Then, we implement the onNext() method. It simply writes information about the received item to the console:

```
@Override
public void onNext(Item item) {
    System.out.printf("%s: Consumer 1: Item received\n", Thread.currentThread().getName());
    System.out.printf("%s: Consumer 1: %s\n", Thread.currentThread().getName(), item.getTitle());
    System.out.printf("%s: Consumer 1: %s\n", Thread.currentThread().getName(), item.getContent());
}
```

5. And finally, we implement the onSubscribe() method. It simply writes a message to the console and doesn't request any item using the request() method of the Subscription object:

```
@Override
public void onSubscribe(Flow.Subscription subscription) {
    System.out.printf("%s: Consumer 1: Subscription received\n", Thread.currentThread().getName());
    System.out.printf("%s: Consumer 1: No Items requested\n", Thread.currentThread().getName());
}
```

6. Now, it's time for the Consumer2 class. Specify that it also implements the Subscriber interface parameterized with the Item class. In this case, we have a private Subscription attribute to store the subscription object. The onComplete() and onError() methods are equivalent to the ones of the Consumer1 class:

```
public class Consumer2 implements Flow.Subscriber<Item> {

    private Subscription subscription;

    @Override
    public void onComplete() {
        System.out.printf("%s: Consumer 2: Completed\n", Thread.currentThread().getName());
    }
```