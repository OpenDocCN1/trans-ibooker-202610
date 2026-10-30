Note  The concept of inheritance will be fully examined in Chapter 6.

To complete this example, update the generated SportsCar class with a public method named GetPetName(), authored as follows:

```
public class SportsCar : Car
{
    public string GetPetName()
    {
        PetName = "Fred";
        return PetName;
    }
}
```

As you would expect, the designer shows the added method to the SportsCar class.
This concludes your first look at Visual Studio. Throughout this text, you will see more examples of using Visual Studio to build C# 9 and .NET 5 applications.

# Building .NET Core Applications with Visual Studio Code

Another popular IDE from Microsoft is Visual Studio Code (VSC). Visual Studio Code is a relatively new edition to the Microsoft family; is free, open source, and cross-platform; and has gained significant adoption among developers in and out of the .NET Core ecosystem. The focus of Visual Studio Code is (as the name suggests) the code of your application. It does not have many of the built-in features that are included in Visual Studio. However, there is additional functionality that can be added into Visual Studio Code through extensions. This allows you to have a fast IDE customized for your workflow. Many samples in this book are built and tested with Visual Studio Code. You can download it here:

https://code.visualstudio.com/download

After installing VSC, you will want to add the C# extension found here:

https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp

Note  Visual Studio Code is used to develop many different types of applications based on a multitude of languages. There are extensions for Angular, View, PHP, Java, and many, many, more.

# Taking Visual Studio Code for a Test-Drive

Let’s take a quick look at Visual Studio Code by building the same .NET 5 Console application from the Visual Studio example.