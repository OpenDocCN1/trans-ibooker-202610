
The advantages of using SSDT over SSMS for developers is the focus on development workflows and the integrations in the program: source control integration, project structuring, and object templates. This developer focus is further strengthened through the possibility of connecting the source control system to a build system and the option to extend the build system to include **continuous integration/deployment (CI/CD)**. Both automated builds and CI/CD have become ubiquitous in application development circles in the past decade. This area has only seen limited support for database development until now, because databases also permanently store data. Now that application development environments have matured, the ability to introduce CI/CD to database projects has become a reality. Luckily, the foundation for CI/CD has long been laid for application development and so the work to implement CI/CD into a database project is greatly reduced. SSDT is therefore fully capable of integrating SQL Server database projects into a source control system and to extend those projects into automated build, test, and deployment workflows.

There is now a wealth of options to cover CI/CD in the SQL Server world. The tools TeamCity for continuous integration and Octopus Deploy are two products that have been proven to work well in conjunction with SSDT to provide a smooth process for CI/CD in SQL Server projects.

![](img/07d1225e179d5df9f2b2d26b026386c6_63_0.png)

An interesting and useful website to visit for more information on topics on SSDT is the Microsoft *SQL Server Data Tools Team Blog* at `https://blogs.msdn.microsoft.com/ssdt/`.

## Tools for developing R and Python code

As you probably already know, SQL Server 2016 brings the support for the R language, and SQL Server 2017 adds support for the Python language. Of course, you need to have a development tool for the R code. There is a free version of the IDE tool called **RStudio IDE** that has been on the market for quite a long time. This is probably the most popular R tool. In addition, Microsoft is developing **R Tools for Visual Studio (RTVS)**, a plug-in for Visual Studio, which enables you to also develop R code in an IDE that is common and well known among developers that use Microsoft products and languages.

In this section, you will learn about:
- RStudio IDE
- R Tools for Visual Studio
- Using Visual Studio for data science applications

### RStudio IDE

The first tool you get to write and execute the R code in is the **R Console**. The console presents the greater than (>) sign as the prompt to the user. In the console, you write commands line by line, and execute them by pressing the *Enter* key. You have some limited editing capabilities in the console. For example, you can use the up and down arrow keys to retrieve the previous or the next command in the buffer. The following screenshot shows the console, with the demo ( ) command executed, which opens an Explorer window with a list of demo packages.

Because R is a functional package, you close the R Console with the q( ) function call. Anyway, you probably want to use a nicer, graphical environment. Therefore, it is time to introduce the RStudio IDE. The following screenshot shows the R console:

![](img/07d1225e179d5df9f2b2d26b026386c6_64_0.png)

R Console

RStudio is a company that is dedicated to helping the R community with its free and payable products (https://www.rstudio.com/). Their most popular product is the RStudio IDE, or as most R developers used to say, just RStudio. RStudio is available in open source and commercial editions, in both cases for desktop computers or for servers https://www.rstudio.com/products/rstudio/. The open source desktop edition, which is described in this section, is very suitable for starting developing in R. Already this edition has the majority of features built-in for smooth and efficient coding. This edition is described in this section.

You can download the RStudio IDE from the RStudio company site. The IDE supports Windows, macOS, and Linux. Once you install it, you can open it through a desktop shortcut, which points to the C:\Program Files\RStudio\bin\rstudio.exe file, if you used the defaults during the installation.

The RStudio screen is usually split into four panes if you open an R script file, as shown in the following figure.

![](img/07d1225e179d5df9f2b2d26b026386c6_65_0.png)

RStudio IDE

![](img/07d1225e179d5df9f2b2d26b026386c6_67_0.png)

RStudio IDE in action

Note that you can zoom the plot and save it in different graphical formats from the Plots pane.

There are literally dozens of keyboard shortcuts. It is impossible to memorize all of them. You memorize them by using them. Nevertheless, you don't need to know all of the shortcuts before you start writing R code. You can always get a quick reference of the keyboard shortcuts by pressing the Alt, Shift, and K keys at the same time. The keyboard Shortcut Quick Reference cheat sheet appears, as shown in the following screenshot. You can get rid of this cheat sheet by pressing the Esc key.

Please note that, although exhaustive, even this cheat sheet is not complete. In the top-right corner of the cheat sheet you can notice a link to even more shortcuts. Finally, it is worth mentioning that you can modify the pre-defined shortcuts and replace them with your own ones.

You have access to many of the keyboard shortcuts actions through the menus at the top of the RStudio IDE window. For example, in the Tools menu, you can find the link to the keyboard shortcuts cheat sheet. In the Help menu, you can find, besides the help options you would expect, the links to various cheat sheets, for example to the complete RStudio IDE cheat sheet, a PDF document you can download from https://www.rstudio.com/wp-content/uploads/2016/01/rstudio-IDE-cheatsheet.pdf at the RStudio site. The following screenshot illustrates the keyboard shortcuts cheat sheet:

![](img/07d1225e179d5df9f2b2d26b026386c6_68_0.png)

RStudio IDE keyboard shortcuts cheat sheet

Using the Panes menu, you can change the default layout of the panes. There are numerous options in the Code menu. For example, you can extract a function from the code. Figure Extracting a function shows an example where the Source pane is enlarged at the right side of the RStudio IDE window to show the path to the Extract Function option in the Code menu. The highlighted code is calculating the third and the fourth population moments for a continuous variable, the skewness and the kurtosis, as known from the descriptive statistics. When you extract a function from the code, you need to provide the name for the function, and RStudio extracts the necessary parameters from the code.

The following screenshot shows a debugging session in RStudio IDE. In the Source pane at the right of the window, you can see a breakpoint in line 19, near the `cnt <- 0` code. You can see also the line that is just about to execute, in this case line 22, with the code `kurt <- sum((p-avg)^4/stdev^4)/cnt-3`. In the Console pane at the bottom-left of the screen, you can see the **Next** button highlighted. This button executes the next line of the code. In the middle-left of the screen you can see the **Trackback** pane, and at the top left the **Environment** pane. You can see that the value of the variable `cnt` is 0, and because this variable is used in the denominator when calculating the `skewness`, the skew variable has an infinite value:

![](img/07d1225e179d5df9f2b2d26b026386c6_70_0.png)

After you find the error, you can stop debugging either from the **Debug** menu or with the *Shift* and F8 keyboard shortcut. Correct the code and run it to test it.

## R Tools for Visual Studio 2015

Microsoft is developing RTVS for those developers that are used to developing code in the most popular Microsoft IDE, Visual Studio. RTVS comes in two versions: as a free download for Visual Studio 2015 and already included in Visual Studio 2017. You can download RTVS for Visual Studio 2015 from https://www.visualstudio.com/vs/rtvs/. Visual Studio 2017 is covered in the next section of this chapter.

Once you install the RTVS you open the Visual Studio like you would open it for any other project. Of course, since SSDT is not a separate product-it is just another shortcut to the Visual Studio IDE-you can also open SSDT and get the R Tools menu besides other common Visual Studio menus.

With RTVS, you get most of the useful common panes of the RStudio IDE. You get the Source pane, the Console pane- which is called R Interactive in RTVS-and the Plots pane. The following figure shows the RTVS window with the Source, R Interactive, and the Plots pane open, showing the same plots for the variables from the iris demo dataset, shown in the figure RStudio IDE in action earlier in this section:

![](img/07d1225e179d5df9f2b2d26b026386c6_71_0.png)

If you are familiar with the Visual Studio IDE, then you might want to test RTVS.
