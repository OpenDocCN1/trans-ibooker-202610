| getTitle() | String | Get the title (`<title>` HTML tag) of the current web page |
| findElement(By by) | WebElement | Find the first WebElement using a given locator in the current web page. In other words, if there are several elements that match the locator, the first one (in the DOM) is returned (see “Locating WebElements” for further details) |
| findElements(By by) | List<WebElement> | Find all the WebElement using a given locator in the current web page (see also “Locating WebElements”) |
| getPageSource() | String | Get the HTML source code of the current web page |
| navigate() | Navigation | Access the browser history and navigate to a given URL (see Chapter 4) |
| getWindowHandle() | String | Get the window handle, i.e., a unique identifier for the open window in the current browser (see Chapter 4 for further details) |
| getWindowHandles() | Set<String> | Get the set of window handles currently opened in the current browser (see also Chapter 4) |
| switchTo() | TargetLocator | Select a frame or window in the current browser (see Chapter 4) |
| manage() | Options | Generic utility for managing different aspects of the browser (e.g., browser size and position, cookies, timeouts, logs, and the Input Method Engine (IME) API) |
| close() | void | Close the current window, quitting the browser if there is no more windows opened |
| quit() | void | Close all windows and quit the browser |

TIP

From now on, I illustrate the examples showing only the test logic. These tests use a WebDriver object created before the test (in the setup method) and closed after the test (in the teardown method). As a convention, I show the JUnit 5 tests in the book (although you can find them also for JUnit 4, Selenium-Jupiter, and TestNG in the examples repository).

# Example 3-3. Test using several basic methods of the WebDriver API