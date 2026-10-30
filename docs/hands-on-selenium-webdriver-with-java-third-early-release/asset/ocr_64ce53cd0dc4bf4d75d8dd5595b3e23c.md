```
@LocalServerPort ③
protected int serverPort;

@Before
public void setup() {
    driver = WebDriverManager.chromedriver().create();
}

@After
public void teardown() {
    driver.quit();
}

@Test
public void testSpringBoot() {
    driver.get("http://localhost:" + serverPort);
    String bodyText =
    driver.findElement(By.tagName("body")).getText();
    assertThat(bodyText)
        .contains("This is a local site served by Spring-Boot");
}
}
```

① We need to use the Spring runner in JUnit 4.

② We use a Spring-Boot annotation for tests to define the Spring-Boot class name. Also, we specify the web application is deployed using a random available port.

③ We inject the web application port as a class attribute.

# Example 9-19. Test using Spring-Boot and TestNG

```
@SpringBootTest(classes = SpringBootDemoApp.class,
    webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
①
public class SpringBootNGTest extends
AbstractTestNGSpringContextTests { ②

    // Same logic as the previous test
}
```