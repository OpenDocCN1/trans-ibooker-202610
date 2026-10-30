+   ① We specify the location of the Gherkin scenarios and the package to search the glue code.

+   ② We need to extend a TestNG parent test class for Cucumber tests.

# Web Frameworks

Web frameworks are software frameworks designed to support the development of web applications and services. One of the most popular frameworks for the Java language is the Spring framework. Spring is an open-source framework for building Java applications, including enterprise web applications and services. The core technology of Spring is known as Inversion of Control (IoC), which is a procedure to create instances outside the class in which these objects are used. These objects, called beans or components in the Spring jargon, are later injected on-demand as dependencies by the Spring IoC container.

The following examples show basic tests that verify a local web application created with Spring-Boot, a subproject of the Spring portfolio aimed to simplify the development of Spring-based applications thanks to convention over configuration and autodiscovery features. In addition, Spring-Boot provides an embedded webserver to ease the development of web applications. The necessary code to integrate Spring-Boot with the unit testing frameworks used in this book is different. This way, Example 9-18 shows a test integrating Spring-Boot and JUnit 4. Then, Example 9-19 show the differences when using TestNG, Example 9-20 illustrates how to use JUnit 5, and finally, Example 9-21 shows a Spring test based on JUnit 5 with Selenium-Jupiter.

Example 9-18. Test using Spring-Boot and JUnit 4

```
@RunWith(SpringRunner.class) ①
@SpringBootTest(classes = SpringBootDemoApp.class,
    webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
②
public class SpringBootJUnit4Test {
    private WebDriver driver;
```