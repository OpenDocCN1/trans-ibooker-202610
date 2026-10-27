# Project Part I

In our first project, we want to use the skills we have learned to build a simple calculator for each VB family member. The calculator should allow the user to enter a simple mathematical operation such as addition, subtraction, multiplication, or division. The calculator should also enable the user to enter the two operands used in the calculation. The calculator will then show the result and allow for another calculation.

In this chapter, we’re going to cover the following main topics:

+   - Developing a calculator in the VB.NET console
+   - Developing a calculator in VB6
+   - Developing a calculator in Visual Basic for Applications (VBA)
+   - Developing a calculator in VBScript
+   - Developing a calculator in Classic ASP
+   - Developing a calculator in ASP.NET

# Technical requirements

Please complete the steps to install and code with the Visual Basic (VB) family member from Chapter 1. The code for this chapter is available in the GitHub repository for this book: https://github.com/PacktPublishing/Learn-Visual-Basics-Quick-Start-Guide-/tree/main/ProjectPartI.

# Building the project in the VB.NET console

This simple console application written in VB.NET programming language performs basic arithmetic operations on two input numbers.

The program first imports the System namespace. Then, it defines a module called program, which contains a single Sub called Main that takes an array of String arguments.

Within the Main method, the program declares five variables: n1, n2, oper of type String, a res of type String initialized to an empty string, and a sentinel of type String initialized to False.

The program enters a Do-While loop until the sentinel is set to True. Within the loop, the program prompts the user to enter the first number or x to quit. If the user enters x, the sentinel variable is set to True, and the loop ends. Otherwise, the program prompts the user to enter the second number and the arithmetic operator.