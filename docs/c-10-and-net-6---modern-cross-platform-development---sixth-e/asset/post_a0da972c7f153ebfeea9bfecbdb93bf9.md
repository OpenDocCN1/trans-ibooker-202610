
```
public static decimal? Median<T>(
    this IEnumerable<T> sequence, Func<T, decimal?> selector)
{
    return sequence.Select(selector).Median();
}

public static int? Mode(
    this IEnumerable<int?> sequence)
{
    var grouped = sequence.GroupBy(item => item);
    var orderedGroups = grouped.OrderByDescending(
        group => group.Count());
    return orderedGroups.FirstOrDefault()?.Key;
}

public static int? Mode<T>(
    this IEnumerable<T> sequence, Func<T, int?> selector)
{
    return sequence.Select(selector).Mode();
}

public static decimal? Mode(
    this IEnumerable<decimal?> sequence)
{
    var grouped = sequence.GroupBy(item => item);
    var orderedGroups = grouped.OrderByDescending(
        group => group.Count());
    return orderedGroups.FirstOrDefault()?.Key;
}

public static decimal? Mode<T>(
    this IEnumerable<T> sequence, Func<T, decimal?> selector)
{
    return sequence.Select(selector).Mode();
}
}
```

If this class was in a separate class library, to use your LINQ extension methods, you simply need to reference the class library assembly because the System.Linq namespace is already implicitly imported.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_527_0.png)

> **Warning!** All but one of the above extension methods cannot be used with IQueryable sequences like those used by LINQ to SQLite or LINQ to SQL Server because we have not implemented a way to translate our code into the underlying query language like SQL.

## Trying the chainable extension method

First, we will try chaining the ProcessSequence method with other extension methods:

- 1. In Program.cs, in the FilterAndSort method, modify the LINQ query for Products to call your custom chainable extension method, as shown highlighted in the following code:
- 2. In Program.cs, uncomment the FilterAndSort method and comment out any calls to other methods.
- 3. Run the code and note that you see the same output as before because your method doesn't modify the sequence. But you now know how to extend a LINQ expression with your own functionality.

```
DbSet<Product>? allProducts = db.Products;

if (allProducts is null)
{
    WriteLine("No products found.");
    return;
}

IQueryable<Product> processedProducts = allProducts.ProcessSequence();

IQueryable<Product> filteredProducts = processedProducts
.Where(product => product.UnitPrice < 10M);
```

## Trying the mode and median methods

Second, we will try using the Mode and Median methods to calculate other kinds of average:

- 1. At the bottom of Program.cs, create a method to output the mean, median, and mode, for UnitsInStock and UnitPrice for products, using your custom extension methods and the built-in Average extension method, as shown in the following code:

```
static void CustomExtensionMethods()
{
    using (Northwind db = new())
    {
        WriteLine("Mean units in stock: {0:N0}",
            db.Products.Average(p => p.UnitsInStock));

        WriteLine("Mean unit price: {0:$#,##0.00}",
            db.Products.Average(p => p.UnitPrice));

        WriteLine("Median units in stock: {0:N0}",
            db.Products.Median(p => p.UnitsInStock));

        WriteLine("Median unit price: {0:$#,##0.00}",
            db.Products.Median(p => p.UnitPrice));

        WriteLine("Mode units in stock: {0:N0}",
            db.Products.Mode(p => p.UnitsInStock));

        WriteLine("Mode unit price: {0:$#,##0.00}",
            db.Products.Mode(p => p.UnitPrice));
    }
}
```

- 2. In Program.cs, comment any previous method calls and call CustomExtensionMethods.
- 3. Run the code and view the result, as shown in the following output:

```
Mean units in stock: 41
Mean unit price: $28.87
Median units in stock: 26
Median unit price: $19.50
Mode units in stock: 0
Mode unit price: $18.00
```

There are four products with a unit price of $18.00. There are five products with 0 units in stock.

## Working with LINQ to XML

LINQ to XML is a LINQ provider that allows you to query and manipulate XML.

## Generating XML using LINQ to XML

Let's create a method to convert the Products table into XML:

- 1. In the LinqWithEFCore project, at the top of Program.cs, import the System.Xml.Linq namespace.
- 2. At the bottom of Program.cs, create a method to output the products in XML format, as shown in the following code:

```
static void OutputProductsAsXml()
{
    using (Northwind db = new())
    {
        Product[] productsArray = db.Products.ToArray();

        XElement xml = new("products",
            from p in productsArray
            select new XElement("product",
                new XAttribute("id", p.ProductId),
                new XAttribute("price", p.UnitPrice),
                new XElement("name", p.ProductName)));

        WriteLine(xml.ToString());
    }
}
```

- 3. In Program.cs, comment the previous method call and call OutputProductsAsXml.
- 4. Run the code, view the result, and note that the structure of the XML generated matches the elements and attributes that the LINQ to XML statement declaratively described in the preceding code, as shown in the following partial output:

```
<products>
    <product id="1" price="18">
        <name>Chai</name>
    </product>
    <product id="2" price="19">
        <name>Chang</name>
    </product>
    ...
```

## Reading XML using LINQ to XML

You might want to use LINQ to XML to easily query or process XML files:

- 1. In the LinqWithEFCore project, add a file named settings.xml.
- 2. Modify its contents, as shown in the following markup:

```
<?xml version="1.0" encoding="utf-8" ?>
<appSettings>
    <add key="color" value="red" />
    <add key="size" value="large" />
    <add key="price" value="23.99" />
</appSettings>
```

If you are using Visual Studio 2022 for Windows, then the compiled application executes in the LinqWithEFCore\bin\Debug\net6.0 folder so it will not find the settings.xml file unless we indicate that it should always be copied to the output directory.

- 3. In Solution Explorer, right-click the settings.xml file and select Properties.
- 4. In Properties, set Copy to Output Directory to Copy always.

5. At the bottom of Program.cs, create a method to complete these tasks, as shown in the following code:
- - Load the XML file.
- - Use LINQ to XML to search for an element named appSettings and its descendants named add.
- - Project the XML into an array of an anonymous type with Key and Value properties.
- - Enumerate through the array to show the results:

```
static void ProcessSettings()
{
    XDocument doc = XDocument.Load("settings.xml");

    var appSettings = doc.Descendants("appSettings")
        .Descendants("add")
        .Select(node => new
        {
            Key = node.Attribute("key")?.Value,
            Value = node.Attribute("value")?.Value
        }).ToArray();

    foreach (var item in appSettings)
    {
        WriteLine($"{item.Key}: {item.Value}");
    }
}
```

6. In Program.cs, comment the previous method call and call ProcessSettings.

7. Run the code and view the result, as shown in the following output:

```
color: red
size: large
price: 23.99
```

## Practicing and exploring

Test your knowledge and understanding by answering some questions, get some hands-on practice, and explore with deeper research into the topics covered in this chapter.

## Exercise 11.1 – Test your knowledge

Answer the following questions:

- 1. What are the two required parts of LINQ?
- 2. Which LINQ extension method would you use to return a subset of properties from a type?
- 3. Which LINQ extension method would you use to filter a sequence?
- 4. List five LINQ extension methods that perform aggregation.
- 5. What is the difference between the Select and SelectMany extension methods?
- 6. What is the difference between IEnumerable<T> and IQueryable<T>? And how do you switch between them?
- 7. What does the last type parameter T in generic Func delegates like Func<T1, T2, T> represent?
- 8. What is the benefit of a LINQ extension method that ends with OrDefault?
- 9. Why is query comprehension syntax optional?
- 10. How can you create your own LINQ extension methods?

## Exercise 11.2 – Practice querying with LINQ

In the Chapter11 solution/workspace, create a console application, named Exercise02, that prompts the user for a city and then lists the company names for Northwind customers in that city, as shown in the following output:

```
Enter the name of a city: London
There are 6 customers in London:
Around the Horn
B's Beverages
Consolidated Holdings
Eastern Connection
North/South
Seven Seas Imports
```

Then, enhance the application by displaying a list of all unique cities that customers already reside in as a prompt to the user before they enter their preferred city, as shown in the following output:

```
Aachen, Albuquerque, Anchorage, Århus, Barcelona, Barquisimeto, Bergamo, Berlin, Bern, Boise, Bräcke, Brandenburg, Bruxelles, Buenos Aires, Butte, Campinas, Caracas, Charleroi, Cork, Cowes, Cunewalde, Elgin, Eugene, Frankfurt a.M., Genève, Graz, Helsinki, I. de Margarita, Kirkland, København, Köln, Lander, Leipzig, Lille, Lisboa, London, Luleå, Lyon, Madrid, Mannheim, Marseille, México D.F., Montréal, München, Münster, Nantes, Oulu, Paris, Portland, Reggio Emilia, Reims, Resende, Rio de Janeiro, Salzburg, San Cristóbal, San Francisco, Sao Paulo, Seattle, Sevilla, Stavern, Strasbourg, Stuttgart, Torino, Toulouse, Tsawassen, Vancouver, Versailles, Walla Walla, Warszawa
```
