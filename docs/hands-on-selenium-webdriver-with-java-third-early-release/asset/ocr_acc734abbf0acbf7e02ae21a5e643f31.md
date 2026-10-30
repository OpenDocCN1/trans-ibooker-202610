test logic.

# Selenium-Jupiter

Selenium-Jupiter provides a complementary feature for creating cross-browser tests, called *test templates*. Test templates are a special kind of parameterized test supported by Jupiter in which some extension collects the parameters. Selenium-Jupiter uses this feature to provide a comprehensive way to specify different browser aspects (such as type, version, arguments, and capabilities) using a custom JSON notation called *browser scenario* in the Selenium-Jupiter jargon. You can find more details about this feature in the Selenium-Jupiter documentation.

Example 8-8 shows a sample browser scenario. This JSON is stored in a file called `browsers.json`, the default name used by a template test. Example 8-9 a template test using this browser scenario.

# *Example 8-8. Browser scenario for a test template in Selenium-Jupiter*

```
```json
{
  "browsers": [
    [
      {
        "type": "chrome"
      }
    ],
    [
      {
        "type": "edge",
        "arguments": [
          "--headless"
        ]
      }
    ],
    [
      {
        "type": "firefox-in-docker",
        "version": "93"
      }
    ]
  ]
}
```
```