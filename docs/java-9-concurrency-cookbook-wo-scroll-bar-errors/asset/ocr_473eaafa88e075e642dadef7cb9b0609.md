+   2. Declare a private Integer attribute called number to store the number that this task will use for its calculations:

```
private final Integer number;
```

+   3. Implement the constructor of the class that initializes the attribute of the class:

```
public FactorialCalculator(Integer number) {
    this.number=number;
}
```

+   4. Implement the call() method. This method returns the factorial of the number attribute of FactorialCalculator:

```
@Override
public Integer call() throws Exception {
```

+   5. First, create and initialize the internal variables used in the method:

```
int result = 1;
```

+   6. If the number is 0 or 1, return 1. Otherwise, calculate the factorial of the number. Between two multiplications, for educational purposes, put this task to sleep for 20 milliseconds:

```
if ((number==0) || (number==1)) {
    result=1;
} else {
    for (int i=2; i<=number; i++) {
        result*=i;
        TimeUnit.MILLISECONDS.sleep(20);
    }
}
```

+   7. Write a message to the console with the result of the operation:

```
System.out.printf("%s: %d\n",Thread.currentThread().getName(),
                result);
```

+   8. Return the result of the operation:

```
return result;
```