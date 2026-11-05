The following table shows when the key versions of modern .NET were released, when future releases are planned, and which version is used by the various editions of this book:

| Version | Released | Edition | Published |
|---|---|---|---|
| .NET Core RC1 | November 2015 | First | March 2016 |
| .NET Core 1.0 | June 2016 |  |  |
| .NET Core 1.1 | November 2016 |  |  |
| .NET Core 1.0.4 and .NET Core 1.1.1 | March 2017 | Second | March 2017 |
| .NET Core 2.0 | August 2017 |  |  |
| .NET Core for UWP in Windows 10 Fall Creators Update | October 2017 | Third | November 2017 |
| .NET Core 2.1 (LTS) | May 2018 |  |  |
| .NET Core 2.2 (Current) | December 2018 |  |  |
| .NET Core 3.0 (Current) | September 2019 | Fourth | October 2019 |
| .NET Core 3.1 (LTS) | December 2019 |  |  |
| Blazor WebAssembly 3.2 (Current) | May 2020 |  |  |
| .NET 5.0 (Current) | November 2020 | Fifth | November 2020 |
| .NET 6.0 (LTS) | November 2021 | Sixth | November 2021 |
| .NET 7.0 (Current) | November 2022 | Seventh | November 2022 |
| .NET 8.0 (LTS) | November 2023 | Eighth | November 2023 |

.NET Core 3.1 included Blazor Server for building web components. Microsoft had also planned to include Blazor WebAssembly in that release, but it was delayed. Blazor WebAssembly was later released as an optional add-on for .NET Core 3.1. I include it in the table above because it was versioned as 3.2 to exclude it from the LTS of .NET Core 3.1.

# Understanding .NET support

.NET versions are either Long Term Support (LTS) or Current, as described in the following list:

+   - **LTS** releases are stable and require fewer updates over their lifetime. These are a good choice for applications that you do not intend to update frequently. LTS releases will be supported for 3 years after general availability, or 1 year after the next LTS release ships, whichever is longer.
+   - **Current** releases include features that may change based on feedback. These are a good choice for applications that you are actively developing because they provide access to the latest improvements. After a 6-month maintenance period, or 18 months after general availability, the previous minor version will no longer be supported.