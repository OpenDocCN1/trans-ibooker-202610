+   3. We confirm the selected is checked.
+   4. We use the pseudo-class *checked* and the operator *not* to locate default checkboxes.
+   5. We check the element id is as expected.
+   6. We confirm the selected is unchecked.

# Locating by XPath

XPath (XML Path Language) is a powerful way of navigating through the DOM of XML-like documents, such as HTML pages. It includes over 200 built-in functions that allow creating advanced queries to select nodes. There are two types of XPath queries. First, *absolute* queries use the symbol slash (`/`) to traverse the DOM from the root node. For example, considering the basic HTML page in **Example 3-5**, to select the link element present in this page using this approach, we need the following XPath query:

```
```text
/html/body/a
```
```

Absolute XPath queries are easy to create, but they have a relevant inconvenience: any minimal change in the page layout would make a locator built with this strategy fail. For this reason, as a general rule, the usage of absolute XPaths is discouraged. Instead, *relative* queries are more convenient. The general syntax for relative XPath queries is as follows:

```
```text
//tagname[@attribute='value']
```
```

**Example 3-11** shows a test with an XPath locator to select the hidden field in the practice web.

```
### Example 3-11. Test using a basic locator with XPath

```java
@Test
void testByXPathBasic() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");
```
```