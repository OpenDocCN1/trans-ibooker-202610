# Chapter 5. Browser-Specific Manipulation

# A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 5th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the development editor at rfernando@oreilly.com.

As you have seen so far, many features of the WebDriver API are compatible across browsers, i.e., we can use these features to drive different types of browsers programmatically. Other parts of the WebDriver API are not interoperable among browsers. In other words, there are some WebDriver characteristics available for some browsers (e.g., Chrome or Edge) but unavailable (or different) for others (e.g., Firefox). This chapter reviews these browser-specific features.

# Browser Capabilities

Selenium WebDriver allows specifying browser-specific aspects by using *capabilities*. Examples of capabilities are headless mode, page loading strategies, usage of web extensions, or push notifications management,