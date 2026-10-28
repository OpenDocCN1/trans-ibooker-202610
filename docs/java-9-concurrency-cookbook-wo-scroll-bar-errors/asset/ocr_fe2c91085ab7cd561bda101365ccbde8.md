7. Implement a method named subtractAmount() to decrement the value of the balance attribute. As it occurs with the addAmount() method, we modify the values of the operations and commission attributes:

```
public void subtractAmount(long amount) {
    this.balance.getAndAdd(-amount);
    this.operations.increment();
    this.commission.accumulate(amount);
}
```

8. Create a class named Company and specify that it implements the Runnable interface. This class will simulate the payments made by a company:

```
public class Company implements Runnable {
```

9. Declare a private Account attribute named account:

```
private final Account account;
```

10. Implement the constructor of the class to initialize its attribute:

```
public Company(Account account) {
    this.account=account;
}
```

11. Implement the run() method of the task. Use the addAmount() method of the account to make 10 increments of 1,000 each in its balance:

```
@Override
public void run() {
    for (int i=0; i<10; i++){
        account.addAmount(1000);
    }
}
```

12. Create a class named Bank and specify that it implements the Runnable interface. This class will simulate the withdrawal of money from the account:

```
public class Bank implements Runnable {
```

13. Declare a private Account attribute named account:

```
private final Account account;
```