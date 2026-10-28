![](bbox=[0.1, 0.1, 0.86, 0.25])

Figure 16-6. Date too late

![](bbox=[0.26, 0.31, 0.71, 0.46])

Figure 16-7. Dates accepted

# Solution

Here is the solution:

```
static void Main(string[] args)
{
    // Inputs
    Console.Write("Payment date: ");
    string inputPayment = Console.ReadLine();
    DateTime paymentDate = Convert.ToDateTime(inputPayment);

    Console.Write("Invoice date: ");
    string inputInvoice = Console.ReadLine();
    DateTime invoiceDate = Convert.ToDateTime(inputInvoice);

    // Checking
    bool ok = true;
    if (invoiceDate < paymentDate)
```