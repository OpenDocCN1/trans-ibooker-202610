

![](img/b67ec8e49249e340add6ed66ab413d78_472_0.png)

Figure 9-6. Practice page using fake data

## Reporting

A test report is a document that summarizes the results after executing a test suite. This document typically contains the number of tests executed plus their verdicts (pass, fail, skip) and execution time. There are different ways to obtain a test report in our Java project. For example, when using Maven, we can create a basic test report using the following commands:

```
mvn test ①
mvn surefire-report:report-only ②
mvn site -DgenerateReports=false ③
```

1. We execute the test with Maven. As a result, the Maven Surefire plugin generated a set of XML files in the target folder. These files contain the results of the test execution.
2. We convert the XML reports into an HTML report. You can find this HTML report in your project folder `target/site`.
3. We force to copy the CSS and images required in the HTML report. Figure 9-7 shows a screenshot example of this report.

![](img/b67ec8e49249e340add6ed66ab413d78_473_0.png)

*Figure 9-7. Test report generated with Maven*

We can generate an equivalent report also when using Gradle. After executing a test suite with this build tool, Gradle automatically generates an HTML report in the folder `build/reports`. Figure 9-8 shows an example of the test report generated for executing a group of tests (using the shell command `gradle test --tests Hello*`).

![](img/b67ec8e49249e340add6ed66ab413d78_474_0.png)

Figure 9-8. Test report generated with Gradle

In addition to Maven and Gradle, we can use existing reporting libraries to create richer reports. A possible alternative is Extent Reports, a library to create interactive test reports. Extent Reports provides professional (commercial) and community (open-source) editions. Example 9-12 shows a test using the latter.

### Example 9-12. Test using Extent Reports to generate an HTML report

```
class ReportingJupiterTest {

    WebDriver driver;

    static ExtentReports reports;

    @BeforeAll
    static void setupClass() {
        reports = new ExtentReports(); ①
        ExtentSparkReporter htmlReporter = new ExtentSparkReporter(
                "extentReport.html");
        reports.attachReporter(htmlReporter); ②
    }

    @BeforeEach
    void setup(TestInfo testInfo) {
        reports.createTest(testInfo.getDisplayName()); ③
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterEach
    void teardown() {
        driver.quit();
    }

    @AfterAll
    static void teardownClass() {
        reports.flush();
    }

    // Test methods ④
}
```

1. We create an instance of the test reporter.
2. We configure it to generate an HTML report.
3. After each test, we create an entry in the test report using the test name as an identifier. In JUnit 5, we use TestInfo, a built-in parameter resolver that allows retrieving information about the current test.
4. As usual, you can find the complete source code in the examples repository. In particular, this class has two test methods. Figure 9-9 shows the resulting test report generated when this test class is executed.

![](img/b67ec8e49249e340add6ed66ab413d78_475_0.png)

An inconvenience of Extent Report is that we need to add each test explicitly to the reporter. A possible solution to this problem is using custom test listeners (as explained in “Test Listeners” in Chapter 8) to group the common logic for reporting.

Another possible library for generating rich test reports is Allure, an open-source reporting framework for generating test reports for different programming languages, including Java, Python, and JavaScript, among others. A notable difference between Allure and Extent Report is that Allure uses a test listener configured in the build tool Maven or Gradle (see Appendix C for further details about this configuration). This way, we did not require to change our test suite for generating Allure reports. Table 9-1 summarized the necessary commands to create Allure reports with Maven and Gradle.

### Table 9-1. Maven and Gradle commands to generate Allure reports

| Maven | Gradle |
|-------|--------|
| `mvn allure:serve` | `allureServe` |

Open the HTML report using a local webserver (as shown in Figure 9-10)

![](img/b67ec8e49249e340add6ed66ab413d78_479_0.png)

Figure 9-10. Test report generated with Allure and served locally

## Behavior Driven Development (BDD)

As introduced in Chapter 1, Behavior-Driven Development (BDD) is a software methodology that promotes the development and testing of software systems using high-level user scenarios. Different tools implement the BDD methodology. One of the most popular is Cucumber. Cucumber executes tests based on user stories written in Gherkin, a human-readable notation based on natural languages (e.g., English and others). Gherkin was designed to be used by non-programmers roles (e.g., end-users), and its main keywords are the following (see the Gherkin user manual for further information):

- *Feature*: High-level description of the software feature tested.
- *Scenario*: Concrete test that illustrates a business rule. Scenarios describe different pieces of information (called *steps* in the Gherkin jargon), such as:
  - *Given*: Preconditions and initial state.
  - *When*: User actions.
  - *And*: Additional user actions.
  - *Then*: Expected outcome.

Example 9-13 shows a Gherkin feature containing two scenarios for a test login (successful and failed).

### Example 9-13. Gherkin scenarios for login into the practice site

```
Feature: Login in practice site ①

    Scenario: Successful login ②
        Given I use "Chrome" ③
        When I navigate to
            "https://bonigarcia.dev/selenium-webdriver-java/login-form.html" ④
        And I log in with the username "user" and password "user" ⑤
        And I click Submit
        Then I should see the message "Login successful" ⑥

    Scenario: Failure login ⑦
        When I navigate to
            "https://bonigarcia.dev/selenium-webdriver-java/login-form.html"
        And I log in with the username "bad-user" and password "bad-password"
        And I click Submit
        Then I should see the message "Invalid credentials"
```

1. Feature description.
2. First scenario (login successful).
3. Browser to be used.
4. Webpage URL.
5. Set of actions (type credentials and click submit button).
6. Expected message.
7. Second scenario (login failed).

To run Gherkin scenarios as test cases, we must first create the corresponding *step definitions*. A step definition is a *glue code* that exercises the SUT using the information specified in the scenario. In Java, we use annotations (such as `@Given`, `@Then`, `@When`, or `@And`) to decorate methods implementing each step. These annotations contain a string value to map each step definition and the parameters. **Example 9-14** shows a step definition for the Gherkin scenario defined on **Example 9-13**. We use the Selenium WebDriver API to implement the required actions for navigation, web element interaction, etc.

```
public class LoginSteps {

    private WebDriver driver;

    @Given("I use {string}")
    public void iUse(String browser) {
        driver = WebDriverManager.getInstance(browser).create();
    }

    @When("I navigate to {string}")
    public void iNavigateTo(String url) {
        driver.get(url);
    }

    @And("I log in with the username {string} and password {string}")
    public void iLogin(String username, String password) {
        driver.findElement(By.id("username")).sendKeys(username);
        driver.findElement(By.id("password")).sendKeys(password);
    }

    @And("I click Submit")
    public void iPressEnter() {
        driver.findElement(By.cssSelector("button")).click();
    }

    @Then("I should see the message {string}") ⑤
    public void iShouldSee(String result) {
        try {
            String bodyText =
                    driver.findElement(By.tagName("body")).getText();
            assertThat(bodyText).contains(result);
        } finally {
            driver.quit();
        }
    }
}
```

1. We use the first step to create a WebDriver instance.
2. We open the URL.
3. We type the credentials.
4. We click on the submit button.
5. We verify that the expected message is on the page.

Finally, we need to run our step definition as a test case. This final action is unit testing framework-dependent. In other words, the required development is different in JUnit 4 (see Example 9-15), JUnit 5 (see Example 9-16), and TestNG (see Example 9-17).

> NOTE
> Selenium-Jupiter does not provide any additional feature for integrating with Cucumber, so the default JUnit 5 procedure is the same in the Selenium-Jupiter project in the examples repository.

### Example 9-15. Cucumber test using JUnit 4

```
@RunWith(Cucumber.class) ①
@CucumberOptions(features = "classpath:io/github/bonigarcia", glue = {
    "io.github.bonigarcia" }) ②
```
