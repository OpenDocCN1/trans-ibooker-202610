# # Chapter 9. Third-Party Integrations

> **A NOTE FOR EARLY RELEASE READERS**
> 
> With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.
> 
> This will be the 9th chapter of the final book. Please note that the GitHub repo will be made active later on.
> 
> If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at rfernando@oreilly.com.

This chapter introduces different third-party technologies (such as libraries or frameworks) that we can use with Selenium WebDriver. We need to use some of these technologies when the Selenium WebDriver API is insufficient to carry out specific tasks. This is the case of file downloading, in which we need to use a third-party utility to wait until the files are correctly downloaded or, alternatively, use an HTTP client to control the download. We also capture the HTTP traffic using a third-party proxy. Another scenario in which we need to use external utilities to Selenium WebDriver is when implementing non-functional tests, such as performance, security, accessibility, or A/B testing. We can also use third-party libraries to develop Selenium WebDriver tests using a fluent API, generate fake test data, or improve test reporting. Finally, we can integrate relevant frameworks such as Cucumber for Behavior Driven Development (BDD) or the Spring Framework (for developing web applications). We will go through all of the uses I mentioned in this chapter.