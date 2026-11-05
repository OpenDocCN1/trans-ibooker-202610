
The **ImageSharp** project was started to fill that gap for modern .NET applications.

In their official documentation for System.Drawing, Microsoft says, "The System.Drawing namespace is not recommended for new development due to not being supported within a Windows or ASP.NET service, and it is not cross-platform. ImageSharp and SkiaSharp are recommended as alternatives."

Let us see what can be achieved with ImageSharp:

- 1. Use your preferred code editor to add a new console app named `WorkingWithImages` to the `Chapter08` solution/workspace.
- 2. In Visual Studio Code, select `WorkingWithImages` as the active OmniSharp project.
- 3. Create an `images` folder and download the nine images from the following link: https://github.com/markjprice/cs10dotnet6/tree/master/Assets/Categories
- 4. Add a package reference for `SixLabors.ImageSharp`, as shown in the following markup:
- 5. Build the `WorkingWithImages` project.
- 6. At the top of `Program.cs`, import some namespaces for working with images, as shown in the following code:
- 7. In `Program.cs`, enter statements to convert all the files in the images folder into grayscale thumbnails at one-tenth size, as shown in the following code:

```
<ItemGroup>
    <PackageReference Include="SixLabors.ImageSharp" Version="1.0.3" />
</ItemGroup>
```

```
using SixLabors.ImageSharp;
using SixLabors.ImageSharp.Processing;
```

```
string imagesFolder = Path.Combine(
    Environment.CurrentDirectory, "images");

IEnumerable<string> images =
    Directory.EnumerateFiles(imagesFolder);

foreach (string imagePath in images)
{
    string thumbnailPath = Path.Combine(
        Environment.CurrentDirectory, "images",
        Path.GetFileNameWithoutExtension(imagePath)
        + "-thumbnail" + Path.GetExtension(imagePath));

    using (Image image = Image.Load(imagePath))
    {
```

```
image.Mutate(x => x.Resize(image.Width / 10, image.Height / 10));
image.Mutate(x => x.Grayscale());
image.Save(thumbnailPath);
}
}
WriteLine("Image processing complete. View the images folder.");
```

- 8. Run the code.
- 9. In the filesystem, open the images folder and note the much-smaller-in-bytes grayscale thumbnails, as shown in Figure 8.1:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_392_0.png)

Figure 8.1: Images after processing

ImageSharp also has NuGet packages for programmatically drawing images and working with images on the web, as shown in the following list:

- SixLabors.ImageSharp.Drawing
- SixLabors.ImageSharp.Web

## Internationalizing your code

Internationalization is the process of enabling your code to run correctly all over the world. It has two parts: globalization and localization.

Globalization is about writing your code to accommodate multiple languages and region combinations. The combination of a language and a region is known as a culture. It is important for your code to know both the language and region because, for example, the date and currency formats are different in Quebec and Paris, despite them both using the French language.

There are International Organization for Standardization (ISO) codes for all culture combinations. For example, in the code da-DK, da indicates the Danish language and DK indicates the Denmark region, and in the code fr-CA, fr indicates the French language and CA indicates the Canada region.

ISO is not an acronym. ISO is a reference to the Greek word *isos* (which means equal).

Localization is about customizing the user interface to support a language, for example, changing the label of a button to be Close (en) or Fermer (fr). Since localization is more about the language, it doesn't always need to know about the region, although ironically enough, standardization (en-US) and standardisation (en-GB) suggest otherwise.

### Detecting and changing the current culture

Internationalization is a huge topic on which several thousand-page books have been written. In this section, you will get a brief introduction to the basics using the `CultureInfo` type in the `System.Globalization` namespace.

Let's write some code:

- 1. Use your preferred code editor to add a new console app named `Internationalization` to the `Chapter08` solution/workspace.
- 2. In Visual Studio Code, select `Internationalization` as the active OmniSharp project.
- 3. At the top of `Program.cs`, import the namespace for using globalization types, as shown in the following code:
- 4. Add statements to get the current globalization and localization cultures and output some information about them, and then prompt the user to enter a new culture code and show how that affects the formatting of common values such as dates and currency, as shown in the following code:

```
using System.Globalization; // CultureInfo
```

```
CultureInfo globalization = CultureInfo.CurrentCulture;
CultureInfo localization = CultureInfo.CurrentUICulture;

WriteLine("The current globalization culture is {0}: {1}",
    globalization.Name, globalization.DisplayName);

WriteLine("The current localization culture is {0}: {1}",
    localization.Name, localization.DisplayName);

WriteLine();

WriteLine("en-US: English (United States)");
WriteLine("da-DK: Danish (Denmark)");
WriteLine("fr-CA: French (Canada)");

Write("Enter an ISO culture code: ");
string? newCulture = ReadLine();

if (!string.IsNullOrEmpty(newCulture))
```

```
{
    CultureInfo ci = new(newCulture);
    
    // change the current cultures
    CultureInfo.CurrentCulture = ci;
    CultureInfo.CurrentUICulture = ci;
}
WriteLine();

Write("Enter your name: ");
string? name = ReadLine();

Write("Enter your date of birth: ");
string? dob = ReadLine();

Write("Enter your salary: ");
string? salary = ReadLine();

DateTime date = DateTime.Parse(dob);
int minutes = (int)DateTime.Today.Subtract(date).TotalMinutes;
decimal earns = decimal.Parse(salary);

WriteLine(
    "{0} was born on a {1:dddd}, is {2:N0} minutes old, and earns {3:C}",
    name, date, minutes, earns);
```

When you run an application, it automatically sets its thread to use the culture of the operating system. I am running my code in London, UK, so the thread is set to English (United Kingdom).

The code prompts the user to enter an alternative ISO code. This allows your applications to replace the default culture at runtime.

The application then uses standard format codes to output the day of the week using format code dddd; the number of minutes with thousand separators using format code N0; and the salary with the currency symbol. These adapt automatically, based on the thread's culture.

- 5. Run the code and enter en-GB for the ISO code and then enter some sample data including a date in a format valid for British English, as shown in the following output:

```
Enter an ISO culture code: en-GB
Enter your name: Alice
Enter your date of birth: 30/3/1967
Enter your salary: 23500
Alice was born on a Thursday, is 25,469,280 minutes old, and earns
£23,500.00
```

If you enter en-US instead of en-GB, then you must enter the date using month/day/year.

- 6. Rerun the code and try a different culture, such as Danish in Denmark, as shown in the following output:

```
Enter an ISO culture code: da-DK
Enter your name: Mikkel
Enter your date of birth: 12/3/1980
Enter your salary: 340000
Mikkel was born on a onsdag, is 18.656.640 minutes old, and earns
340.000,00 kr.
```

In this example, only the date and salary are globalized into Danish. The rest of the text is hardcoded as English. This book does not currently include how to translate text from one language to another. If you would like me to include that in the next edition, please let me know.

**Good Practice:** Consider whether your application needs to be internationalized and plan for that before you start coding! Write down all the pieces of text in the user interface that will need to be localized. Think about all the data that will need to be globalized (date formats, number formats, and sorting text behavior).

## Practicing and exploring

Test your knowledge and understanding by answering some questions, get some hands-on practice, and explore with deeper research into the topics in this chapter.

### Exercise 8.1 – Test your knowledge

Use the web to answer the following questions:

- 1. What is the maximum number of characters that can be stored in a string variable?
- 2. When and why should you use a SecureString type?
- 3. When is it appropriate to use a StringBuilder class?
- 4. When should you use a LinkedList<T> class?
- 5. When should you use a SortedDictionary<T> class rather than a SortedList<T> class?
