session-timeout, which is the timeout in which nodes are terminated when there is no activity (300 seconds by default).

+   - Using TOML files. TOML (Tom’s Obvious Minimal Language) is a configuration format designed to be human-readable. Like the CLI options, these files allow configuring the Selenium Grid parameters but using a TOML notation.

# Cloud Providers

As introduced in Chapter 1, a cloud provider in the Selenium ecosystem is a company that provides managed services (usually commercial) for automated web and mobile testing. Common services offered by cloud providers include:

+   - Browsers as a service: to request on-demand web browsers hosted by the providers. These browsers are usually of different types, versions, and operating systems. This feature is typically used for cross-browser automated or live testing.

+   - Analysis capabilities: to monitor and debug automated tests. To that aim, cloud providers typically support session recordings or rich error reporting features.

+   - Mobile testing: to request emulated (and real) mobiles devices on different platforms, such as Android and iOS.

+   - Visual testing: automatically inspect the UI and ensure that end-users have a correct visual experience.

Examples of cloud providers for Selenium nowadays are Sauce Labs, BrowserStack, LambdaTest, CrossBrowserTesting, Moon Cloud, TestingBot, Perfecto, or Testinium. All these companies offer specific services with different pricing plans. The common aspect between them is that each cloud provider maintains a Selenium Server endpoint that we can use in RemoteWebDriver tests. Example 6-9 illustrates how to use one