
Contains a set of folders and files for the so-called Gradle *wrapper*. This wrapper is a script file (called `gradlew` for Unix-like systems and `gradlew.bat` for Windows) that provides the following benefits:

- Build a project without the need to install Gradle on the local machine.
- Force the use of a given version, which can be different from the locally installed instance of Gradle.
- Upgrade to a new version easily by changing the wrapper artifacts in the `gradle/wrapper` folder.

> **NOTE**
>
> It is out of the scope of this book to explain all the features provided by Maven and Gradle. Nevertheless, you can find more information about their build lifecycles and typical commands. For further information, consider reading the official Maven and Gradle documentation.

## Dependencies

The dependencies of a software project are the required libraries or plugins. Among other features, build tools enable the automated management of project dependencies. To that aim, we need to specify the *coordinates* of such dependencies in the project configuration file (see the following subsections for specifics on Maven and Gradle). The coordinates of a Java project are a group of three labels that univocally identify this project (e.g., a library, plugin, etc.), namely:

- `groupId`: Organization, company, person, etc., that created the project.
- `artifactId`: Unique name that identifies the project.
- `version`: Particular release of the project. By default, I recommend that you use the latest version of every release. For that reason, I use Dependabot to maintain these versions automatically.

## Semantic Versioning

Nowadays, a popular way to select a project version is **Semantic Versioning**, also called *SemVer*. According to the SemVer manifesto, a release has three parts separated by dots, i.e., `MAJOR.MINOR.PATCH`. In a nutshell, `MAJOR` identifies incompatible changes, `MINOR` identifies backward-compatible changes, and `PATCH` identifies bug fixes.

The rest of this section explains the Java dependencies I use in the examples repository. First, of course, we need Selenium WebDriver to carry out browser automation. This dependency is the only one strictly mandatory. Then, I recommend using additional dependencies for automated driver management utilities, a unit testing framework, fluent assertions, and logging. The remainder of this section explains the motivation and basic usage of each one of these utilities.

## Selenium WebDriver

One of the most relevant concepts of Selenium WebDriver is the `WebDriver` hierarchy, which is a collection of classes aimed at controlling different web browsers. As you can see in **Figure 2-2**, this hierarchy follows the object-oriented programming paradigm. At the top, we find the `WebDriver` interface, which is the parent of the whole structure. The lower part of the hierarchy corresponds to Java classes that allow driving individual browsers. For instance, we need to use an instance of the class `ChromeDriver` to control a local Chrome browser. **Table 2-1** shows a comprehensive summary of the main classes of the WebDriver hierarchy and their corresponding target browsers.

![](img/b67ec8e49249e340add6ed66ab413d78_68_0.png)

*Figure 2-2. Hierarchy of the WebDriver object*

**Table 2-1. Description of the WebDriver hierarchy**

| Package | Class | Browser |
|---|---|---|
| `org.openqa.selenium.chrome` | `ChromeDriver` | Chrome |
| `org.openqa.selenium.edge` | `EdgeDriver` | Edge |
| `org.openqa.selenium.firefox` | `FirefoxDriver` | Firefox |
| `org.openqa.selenium.safari` | `SafariDriver` | Safari |
| `org.openqa.selenium.opera` | `OperaDriver` | Opera |
| `org.openqa.selenium.ie` | `InternetExplorerDriver` | Internet Explorer |
| `org.openqa.selenium.remote` | `RemoteWebDriver` | Remote browsers (see *Chapter 6*) |

## Automated Driver Management

Before instantiating an object of the WebDriver hierarchy, it is mandatory to resolve its corresponding driver. For example, to control Chrome with ChromeDriver, first, we need to manage chromedriver. To avoid the potential problems related to manual driver management (see Chapter 1), I recommend carrying out the whole driver management process (download, setup, and maintenance) in an automated manner. Concerning Java, the reference implementation is **WebDriverManager**, a Selenium WebDriver helper library that allows driver management in an automated fashion. This section explains how to use WebDriverManager as a Java dependency.

Once the WebDriverManager dependency is resolved in our project (see *Appendix 3* for the configuration details), we can use the WebDriverManager API to manage drivers. This API provides a set of singletons, called *managers*, to download, set up, and maintain drivers. These singletons are accessible through the WebDriverManager class. For instance, we need to invoke the method *chromedriver()* to manage the driver required by Chrome, i.e., chromedriver, as follows:

```java
WebDriverManager.chromedriver().setup();
WebDriver driver = new ChromeDriver();
```

**Table 2-2** summarizes the basic WebDriverManager calls for all the supported browsers. In addition to these basic calls (i.e., the method `setup()`), WebDriverManager exposes a fluent API for advanced configuration. See *Appendix 2* for more details about the WebDriverManager methodology, configuration capabilities, and other usages, such as a Command Line Interface (CLI) tool (from the shell), as a server (using a REST-like (Representational State Transfer) API), as an agent (using Java instrumentation), or as a Docker container.

**Table 2-2. WebDriverManager basic call**

| WebDriverManager basic call | Browser | Driver |
|---|---|---|
| `WebDriverManager.chromedriver().setup();` | Chrome | chromedriver |
| `WebDriverManager.edgedriver().setup();` | Edge | msedgedriver |
| `WebDriverManager.firefoxdriver().setup();` | Firefox | geckodriver |
| `WebDriverManager.operadriver().setup();` | Opera | operadriver |
| `WebDriverManager.chromiumdriver().setup();` | Chromium | chromedriver |
| `WebDriverManager.iedriver().setup();` | Internet Explorer | IEDriverServer |

## Unit Testing Frameworks

As explained in Chapter 1, unit testing frameworks are the basis for creating different types of tests. In this book, you will learn how to implement end-to-end tests for web applications using Selenium WebDriver. Hence, I suggest embedding the Selenium WebDriver calls within tests created with a particular unit testing framework. The alternatives I recommend you use are one of these options: JUnit 4, JUnit 5 (alone or in conjunction with Selenium-Jupiter, which is an extension for Selenium WebDriver), or TestNG. The following subsections provide more details about these alternatives. My advice is to focus on the unit testing framework and the build tool you prefer to continue practicing with the examples presented in the rest of the book.

## JUnit 4

JUnit is a unit testing framework for Java created by Erich Gamma and Kent Beck in 1999. It is considered by many to be the de facto standard framework for developing tests in Java. In JUnit, a test is a method within a Java class used for testing. As of JUnit 4, the building blocks for developing JUnit tests are Java annotations. The fundamental annotation of JUnit 4 is `@Test`, since it allows identifying the method or methods that contain the test logic (i.e., the code used to exercise and verify a piece of software). In addition, there are other annotations to identify the methods used for setup (i.e., what happens before the tests) and teardown (i.e., what happens after the tests).

- `@BeforeClass` is executed once before all tests.
- `@Before` is executed before each test.
- `@After` is executed after each test.
- `@AfterClass` is executed once after all tests.

Figure 2-3 shows a graphical representation of the basic test lifecycle in JUnit 4.

![](img/b67ec8e49249e340add6ed66ab413d78_74_0.png)

*Figure 2-3. JUnit 4 test lifecycle*

## JUnit 5

Due to several limitations in JUnit 4 (such as monolithic architecture or the impossibility of composing JUnit runners), the JUnit team released a new major version (i.e., JUnit 5) in 2017. JUnit was entirely redesigned in version 5, following a modular architecture consisting of three components (see Figure 2-4). The first component is the JUnit *Platform*, which is the foundation of the whole framework. The goal of the JUnit Platform is twofold:

- It allows the discovery and execution (sequential or parallel) of tests in the JVM through the *test launcher* API. This API is typically used by programmatic clients such as build tools and IDEs.
