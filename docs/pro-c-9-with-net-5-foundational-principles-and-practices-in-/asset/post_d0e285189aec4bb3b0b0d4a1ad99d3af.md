
# Pro C# 9
with .NET 5

Foundational Principles and Practices
in Programming

Tenth Edition

Andrew Troelsen
Phillip Japikse

![](img/587021f4d5ba039be70a11107130377b_0_0.png)

# Pro C# 9 with .NET 5

Foundational Principles and Practices
in Programming

Tenth Edition

Andrew Troelsen

Phillip Japikse

# Pro C# 9 with .NET 5: Foundational Principles and Practices in Programming

Andrew Troelsen
Minneapolis, MN, USA

Phillip Japikse
West Chester, OH, USA

ISBN-13 (pbk): 978-1-4842-6938-1
https://doi.org/10.1007/978-1-4842-6939-8

ISBN-13 (electronic): 978-1-4842-6939-8

# Copyright © 2021 by Andrew Troelsen, Phillip Japikse

This work is subject to copyright. All rights are reserved by the Publisher, whether the whole or part of the material is concerned, specifically the rights of translation, reprinting, reuse of illustrations, recitation, broadcasting, reproduction on microfilms or in any other physical way, and transmission or information storage and retrieval, electronic adaptation, computer software, or by similar or dissimilar methodology now known or hereafter developed.

Trademarked names, logos, and images may appear in this book. Rather than use a trademark symbol with every occurrence of a trademarked name, logo, or image we use the names, logos, and images only in an editorial fashion and to the benefit of the trademark owner, with no intention of infringement of the trademark.

The use in this publication of trade names, trademarks, service marks, and similar terms, even if they are not identified as such, is not to be taken as an expression of opinion as to whether or not they are subject to proprietary rights.

While the advice and information in this book are believed to be true and accurate at the date of publication, neither the authors nor the editors nor the publisher can accept any legal responsibility for any errors or omissions that may be made. The publisher makes no warranty, express or implied, with respect to the material contained herein.

- Managing Director, Apress Media LLC: Welmoed Spahr
- Acquisitions Editor: Joan Murray
- Development Editor: Laura Berendson
- Coordinating Editor: Mark Powers

Cover designed by eStudioCalamar

Cover image by Shutterstock (www.shutterstock.com)

Distributed to the book trade worldwide by Apress Media, LLC, 1 New York Plaza, New York, NY 10004, U.S.A. Phone 1-800-SPRINGER, fax (201) 348-4505, e-mail orders-ny@springer-sbm.com, or visit www.springeronline.com. Apress Media, LLC is a California LLC and the sole member (owner) is Springer Science + Business Media Finance Inc (SSBM Finance Inc). SSBM Finance Inc is a Delaware corporation.

For information on translations, please e-mail booktranslations@springernature.com; for reprint, paperback, or audio rights, please e-mail bookpermissions@springernature.com.

Apress titles may be purchased in bulk for academic, corporate, or promotional use. eBook versions and licenses are also available for most titles. For more information, reference our Print and eBook Bulk Sales web page at www.apress.com/bulk-sales.

Any source code or other supplementary material referenced by the author in this book is available to readers on GitHub via the book’s product page, located at www.apress.com/9781484269381. For more detailed information, please visit www.apress.com/source-code.

Printed on acid-free paper

To my family, Amy, Conner, Logan, and Skylar,
thank you for all of the support and patience you have given me.
Also, to my father (Cor);
father, husband, inventor, and my center of excellence.
—Phil

# Table of Contents

About the Authors....................................................................................................................xlv

About the Technical Reviewers ...................................................................................................xlvii

Acknowledgments ....................................................................................................................xlix

Introduction............................................................................................................................li

Part I: Introducing C# and .NET 5 ........................................................................................... 1

Chapter 1: Introducing C# and .NET (Core) 5........................................................................... 3

Exploring Some Key Benefits of the .NET Core Platform....................................................... 4

Understanding the .NET Core Support Lifecycle...................................................................... 4

Previewing the Building Blocks of the .NET Core Platform (.NET Runtime, CTS, and CLS)................... 5

The Role of the Base Class Libraries ...................................................................................... 6

What C# Brings to the Table................................................................................................... 6

Managed vs. Unmanaged Code ............................................................................................... 10

Using Additional .NET Core–Aware Programming Languages................................................ 10

Getting an Overview of .NET Assemblies ............................................................................... 11

The Role of the Common Intermediate Language .................................................................... 11

Benefits of CIL ...................................................................................................................... 14

Compiling CIL to Platform-Specific Instructions..................................................................... 15

The Role of .NET Core Type Metadata.................................................................................... 15

The Role of the Assembly Manifest ....................................................................................... 16

Understanding the Common Type System............................................................................... 17

CTS Class Types.................................................................................................................... 17

CTS Interface Types .............................................................................................................. 18

# TABLE OF CONTENTS

- CTS Structure Types ........................................................... 18
- CTS Enumeration Types .......................................................... 19
- CTS Delegate Types ............................................................. 19
- CTS Type Members ............................................................... 19
- Intrinsic CTS Data Types ........................................................ 20
- Understanding the Common Language Specification ................................... 21
  - Ensuring CLS Compliance ....................................................... 22
- Understanding the .NET Core Runtime .............................................. 22
- Distinguishing Between Assembly, Namespace, and Type ............................. 23
  - Accessing a Namespace Programmatically ....................................... 24
  - Referencing External Assemblies .............................................. 25
- Exploring an Assembly Using ildasm.exe .......................................... 25
- Summary ........................................................................... 26
- Chapter 2: Building C# Applications ............................................... 27
- Installing .NET 5 ................................................................ 27
  - Understanding the .NET 5 Version Numbering Scheme ........................... 27
  - Confirming the .NET 5 Install ................................................ 28
- Building .NET Core Applications with Visual Studio .............................. 29
  - Installing Visual Studio 2019 (Windows) ...................................... 29
  - Taking Visual Studio 2019 for a Test-Drive ................................... 31
- Building .NET Core Applications with Visual Studio Code ........................ 41
  - Taking Visual Studio Code for a Test-Drive ................................... 41
- Finding the .NET Core and C# Documentation ...................................... 45
- Summary ........................................................................... 45
- Part II: Core C# Programming ..................................................... 47
- Chapter 3: Core C# Programming Constructs, Part 1 ............................... 49
- Breaking Down a Simple C# Program ............................................... 49
  - Using Variations of the Main() Method (Updated 7.1) .......................... 51
  - Using Top-Level Statements (New 9.0) ......................................... 52
  - Specifying an Application Error Code (Updated 9.0) ........................... 53
