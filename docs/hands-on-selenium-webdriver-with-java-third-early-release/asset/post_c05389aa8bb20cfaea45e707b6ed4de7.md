
# Table 1-9. Unit testing frameworks

| Name | Language | Description | License | Maintainer | Website |
|---|---|---|---|---|---|
| JUnit | Java | Reference implementation of xUnit family | EPL | JUnit team | https://junit.org/ |
| TestNG | Java | Inspired by JUnit and NUnit, including extra features | Apache 2.0 | Cedric Beust | https://testng.org/ |
| Mocha | JavaScript | Test framework for Node.js and the browser | MIT | OpenJS Foundation | https://mochajs.org/ |
| Jest | JavaScript | Focused on simplicity with a focus on web applications | MIT | Facebook | https://jestjs.io/ |
| Karma | JavaScript | It allows you to execute JavaScript tests in web browsers | MIT | Karma team | http://karma-runner.github.io/ |
| NUnit | .Net | Unit-testing framework for all .Net languages (C#, Visual Basic, and F#) | MIT | .NET Foundation | https://nunit.org/ |
| unittest | Python | Unit testing framework included as a standard library as of Python 2.1 | PSF License | Python Software Foundation | https://docs.python.org/library/unittest.html |
| minitest | Ruby | Complete suite of testing utilities for Ruby | MIT | Seattle Ruby Brigade | https://github.com/seattlers/minitest |

An important common characteristic of the xUnit family is the test structure, composed of four phases (see Figure 1-9):

- *Setup.* The test case initializes the SUT to exhibit the expected behavior.
- *Exercise.* The test case interacts with the SUT. As a result, the test gets an outcome from the SUT.
- *Verify.* The test case decides if the obtained outcome from the SUT is as expected. To that aim, the test contains one or more assertions. An *assertion* (or predicate) is a boolean-valued function that compares if an expected condition is true. The execution of the assertions generates a test verdict (typically, pass or fail).
- *Teardown.* The test case put the SUT back into the initial state.

![](img/b67ec8e49249e340add6ed66ab413d78_50_0.png)

*Figure 1-9. Unit test generic structure*

**TIP**

We can use unit testing frameworks in conjunction with other libraries or utilities to implement any test type. For example, as explained in Chapter 2, we use JUnit and TestNG to embed the call to the Selenium WebDriver API, implementing this way end-to-end tests for web applications.

The stages of setup and teardown are optional in a unit test case. The step of verifying, although it is not strictly mandatory, is highly recommended. Even if unit testing frameworks include capabilities to implement assertions, it is common to incorporate third-party *assertions libraries*. These libraries aim to improve the test code readability by providing a rich set of fluent assertions. Besides, these libraries offer enhanced error messages to help testers to understand the cause of a failure. Table 1-10 contains a summary of some of the most relevant assertions libraries for Java.

| Hamcrest | Java library of matchers aimed to create flexible assertions | BSD | Hamcrest team | http://hamcrest.org/ |
| Truth | Fluent assertions for Java and Android | Apache 2.0 | Google | https://truth.dev/ |

As you can see in Figure 1-9, the SUT usually can query another component, named the *Depended-On Component* (DOC). In some cases (e.g., at the unit or system testing level), we might want to isolate the SUT from the DOC(s). We can find a wide variety of mocking libraries to achieve this isolation. Table 1-11 shows a comprehensive summary of some of these mock libraries for Java.

Table 1-11. Mock libraries for Java

| Name | Level | Description | License | Maintainer | Website |
|---|---|---|---|---|---|
| EasyMock | Unit | It allows to mock objects for unit testing using Java annotations | Apache | EasyMock team | https://easymock.org/ |
| Mockito | Unit | Mocking Java library for mock creation and verification | MIT | Mockito team | https://site.mockito.org/ |
| JMockit | Integration | It allows out-of-container integration testing for Java EE and Spring-based apps | Open | JMockit team | https://jmockit.github.io/ |
| MockServer | System | Mocking library for any system integrated via HTTP or HTTPS with Java clients | Apache 2.0 | James D. Bloom | https://www.mock-server.com/ |
| WireMock | System | Tool for simulate HTTP-based services | Apache 2.0 | Tom Akehurst | http://wiremock.org/ |

The last category of testing tools we analyze in this section is related to BDD. As you know, BDD is a development process aimed to create acceptance tests. There are plenty of alternatives to implement this approach. For instance, Table 1-12 shows a condensed summary of different relevant BDD frameworks.

# Table 12. BDD frameworks

| Name | Language | Description | License | Maintainer | Website |
|---|---|---|---|---|---|
| Cucumber | Ruby, Java, JavaScript, Python | Testing framework to created automated acceptance tests following a BDD approach. | MIT | SmartBear Software | https://cucumber.io/ |
| FitNesse | Java | Standalone collaborative wiki and acceptance testing framework | CPL | FitNesse team | http://fitnesse.org/ |
| JBehave | Java | BDD framework for all JVM languages (Java, Groovy, Kotlin, Ruby, Scala) | BSD-3-Clause | JBehave team | https://jbehave.org/ |
| Jasmine | JavaScript | BDD framework for JavaScript code | MIT | Jasmine team | https://jasmine.github.io/ |
| Capybara | Ruby | Web-based acceptance test framework that simulates scenarios for user stories | MIT | Thomas Walpole | https://teamcapybara.github.io/capybara |

# Summary and Outlook

Selenium has come a long way since its inception in 2004. Nowadays, many practitioners consider it the de-facto standard solution to develop end-to-end tests for web applications, and it is used by thousands of projects worldwide. In this chapter, you have seen the foundations of the Selenium project (made up of WebDriver, Grid, and IDE). In addition, you can notice that Selenium has a rich ecosystem and active community. WebDriver is the heart of the Selenium project, and it is a library that provides an API to control different web browsers (e.g., Chrome, Firefox, Edge, etc.) programmatically. Table 1-13 contains a comprehensive overview of the primary and secondary usages of Selenium WebDriver.

Table-13. Selenium WebDriver primary and secondary usages

|  | Primary | Secondary (other usages) |
|---|---|---|
| **Purpose** | Automated testing | Web scraping, web-based administration tasks |
| **Test level** | End-to-end testing | System testing (mocking backend calls)<br>Acceptance testing (e.g., using with Cucumber) |
| **Test type** | Functional testing (ensuring expected behavior)<br><br>Cross-browser testing (compatibility in different web browsers)<br><br>Regression testing (ensuring build after each commit in CI) | Non-functional testing (e.g., load, security, accessibility, or localization) |
| **Test methodology** | TLD (implementing tests when SUT is available) | BDD (defining user scenarios at early development stages) |

In the next chapter, you discover how to set up a Java project using Maven or Gradle as build tools. This project will contain end-to-end tests for web applications using JUnit and TestNG as the unit testing frameworks and calls to the WebDriver API. In addition, you will learn how to control different web browsers (e.g., Chrome, Firefox, or Edge) with a basic test case (the Selenium WebDriver’s version of the classic *hello world*).

# Chapter 2. Preparing for Testing

# A NOTE FOR EARLY RELEASE READERS

> With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.
> 
> This will be the 2nd chapter of the final book. Please note that the GitHub repo will be made active later on.
> 
> If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the development editor at rfernando@oreilly.com.

This chapter aims to implement your first end-to-end test using Selenium WebDriver and the Java language. For that, we first review the technical requirements in terms of previous knowledge, hardware, and software. Second, this chapter provides an overview for setting up a Java project that includes Selenium WebDriver tests. To ease the project setup, you can use a build tool such as Maven or Gradle. Finally, you will learn to implement a basic end-to-end test with Selenium WebDriver, i.e., a *hello world* test. We will implement this test in several flavors, using different web browsers (such as Chrome, Edge, or Firefox) and unit testing frameworks (JUnit and TestNG). Remember that every code example I present in this book is available in an open-source GitHub repository. Thus, you can reuse the content and configuration of this repository as the foundation for your own tests.

## Requirements

The first requirement to start using Selenium WebDriver with Java is comprehending the Java language and object-oriented programming. It is not necessary to be an expert, but basic knowledge about it is required. Then, you
