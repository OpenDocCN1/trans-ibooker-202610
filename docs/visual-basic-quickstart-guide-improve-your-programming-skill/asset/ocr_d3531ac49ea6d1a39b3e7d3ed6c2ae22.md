[US/Default.aspx](US/Default.aspx).

Next up, we will examine how to plug the example code into Visual Basic 6 and run the program.

# Running the Visual Basic 6 Hello, World! program

The following steps will allow you to code and execute a Visual Basic 6 version of **Hello World!:**

+   1. Start **Microsoft Visual Basic 6**.
+   2. Choose **Standard EXE** as the project type.
+   3. When the designer loads, double-click inside the rectangle representing your form.
+   4. Enter the **MsgBox** line from the code example seen earlier.
+   5. Click the green triangle on the toolbar above your code to run your program.

Let’s move on to look at VBA.

# Programming with VBA

VBA is a programming language that automates tasks in Microsoft Office applications, such as Excel, Word, PowerPoint, and Access. VBA is primarily used to automate repetitive tasks and interact with external applications.

VBA is a powerful language that can import and translate data, automate sequences of calculations, and create custom forms and specialized reports. These powers lead VBA to be used by finance professionals and data analysts who work with large amounts of data and perform complex analyses. VBA is a powerful language that allows users to create functions, perform error handling, and use advanced programming techniques such as loops and conditional statements. VBA is also licensed from Microsoft or reproduced by third-party companies. A small sample of applications that host VBA includes **WordPerfect**, **LibreOffice**, and **CorelDRAW**.

# Hello, World! in VBA

VBA code is stored in text files with an extension of **.bas** when exported from the host. Often, the code is stored in the host document, depending on the application host. The following is sample VBA macro code, with a macro named **test**. Running the macro will display a message box that displays **Hello, World!** on startup:

```
Sub test()
```