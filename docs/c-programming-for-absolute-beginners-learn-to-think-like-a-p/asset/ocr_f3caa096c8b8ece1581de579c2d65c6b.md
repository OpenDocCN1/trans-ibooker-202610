![](bbox=[0.139, 0.109, 0.901, 0.324])

*Figure 10-2.* Calculating total costs

# Solution

Here is the code:

```
```csharp
static void Main(string[] args)
{
    // Fixed values
    const double bookPrice = 29.8;
    const double dvdPrice = 9.9;
    const double shipmentPrice = 25;

    // Inputs
    Console.WriteLine("Order");
    Console.WriteLine("-----");

    Console.Write("Product \"C# Programming for Absolute Beginners (book)\" - enter number of pieces: ");
    string inputBookPieces = Console.ReadLine();
    int bookPieces = Convert.ToInt32(inputBookPieces);

    Console.Write("Product \"All Quiet on Western Front (DVD)\" - enter number of pieces: ");
    string inputDvdPieces = Console.ReadLine();
    int dvdPieces = Convert.ToInt32(inputDvdPieces);
```
```