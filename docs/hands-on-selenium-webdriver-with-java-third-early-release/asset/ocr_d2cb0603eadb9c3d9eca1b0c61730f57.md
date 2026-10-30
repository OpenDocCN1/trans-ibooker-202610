themselves automatically). Third, these browsers are available for the major operating systems: Windows, Linux, and macOS (unlike Safari, which is also a popular browser but is only available on macOS). Lastly, these browsers are available in the CI environment used in the GitHub repository (i.e., GitHub Actions).

The last requirement for controlling web browsers with Selenium WebDriver is the driver binaries: chromedriver (for Chrome), msedgedriver (for Edge), and geckodriver (for Firefox). As discussed in Chapter 1, driver management involves three steps: download, setup, and maintenance. To avoid the potential problems explained in that chapter, I strongly recommend automating this process with WebDriverManager.

> **TIP**
> 
> *Appendix 2 provides fine-grained details about the automated driver management process performed by WebDriverManager. In addition, and just in case you need it for some reason, this appendix explains how to carry out the driver management manually.*

# Build Tools

Another important component is the *build tool*. Build tools are software utilities used to automate the creation of executable applications from source code. These tools ease the project management in terms of dependencies management, compilation, packaging, test execution, and deployment. Overall, build tools are a convenient way to automate the development of software projects, both in build servers (e.g., GitHub Actions) and developer machines. Therefore, I highly recommend using a build tool to set up your project. The alternatives we cover in this book are:

+   - Maven is an open-source build automation tool maintained by the Apache Software Foundation. It is used primarily for Java projects, although it also supports other languages such as C#, Ruby, or Scala.
+   - Gradle is another open-source build automation tool for software development. It supports Java and other languages such as Kotlin, Groovy, Scala, C/C++, or JavaScript.