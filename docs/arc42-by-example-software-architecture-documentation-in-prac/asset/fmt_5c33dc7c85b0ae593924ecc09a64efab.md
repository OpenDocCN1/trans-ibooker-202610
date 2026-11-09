
```
// and images are copied to build directory

    dependsOn asciidoctor

    sourceDir = new File("${buildDir}/html5")

// files to check, in Set-notation

sourceDocuments = ["many-errors.html", "no-errors.html"]

// fail the build if any error is encountered

failOnErrors = false

// set the http connection timeout to 2 secs

httpConnectionTimeout = 2000

ignoreLocalHost = false

ignoreIPAddresses = false

    }

    defaultTasks 'htmlSanityCheck'
```
## II.8 Crosscutting Concepts
One should explain crosscutting and the ubiquitous rules of the system. `arc42` calls them concepts: they often affect multiple building blocks and are relevant in several parts of the system and its implementation. Examples include rules for the usage of technologies and/or frameworks and implementation rules, and the design or architecture patterns that are used.

### 8.1 Domain Model
The following diagram shows a domain model:

![Figure 2.20: Domain checking model ](image/B15546_02_20.jpg)

###### Figure 2.20: Domain checking model

The properties of the implementation classes are private since we manipulate them via getter/setter methods.

The following table describes every component of the domain model:

![Figure 2.21: Description of the terms in the HTML domain checking model ](image/B15546_02_21.jpg)

###### Figure 2.21: Description of the terms in the HTML domain checking model

### 8.2 Structure of HTML Links
For many web developers or HTML experts, the following information on URI syntax might be completely evident. We wrote this book for different kinds of people, so we included this information just in case.

`HtmlSC` performs various checks on HTML links (hyperlinks), which usually follow the URI syntax specified by RFC-2396 ([https://www.ietf.org/rfc/rfc2396.txt](https://www.ietf.org/rfc/rfc2396.txt)). URIs are generally used to provide links to arbitrary resources (documents, files, or parts within documents).

Their general structure is depicted in the following diagram. A unit test follows:

![Figure 2.22: Generic URI structure ](image/B15546_02_22.jpg)

###### Figure 2.22: Generic URI structure

Test Showing Generic URI Syntax

The following code snippet is some test code for generic URI syntax:
```
@Test

public void testGenericURISyntax() {

// based upon an example from the Oracle(tm) Java tutorial:

// https://docs.oracle.com/javase/tutorial/networking/urls/urlInfo.\\

html

def  aURL  =  new  URL(

"https://example.com:42/docs/tutorial/index.html?name=aim42#INT\\

     RO");

    aURL.with  {

assert getProtocol() == "http"

assert getAuthority() == "example.com:42"

assert getHost() == "example.com"

assert getPort() == 42

assert getPath() == "/docs/tutorial/index.html"

assert getQuery() == "name=aim42"

assert getRef() == "INTRO"

    }

    }
```

### 8.3 Multiple Checking Algorithms
`HtmlSC` uses the template method pattern ([https://sourcemaking.com/design_patterns/template_method/](https://sourcemaking.com/design_patterns/template_method/)) to enable flexible checking algorithms:

"The Template Method defines *a skeleton of an algorithm* in an operation and defers some steps to subclasses."

We achieve this by defining the skeleton of the checking algorithm in one operation (`performCheck`), thereby deferring the specific checking algorithm steps to subclasses. The invariant steps are implemented in the abstract base class, while the variant checking algorithms have to be provided by the subclasses.

Template Method for Performing a Single Type of Check
```
/**
* Prerequisite: pageToCheck has been successfully parsed,
* prior to constructing this Checker instance.
*/

public CheckingResultsCollector performCheck() {

// assert prerequisite

assert  pageToCheck  !=  null

initResults()

return check() // subclass executes the actual checking algorithm

    }
```
The following diagram depicts the template method:

![Figure 2.23: Template method (excerpt) ](image/B15546_02_23.jpg)

###### Figure 2.23: Template method (excerpt)

The following table describes the components of the template:

![Figure 2.24: Description of template components ](image/B15546_02_24.jpg)

###### Figure 2.24: Description of template components

### 8.4 Reporting
`HtmlSC` supports the following output (`== reporting`) formats and destinations:
* Formats include HTML and text.
* Destinations include file and console.

The reporting subsystem uses the template method pattern to allow different output formats (for example, the console and HTML). The overall structure of the reports is always the same.

Report Findings Using the Template Method Pattern

The (generic and abstract) reporting is implemented in the abstract `Reporter` class, as follows:
```
/**
* main entry point for reporting - to be called when a report is requested
* Uses template-method to delegate concrete implementations to subclasses
*/

public void reportFindings() {

initReport() // (1)

reportOverallSummary() // (2)

reportAllPages() // (3)

closeReport() // (4)

}

private void reportAllPages() {

pageResults.each { pageResult ->

reportPageSummary( pageResult ) // (5)

pageResult.singleCheckResults.each { resultForOneCheck ->

reportSingleCheckSummary( resultForOneCheck ) // (6)

reportSingleCheckDetails( resultForOneCheck ) // (7)

reportPageFooter()

}

}
```
The preceding code can be summarized as follows:
1. Initialize the report, that is, create and open the file, and copy the `CSS`, `JavaScript`, and image files.
2. Create the overall summary, along with the overall success percentage and a list of all checked pages with their success rates.
3. Iterate over all the pages.
4. Write a report footer. In the HTML report, also create a back-to-top link.
5. For a single page, report the number of checks and problems, plus the success rate.
6. For every `singleCheck` on that page, report a summary and all the detailed findings for a `singleCheck`.
7. For every checked page, create a footer, page break, or similar to graphically distinguish pages from each other.

The following sample report illustrates these steps:

![Figure 2.25: Sample report showing run/page/check hierarchy of results ](image/B15546_02_25.jpg)

###### Figure 2.25: Sample report showing run/page/check hierarchy of results

## II.9 Design Decisions
#### Note
You may like to understand some of the important, huge, expensive, risky, or otherwise special architecture and design decisions.

It's especially interesting to understand the reasons for these decisions.

### 9.1 Checking of External Links Postponed
In the current version of `HtmlSC`, we can't check external links. These checks have been postponed for later versions.

### 9.2 HTML Parsing with jsoup
To check HTML, we parse it into an internal (`DOM`-like) representation. For this task, we use `jsoup` ([https://jsoup.org](https://jsoup.org)), an open source parser without external dependencies.

To quote from their website:

"*jsoup is a Java library for working with real-world HTML. It provides a very convenient API for extracting and manipulating data, using the best of DOM, CSS, and jQuery-like methods*."

Goals of this decision: Check HTML programmatically by using an existing API that provides access and finder methods to the `DOM` tree of the file(s) to be checked.

Decision criteria would be as follows:
* Few dependencies, so the `HtmlSC` binary stays as small as possible
* Very easy to use: Simple and elegant (accessor and finder) methods to easily locate images, links, and link targets within the `DOM` tree
* Highly flexible: Can parse files and strings (and other) input

Alternatives to the HTML parser would be as follows:
* `jsoup`: A plain HTML parser without any dependencies and a rich API to access all the HTML elements in the `DOM`-like syntax. This is a clear winner!
