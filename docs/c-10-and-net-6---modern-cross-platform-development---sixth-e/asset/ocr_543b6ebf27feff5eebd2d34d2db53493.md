# Making a field static

The fields that we have created so far have all been instance members, meaning that a different value of each field exists for each instance of the class that is created. The alice and bob variables have different Name values.

Sometimes, you want to define a field that only has one value that is shared across all instances.

These are called static members because fields are not the only members that can be static. Let's see what can be achieved using static fields:

+   1. In the PacktLibrary project, add a new class file named BankAccount.cs.
+   2. Modify the class to give it three fields, two instance fields and one static field, as shown in the following code:

```
namespace Packt.Shared
{
    public class BankAccount
    {
        public string AccountName; // instance member
        public decimal Balance; // instance member
        public static decimal InterestRate; // shared member
    }
}
```

Each instance of BankAccount will have its own AccountName and Balance values, but all instances will share a single InterestRate value.

+   3. In Program.cs, add statements to set the shared interest rate and then create two instances of the BankAccount type, as shown in the following code:

```
BankAccount.InterestRate = 0.012M; // store a shared value

BankAccount jonesAccount = new(); // C# 9.0 and later
jonesAccount.AccountName = "Mrs. Jones";
jonesAccount.Balance = 2400;

WriteLine(format: "{0} earned {1:C} interest.",
    arg0: jonesAccount.AccountName,
    arg1: jonesAccount.Balance * BankAccount.InterestRate);

BankAccount gerrierAccount = new();
gerrierAccount.AccountName = "Ms. Gerrier";
gerrierAccount.Balance = 98;

WriteLine(format: "{0} earned {1:C} interest.",
    arg0: gerrierAccount.AccountName,
    arg1: gerrierAccount.Balance * BankAccount.InterestRate);
```