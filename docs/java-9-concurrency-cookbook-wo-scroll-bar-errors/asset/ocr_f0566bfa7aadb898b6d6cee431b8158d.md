14. Implement the constructor of the class to initialize its attribute:

```
public Bank(Account account) {
    this.account=account;
}
```

15. Implement the run() method of the task. Use the subtractAmount() method of the account to make 10 decrements of 1,000 each from its balance:

```
@Override
public void run() {
    for (int i=0; i<10; i++){
        account.subtractAmount(1000);
    }
}
```

16. Implement the main class of the example by creating a class named Main and adding the main() method to it:

```
public class Main {
    public static void main(String[] args) {
```

17. Create an Account object and set its balance to 1000:

```
Account account=new Account();
account.setBalance(1000);
```

18. Create a new Company task and a thread to execute it:

```
Company company=new Company(account);
Thread companyThread=new Thread(company);
```

19. Create a new Bank task and a thread to execute it:

```
Bank bank=new Bank(account);
Thread bankThread=new Thread(bank);
```

20. Write the initial balance of the account in the console:

```
System.out.printf("Account : Initial Balance: %d\n", account.getBalance());
```

21. Start the threads:

```
companyThread.start();
bankThread.start();
```