| Method | Return type | Description |
|---|---|---|
| `forward()` | `void` | |
| `to(String url)` | `void` | Load a new web page in the current window |
| `to(URL url)` |  | |
| `refresh()` | `void` | Refresh the current page |

# Example 4-13. Test using navigation methods

```
```java
@Test
void testHistory() {
    String baseUrl = "https://bonigarcia.dev/selenium-webdriver-java/";
    String firstPage = baseUrl + "navigation1.html";
    String secondPage = baseUrl + "navigation2.html";
    String thirdPage = baseUrl + "navigation3.html";

    driver.get(firstPage);

    driver.navigate().to(secondPage);
    driver.navigate().to(thirdPage);
    driver.navigate().back();
    driver.navigate().forward();
    driver.navigate().refresh();

    assertThat(driver.getCurrentUrl()).isEqualTo(thirdPage);
}
```
```

# Cookies

HTTP 1.x is a stateless protocol, meaning that the server does not track the user state. In other words, web servers do not remember users across different requests. The cookies mechanism is an extension to HTTP that allows tracking users by sending small pieces of text called *cookies* from server to client. These cookies must be sent back by clients, and this way, servers *remember* their clients. Cookies allow you to maintain web sessions or personalize the user experience on the website, among other functions.