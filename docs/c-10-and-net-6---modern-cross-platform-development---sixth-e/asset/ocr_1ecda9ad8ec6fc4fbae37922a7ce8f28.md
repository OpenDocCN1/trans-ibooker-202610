```
case "VA": // Virginia
    rate = 0.05M;
    break;
case "CA": // California
    rate = 0.0825M;
    break;
default: // most US states
    rate = 0.06M;
    break;
}

return amount * rate;
}
```

In the preceding code, note the following:

+   - CalculateTax has two inputs: a parameter named amount that will be the amount of money spent, and a parameter named twoLetterRegionCode that will be the region the amount is spent in.
+   - CalculateTax will perform a calculation using a switch statement and then return the sales tax or VAT owed on the amount as a decimal value; so, before the name of the function, we have declared the data type of the return value to be decimal.

2. Comment out the TimesTable method call and call the CalculateTax method, passing values for the amount such as 149 and a valid region code such as FR, as shown in the following code:

```
// TimesTable(6);

decimal taxToPay = CalculateTax(amount: 149, twoLetterRegionCode: "FR");
WriteLine($"You must pay {taxToPay} in tax.");
```

3. Run the code and view the result, as shown in the following output:

```
You must pay 29.8 in tax.
```

![](bbox=[0.14, 0.79, 0.22, 0.86])

> We could format the taxToPay output as currency by using {taxToPay:C} but it will use your local culture to decide how to format the currency symbol and decimals. For example, for me in the UK, I would see £29.80.

Can you think of any problems with the CalculateTax function as written? What would happen if the user enters a code such as fr or UK? How could you rewrite the function to improve it? Would using a switch expression instead of a switch statement be clearer?