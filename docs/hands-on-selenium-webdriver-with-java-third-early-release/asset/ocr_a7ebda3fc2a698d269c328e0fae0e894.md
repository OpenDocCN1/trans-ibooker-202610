```
public class CucumberTest {
}
```

① We need to use the Cucumber runner to execute the step definitions as test cases.

② We specify the location of the Gherkin scenarios. In this case, the feature is the folder io/github/bonigarcia in the project classpath (concretely, in the src/test/resources folder). This annotation also specified the initial package to search the glue code (i.e., the steps definition).

# Example 9-16. Cucumber test using JUnit 5

```
@Suite ①
@IncludeEngines("cucumber") ②
@SelectClasspathResource("io/github/bonigarcia") ③
@ConfigurationParameter(key = GLUE_PROPERTY_NAME, value = "io.github.bonigarcia") ④
public class CucumberTest {
}
```

① We need to use the JUnit 5 suite module to run Cucumber tests.

② We include the Cucumber engine into the JUnit Platform.

③ We specify the path for features within the project classpath.

④ We set the initial package to search the glue code.

# Example 9-17. Cucumber test using TestNG

```
@CucumberOptions(features = "classpath:io/github/bonigarcia", glue = {
    "io.github.bonigarcia" }) ①
public class CucumberTest extends AbstractTestNGCucumberTests { ②
}
```