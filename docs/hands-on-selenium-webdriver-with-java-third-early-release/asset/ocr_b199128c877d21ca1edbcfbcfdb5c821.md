| Attribute | Capability name | Description |
|---|---|---|
| `BROWSER_NAME` | `browserName` | Browser name (e.g., `chrome`, `firefox`, or `msedge`) |
| `PLATFORM_NAME` | `platformName` | Platform name (e.g., `WINDOWS`, `LINUX`, `MAC`, `ANDROID`, `IOS`, among others) |
| `BROWSER_VERSION` | `browserVersion` | Browser version |
| `SUPPORTS_JAVASCRIPT` | `javascriptEnabled` | Enable or disable JavaScript execution |
| `PAGE_LOAD_STRATEGY` | `pageLoadStrategy` | Page loading strategy (`normal`, `eager`, or `none`) |

An alternative way to specify the required capabilities in a `RemoteWebDriver` object is using an instance of `DesiredCapabilities`. Table 6-2 summarizes the methods provided by these objects.