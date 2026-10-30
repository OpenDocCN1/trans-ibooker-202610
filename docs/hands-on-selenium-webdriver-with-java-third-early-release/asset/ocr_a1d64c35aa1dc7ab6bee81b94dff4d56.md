```
Locale.forLanguageTag(lang)); ③
String home = strings.getString("home");
String content = strings.getString("content");
String about = strings.getString("about");
String contact = strings.getString("contact");

String bodyText =
driver.findElement(By.tagName("body")).getText();

assertThat(bodyText).contains(home).contains(content).contains(about)
    .contains(contact); ④
}
```

+   ① We specify Spanish of Spain as the preferred language in Chrome.

+   ② We open a practice page that supports multilanguage (English and Spanish).

+   ③ We read the text translations using a resource bundle. You can find these strings in the file strings_es.properties (and strings_en.properties) in the project folder src/test/resources.

+   ④ We assert that the document body contains all the expected strings.

# Example 5-21. Test setup that specifies a preferred locale for Firefox

```
@BeforeEach
void setup() {
    lang = "es-ES";
    FirefoxOptions options = new FirefoxOptions();
    options.addPreference("intl.accept_languages", lang);

    driver = WebDriverManager.firefoxdriver().capabilities(options)
        .create();
}
```

There is a second alternative to practice localization testing with Selenium WebDriver. Instead of changing the preferred language (which determines the HTTP header accept-language), we can change the default language of the web browser. Multilanguage applications, if that HTTP