
The event handler is empty, so it doesn’t contain any specific code to execute when the page loads.

The section of code that starts with `Protected Sub Button1_Click` is an event handler for the `click` event of a control with an ID of `Button1` (as specified in the `Handles Button1.Click` part). This event is triggered when the user clicks the button. Here’s what the code inside the event handler does:

- It uses the `CInt` function to convert the text entered in a control with an ID of `tbGrade` to an integer and adds it to the value stored in the `Total` session variable.
- It increments the value stored in the `Cnt` session variable by `1`.
- It calculates the average grade by dividing the total sum stored in the `Total` session variable by the count stored in the `Cnt` session variable.
- The calculated average is then assigned to the `Text` property of a control with an ID of `lblAverage`, which will display the calculated average on the web page.

The code uses session variables to store and maintain the running total and count of grades across different requests and interactions. The calculated average is dynamically displayed on the web page when the user clicks the `Button1` button.

## Coding the ASP.NET server-side grade averager with Visual Studio

Follow these steps to code and execute an ASP.NET web page that displays `Hello World`:

- 1. Start **Microsoft Visual Studio 2019**.
- 2. Choose **Create New Project**.
- 3. Choose **ASP.NET Web Application** and click **Next**.
- 4. Accept **Default Project Name** and **Solution Name**.
- 5. Change the location if you want your code to be in a different folder.
- 6. Accept the default **Framework** option.
- 7. Click **Create Project**.
- 8. Choose **Web Forms**.
- 9. When the designer loads, navigate to **Solution Explorer** and right-click on the solution.
- 10. Choose **Add Item** from the pop-up menu.
- 11. Choose **Web Form Visual Basic**.
- 12. Choose **Source**.
- 13. Paste in the ASPX code within the **FORM** tag.
- 14. Right-click on the ASPX source and choose **View Code** from the pop-up menu.
- 15. Enter the VB.NET code example from the previous section that falls in the `Button1_click` procedure.
- 16. To run your program, click the green triangle on the toolbar above your code.

Try to internalize the similarities between the Classic ASP and ASP.NET versions of the code. As you can see, ASP.NET separates the HTML from VB to try to make maintenance easier. This becomes very important as the project’s size grows.

## Summary

In this chapter, we utilized the skills we gained in the previous chapters regarding the HTTP request-response model, variable scope, and concurrency to build a server-side student grade averaging tool for both **Classic ASP** and **ASP.NET**. In the next chapter, we will conclude this book by summarizing what we have covered, thinking about other VB topics you can study, and projects you can develop to demonstrate the skills you’ve learned throughout this book.

## Conclusions

We have covered many different **Visual Basic (VB)** topics in this book. I want to close out this book by summarizing what we have learned while suggesting other areas of interest for you to study around VB and some suggestions for projects you could do to show a potential employer your new skills.

In this chapter, we’re going to cover the following main topics:

- Summing it all up
- Further reading topics
- Suggested projects

### Summing it all up

Throughout this book, we’ve looked at the use of VB in many different environments, including Windows Desktop development with Classic VB and VB.NET Console. We also looked at using VB as a server-side programming language in **Classic ASP** and **ASP.NET**. **Visual Basic for Applications (VBA)** was also used to embed VB in Microsoft Office products, and we explored using VB in scripts with **VBScript.**

In this section, we’ll provide a brief review of the major topics we covered in this book.

In VB, variable declaration, type, scope, and assignment are fundamental concepts that are used to work with data and manage program logic. Here’s an overview of each of these concepts.

### Variable declaration

In VB, you declare variables using the `Dim` (short for **Dimension**) keyword. Variable declaration reserves memory space for storing data and associates a name with it. The basic syntax for declaring a variable is as follows:

```vb
Dim variableName As DataType
```

### Data types

VB supports various data types to represent different kinds of values. Some common data types include the following:

- **Integer**: Represents whole numbers.
- **Double** or **Single**: Represents floating-point numbers.
- **String**: Represents text.
- **Boolean**: Represents true or false values.
- **Date**: Represents dates and times.
- **Object**: Represents an instance of a class.

### Scope

The scope of a variable refers to where in the code the variable is accessible and can be used. There are different levels of scope in VB:

- **Procedure-level scope**: Only variables declared within a procedure (function or subroutine) are accessible.
- **Module-level scope**: Variables declared at the module level (outside of any procedure) are accessible to all procedures within that module.
- **Global scope**: Variables declared in a module with the **Public** keyword are accessible from any module or form in the project.

### Assignment

Assignment involves storing a value in a variable. You can assign values to variables using the assignment operator (=). Here’s an example:

```vb
age = 25
name = "John"
```

### Iteration

Iteration, also known as looping, is a fundamental programming concept that allows you to execute a block of code repeatedly. In VB, you can use various constructs to implement iteration. Here are some standard methods for iteration:

- **The For...Next loop**: The **For...Next** loop is used when you know the number of iterations in advance. It iterates through a range of values defined by a starting, ending, and optional step value:

  ```vb
  For counter As Integer = startValue To endValue Step stepValue
  Next
  ```

- **The While loop**: The **While** loop repeats a code block, so long as a specified condition remains true:

  ```vb
  While condition
      ' Code to be executed in each iteration
  End While
  ```

- **The Do...Loop loop**: The **Do...Loop** loop is similar to the **While** loop, but the condition is checked at the end. This means the loop’s code block is always executed at least once:

  ```vb
  Do
      ' Code to be executed in each iteration
  Loop While condition
  ```

- **The For Each loop**: The **For Each** loop is used to iterate through elements in a collection, such as an array or a collection class.

- **Sub procedures**: Sub procedures perform a task and do not return a value:

  ```vb
  ' Code to perform a specific task
  End Sub
  ```

- **Function procedures**: Function procedures, or functions, are similar to subs but return a value. You can use them when you need to calculate or derive a result:

  ```vb
  Function FunctionName(parameters) As DataType
      ' Code to calculate and return a value
  End Function
  ```

### Collections

In VB, data structures allow you to store and manage multiple elements of the same or different data types. Collections provide a convenient way to work with data groups and offer various methods for adding, removing, and accessing elements. VB provides several built-in collection classes that you can use to accomplish different tasks. We will not summarize the collection types here as they differ by VB family member, but you should revisit [Chapter 10](#) for a recap.

### File handling

File handling in VB involves reading from and writing to files on your computer’s filesystem. Each VB family member provides various classes and methods for handling files, allowing you to create, open, read, and write files. You can revisit [Chapter 9](#) for a recap.

### Object encapsulation

Object encapsulation, also known as encapsulation or information hiding, is a fundamental principle of **object-oriented programming (OOP)** that emphasizes bundling data (attributes) and the methods (functions or procedures) that operate on that data into a single unit called an object. Encapsulation helps control access to an object’s internal state and promotes modularization and abstraction. Please revisit [Chapter 12](#) to learn how to encapsulate variables in the different VB family members.

### Inheritance

Inheritance is a fundamental concept in OOP that allows you to create new classes (called subclasses or derived classes) based on existing classes (called base classes or parent classes). Inheritance enables you to reuse and extend the functionality of existing classes, promoting code reusability and hierarchy. In VB, you can implement inheritance using the **Inherits** keyword or interface inheritance with
