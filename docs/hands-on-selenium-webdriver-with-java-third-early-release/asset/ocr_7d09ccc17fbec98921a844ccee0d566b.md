# Chapter 1. A Primer on Selenium

# A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 1st chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the development editor at rfernando@oreilly.com.

*Selenium* is an open-source suite composed of a set of libraries and tools that enables the automation of web browsers. We can see Selenium as an umbrella project composed of three core components: WebDriver, Grid, and IDE (Integrated Development Environment). Selenium WebDriver is a library that allows driving browsers programmatically. Thus, we can use Selenium WebDriver to navigate websites and interact with web pages (e.g., clicking on links, filling in forms, etc.) as a real user would do in an automated fashion. The primary use of Selenium WebDriver is the automated testing of web applications. Other usages of Selenium include the automation of web-based administration tasks or web scraping (automated web data extraction).

This chapter provides a comprehensive overview of the Selenium core components: WebDriver, Grid, and IDE. Then, it reviews the Selenium ecosystem, i.e., other tools and technologies around it. Finally, it analyzes the foundations of software testing related to Selenium.

# Selenium Core Components

Jason Huggins and Paul Hammant created Selenium in 2004 while working in ThoughtWorks. They chose the name “Selenium” as a counterpart to Mercury, an existing testing framework developed by HP. The name is significant because the chemical selenium is known for reducing the toxicity of mercury.

That initial version of Selenium (known nowadays as *Selenium Core*) is a JavaScript library that impersonates user actions in web applications. Selenium Core interprets the so-called *Selenese* commands to achieve this task. These commands are encoded as an HTML table composed of three parts: *command* (action executed in a web browser, such as opening an URL