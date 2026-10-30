## Preface

Selenium is an open-source umbrella project that enables the automation of web browsers. The core component of the Selenium project is Selenium WebDriver, a library for controlling browsers (e.g., Chrome, Firefox, Edge, Safari, or Opera) programmatically. Selenium WebDriver provides a cross-browser Application Programming Interface (API) in several programming languages (officially supported in Java, JavaScript, Python, C#, or Ruby).

Although we can use Selenium WebDriver for multiple purposes related to browser automation, its primary usage is implementing end-to-end tests for web applications verification. Nowadays, thousands of organizations and testers use Selenium worldwide, and it is one of the leading solutions for end-to-end testing, supporting a multi-million dollar industry.

### Who Should Read This Book

This book provides a comprehensive summary of the main features of Selenium WebDriver version 4 using Java as language binding. It reviews the main aspects of automated web navigation, browser manipulation, web element interaction, user impersonation, automated driver management, the “Page Object Model” (POM) design pattern, use of remote and cloud infrastructure, integration with Docker and third-party tools, and much more.

The primary audience of this book includes Java coders of different levels (from beginners to advanced), such as developers, testers, QA engineers, etc. Thus, you need a basic knowledge of the Java language and object-oriented programming. The final goal is to have a comprehensive understanding of the main aspects of Selenium WebDriver to create end-to-end tests in Java using different testing frameworks of your choice (e.g., JUnit or TestNG).

### Why I Wrote This Book

Test automation is a software testing technique that leverages automation tools to control test execution. It allows increasing the efficiency and effectiveness while ensuring the overall quality of a software system. In this arena, Selenium WebDriver is the de-facto standard library to develop end-to-end tests for web applications. This book provides the first complete review of Selenium 4 to date.

This book follows a learn-by-doing approach. To that aim, we review the main features of Selenium WebDriver through ready-to-be-executed test examples. These examples are publicly available in a GitHub open-source repository (https://github.com/bonigarcia/selenium-webdriver-java). For the sake of completeness, this repository contains each test example in different flavors regarding the embedding testing framework: JUnit 4, JUnit 5 (alone or with Selenium-Jupiter), and TestNG.

### Navigating This Book

The content of this book is divided into three parts and ten chapters, namely:

## Part I, Introduction

Part I provides a technological background on Selenium, test automation, and project setup. This part, more theoretical than practical, is composed of two chapters:

- **Chapter 1**, *A Primer on Selenium*, presents the core components of the Selenium project (WebDriver, Grid, and IDE) and its ecosystem (i.e., the tools and technologies around Selenium). In addition, this chapter reviews the principles of end-to-end testing related to Selenium.
- **Chapter 2**, *Preparing for Testing*, explains how to set up a Java project (Maven and Gradle) containing end-to-end tests that use the Selenium WebDriver API. Then, you will learn how to develop your first WebDriver tests using different testing frameworks: JUnit 4, JUnit 5 (alone or in conjunction with Selenium-Jupiter), and TestNG.

## Part III, Advanced Concepts

Part III focuses on leveraging the Selenium WebDriver API in different ambits and use cases. This part includes the following chapters:

- **Chapter 7**, *The Page Object Model (POM)*, introduces POM, a popular design pattern used in conjunction with Selenium WebDriver. This pattern allows to model web pages using object-oriented classes to ease test maintenance and reduce code duplication.
- **Chapter 8**, *Testing Framework Specifics*, reviews several particular features to the unit testing framework used together with Selenium WebDriver that allows improving different aspects of the overall testing process. To that aim, this chapter first explains how to carry out cross-browser testing (i.e., reusing the same test logic for verifying web applications using different browsers) using parameterized tests and test templates. Then, you will learn how to split tests into different categories for execution filtering, ordering tests, failure analysis (i.e., collecting and analyzing data to determine the cause of a failure), retrying tests, parallel test execution, test listeners, or disabling tests.
- **Chapter 9**, *Third-Party Integrations*, reviews different technologies you can use to enhance your Selenium WebDriver tests, such as reporting tools, test data generation, and other frameworks (e.g., Cucumber or Spring). Moreover, this chapter describes how to use external libraries with Selenium to implement specific use cases, such as file downloading or non-functional tests (such as load, security, or accessibility).
- **Chapter 10**, *Beyond Selenium*, presents a couple of automation frameworks related to Selenium, namely Appium (for mobile testing) and REST-assured (for testing REST web services). To conclude, we review some of the most relevant alternatives to





WARNING
此元素表示警告或注意。

## 使用代码示例

代码示例可在 https://github.com/bonigarcia/selenium-webdriver-java 上下载。如果您对使用代码示例有技术问题或遇到问题，请发送电子邮件至 bookquestions@oreilly.com。

本书旨在帮助您完成工作。一般而言，如果本书提供了示例代码，您可以在自己的程序和文档中使用它。除非您复制了代码的很大一部分，否则无需联系我们征求许可。例如，使用本书中的几段代码编写程序不需要许可。出售或分发 O’Reilly 书籍中的示例需要许可。引用本书并引用示例代码来回答问题不需要许可。将大量示例代码纳入您产品的文档中需要许可。

我们感谢但通常不需要署名。署名通常包括标题、作者、出版商和 ISBN。例如：“Book Title by Some Author (O’Reilly). Copyright 2012 Some Copyright Holder, 978-0-596-xxxx-x。”

如果您认为使用代码示例超出了合理使用范围或上述许可范围，请随时联系我们 permissions@oreilly.com。

## O’Reilly Online Learning

> **NOTE**
> 
> 40 多年来，*O’Reilly Media* 一直为公司提供技术和商业培训、知识和见解，帮助公司取得成功。

我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享他们的知识和专业知识。O’Reilly 的在线学习平台为您提供按需访问实时培训课程、深入学习路径、交互式编码环境，以及来自 O’Reilly 和 200 多家其他出版商的庞大文本和视频集合。有关更多信息，请访问 *http://oreilly.com*。

## 如何联系我们

请将有关本书的评论和问题发送给出版商：

O’Reilly Media, Inc.  
1005 Gravenstein Highway North  
Sebastopol, CA 95472  
800-998-9938（美国或加拿大境内）  
707-829-0515（国际或本地）  
707-829-0104（传真）

我们为本书设有网页，在该网页上列出勘误表、示例和任何其他信息。您可以通过以下地址访问此页面

*https://learning.oreilly.com/library/view/hands-on-selenium-webdriver/9781098109998*。

发送电子邮件至 *bookquestions@oreilly.com* 对本书发表评论或提出技术问题。

有关我们书籍和课程的新闻和信息，请访问 *http://oreilly.com*。

在 Facebook 上找到我们：*http://facebook.com/oreilly*。

在 Twitter 上关注我们：*http://twitter.com/oreillymedia*。

在 YouTube 上观看我们：*http://www.youtube.com/oreillymedia*。

## 致谢
