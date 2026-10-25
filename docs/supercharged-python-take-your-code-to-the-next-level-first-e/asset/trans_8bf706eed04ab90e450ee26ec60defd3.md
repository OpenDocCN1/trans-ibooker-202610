

# TAKE YOUR CODE TO THE NEXT LEVEL

# SUPERCHARGED PYTHON

BRIAN OVERLAND | JOHN BENNETT

## About This eBook

ePUB 是一个开放的、行业标准的电子书格式。但是，对 ePUB 及其众多功能的支持因阅读设备和应用程序而异。使用设备或应用程序的设置来自定义呈现方式，以符合您的喜好。可自定义的设置通常包括字体、字号、单栏或双栏、横向或纵向模式，以及可以点击或轻触以放大的图形。有关阅读设备或应用程序上的设置和功能的更多信息，请访问设备制造商的网站。

许多标题包含编程代码或配置示例。为了优化这些元素的呈现效果，请以单栏、横向模式查看电子书，并将字体大小调整为最小设置。除了以可重排文本格式呈现代码和配置外，我们还包含了模拟印刷书籍中呈现效果的代码图像；因此，在可重排格式可能会影响代码清单呈现效果的地方，您将看到“Click here to view code image”链接。单击该链接以查看保真度高的代码图像。要返回之前查看的页面，请单击设备或应用程序上的“Back”按钮。

## Supercharged Python

Brian Overland
John Bennett

Addison-Wesley

Boston • Columbus • New York • San Francisco • Amsterdam • Cape Town
Dubai • London • Madrid • Milan • Munich • Paris • Montreal
• Toronto • Delhi • Mexico City
São Paulo • Sydney • Hong Kong • Seoul • Singapore • Taipei • Tokyo

制造商和卖家用于区分其产品的许多名称均被声明为商标。当这些名称出现在本书中，且出版商知晓商标声明时，这些名称已以首字母大写或全大写形式印刷。

作者和出版商在编写本书时已尽力确保准确性，但不做任何明示或暗示的任何种类的担保，也不对任何错误或遗漏承担责任。对于与本文所含信息或程序的使用相关或由此产生的偶然或后果性损害，不承担任何责任。

如需了解批量购买本书或特殊销售机会（可能包括电子版本、定制封面设计以及特定于您的业务、培训目标、营销重点或品牌兴趣的内容）的信息，请联系我们的企业销售部门，电子邮件：corpsales@pearsoned.com，电话：(800) 382-3419。

如需政府销售咨询，请联系 governmentsales@pearsoned.com。

如需了解美国境外销售问题，请联系 intlcs@pearson.com。

访问我们的网站：informit.com/aw

国会图书馆控制编号：2019936408

版权所有 © 2019 Pearson Education, Inc.

封面插图：Open Studio/Shutterstock

版权所有。本出版物受版权保护，在未获得出版商事先许可的情况下，禁止以任何形式或任何方式（电子、机械、复印、录制或其他方式）进行复制、存储在检索系统中或传输。有关许可信息、请求表格以及 Pearson Education Global Rights & Permissions 部门内的相应联系人，请访问 www.pearsoned.com/permissions/。

ISBN-13: 978-0-13-515994-1
ISBN-10: 0-13-515994-6

1 19

> To my beautiful and brilliant mother, Betty P. M. Overland. . . .
> All the world is mad except for me and thee. Stay a little.
> —Brian

> To my parents, who did so much to shape who I am.
> —John

## Contents

### Preface

#### What Makes Python Special?

#### Paths to Learning: Where Do I Start?

#### Clarity and Examples Are Everything

#### Learning Aids: Icons

#### What You'll Learn

#### Have Fun

### Acknowledgments

### About the Authors

## Chapter 1 Review of the Fundamentals

### 1.1 Python Quick Start

### 1.2 Variables and Naming Names

### 1.3 Combined Assignment Operators

### 1.4 Summary of Python Arithmetic Operators

### 1.5 Elementary Data Types: Integer and Floating Point

### 1.6 Basic Input and Output

### 1.7 Function Definitions

### 1.8 The Python “if” Statement

### 1.9 The Python “while” Statement

### 1.10 A Couple of Cool Little Apps

### 1.11 Summary of Python Boolean Operators

### 1.12 Function Arguments and Return Values

### 1.13 The Forward Reference Problem

### 1.14 Python Strings

### 1.15 Python Lists (and a Cool Sorting App)

### 1.16 The “for” Statement and Ranges

### 1.17 Tuples

### 1.18 Dictionaries

### 1.19 Sets

### 1.20 Global and Local Variables

### Summary

### Review Questions

### Suggested Problems

## Chapter 2 Advanced String Capabilities

### 2.1 Strings Are Immutable

### 2.2 Numeric Conversions, Including Binary

### 2.3 String Operators (+, =, *, >, etc.)

### 2.4 Indexing and Slicing

### 2.5 Single-Character Functions (Character Codes)

### 2.6 Building Strings Using “join”

### 2.7 Important String Functions

### 2.8 Binary, Hex, and Octal Conversion Functions

### 2.9 Simple Boolean (“is”) Methods

### 2.10 Case Conversion Methods

### 2.11 Search-and-Replace Methods

### 2.12 Breaking Up Input Using “split”

### 2.13 Stripping

### 2.14 Justification Methods

### Summary

### Review Questions

### Suggested Problems

## Chapter 3 Advanced List Capabilities

### 3.1 Creating and Using Python Lists

### 3.2 Copying Lists Versus Copying List Variables

### 3.3 Indexing

#### 3.3.1 Positive Indexes

#### 3.3.2 Negative Indexes

#### 3.3.3 Generating Index Numbers Using “enumerate”

### 3.4 Getting Data from Slices

### 3.5 Assigning into Slices

### 3.6 List Operators

### 3.7 Shallow Versus Deep Copying

### 3.8 List Functions

### 3.9 List Methods: Modifying a List

### 3.10 List Methods: Getting Information on Contents

### 3.11 List Methods: Reorganizing

### 3.12 Lists as Stacks: RPN Application

### 3.13 The “reduce” Function

### 3.14 Lambda Functions

### 3.15 List Comprehension

### 3.16 Dictionary and Set Comprehension

### 3.17 Passing Arguments Through a List

### 3.18 Multidimensional Lists

#### 3.18.1 Unbalanced Matrixes

#### 3.18.2 Creating Arbitrarily Large Matrixes

### Summary

### Review Questions

### Suggested Problems

## Chapter 4 Shortcuts, Command Line, and Packages

### 4.1 Overview

### 4.2 Twenty-Two Programming Shortcuts

#### 4.2.1 Use Python Line Continuation as Needed

#### 4.2.2 Use “for” Loops Intelligently

#### 4.2.3 Understand Combined Operator Assignment (+= etc.)

#### 4.2.4 Use Multiple Assignment

#### 4.2.5 Use Tuple Assignment

#### 4.2.6 Use Advanced Tuple Assignment

#### 4.2.7 Use List and String “Multiplication”

#### 4.2.8 Return Multiple Values

#### 4.2.9 Use Loops and the “else” Keyword

#### 4.2.10 Take Advantage of Boolean Values and “not”

#### 4.2.11 Treat Strings as Lists of Characters

#### 4.2.12 Eliminate Characters by Using “replace”

#### 4.2.13 Don’t Write Unnecessary Loops

#### 4.2.14 Use Chained Comparisons (n < x < m)

#### 4.2.15 Simulate “switch” with a Table of Functions

#### 4.2.16 Use the “is” Operator Correctly

#### 4.2.17 Use One-Line “for” Loops

#### 4.2.18 Squeeze Multiple Statements onto a Line

#### 4.2.19 Write One-Line if/then/else Statements

#### 4.2.20 Create Enum Values with “range”

#### 4.2.21 Reduce the Inefficiency of the “print” Function Within IDLE

#### 4.2.22 Place Underscores Inside Large Numbers

### 4.3 Running Python from the Command Line

#### 4.3.1 Running on a Windows-Based System

#### 4.3.2 Running on a Macintosh System

#### 4.3.3 Using pip or pip3 to Download Packages

### 4.4 Writing and Using Doc Strings

### 4.5 Importing Packages

### 4.6 A Guided Tour of Python Packages

### 4.7 Functions as First-Class Objects

### 4.8 Variable-Length Argument Lists

#### 4.8.1 The *args List

#### 4.8.2 The “**kwargs” List

### 4.9 Decorators and Function Profilers

### 4.10 Generators

#### 4.10.1 What’s an Iterator?

#### 4.10.2 Introducing Generators

### 4.11 Accessing Command-Line Arguments

### Summary

### Questions for Review

### Suggested Problems

## Chapter 5 Formatting Text Precisely

### 5.1 Formatting with the Percent Sign Operator (%)

### 5.2 Percent Sign (%) Format Specifiers

