
The full source code is available from the preceding link – you may even want to configure your `Gradle` build to use this software. If you're writing documentation based on `Asciidoctor`, this would be a great idea!

But enough preamble. Let's get started.

## II.1 Introduction and Goals

This section will explain the driving forces for architecturally relevant decisions and important use cases or features, all of which will be summarized in a few sentences. If possible, refer to the existing requirements documentation.

The main goal of this section is to ensure that your stakeholders understand the solution at hand.

`HtmlSanityCheck` (`HtmlSC`) supports authors creating digital formats by checking hyperlinks, images, and similar resources.

### 1.1 Requirements Overview

#### Note

If you would like to have a brief explanation of the important goals and requirements, use cases, and features of the system, please refer to the existing requirements documentation.

It's important that you understand the central tasks of the system before you take a look at the architecture of the system (starting with `arc42-section 3`).

The overall goal of `HtmlSC` is to create neat and clear reports that show errors within HTML files. The following is a sample report:

![Figure 2.1: Sample report ](image/B15546_02_01.jpg)

###### Figure 2.1: Sample report

`HtmlSanityCheck` (`HtmlSC`) checks HTML for semantic errors such as broken links and missing images. It was created to support authors who create HTML as an output format. This operation can be explained as follows:

1. Authors write in formats such as `AsciiDoc` ([https://asciidoctor.org/docs/what-is-asciidoc/](https://asciidoctor.org/docs/what-is-asciidoc/)), `Markdown` ([https://www.daringfireball.net/projects/markdown/syntax](https://www.daringfireball.net/projects/markdown/syntax)), and other formats, which are transformed into HTML by the corresponding generators.
2. `HtmlSC` checks the generated HTML for broken links, missing images, and other semantic issues.
3. `HtmlSC` creates a test report that is similar to the well-known unit test report.

The following diagram depicts how `HtmlSC` operates:

![Figure 2.2: Semantic checking of HTML pages ](image/B15546_02_02.jpg)

###### Figure 2.2: Semantic checking of HTML pages

### Basic Usage

The usage of `HtmlSC` is explained as follows:

1. A user configures the location (directory and filename) of one or several HTML files and the corresponding images directory.
2. `HtmlSC` performs various checks on the HTML and reports its results either on the console or as an HTML report.

`HtmlSC` can be run from the command line or as a `Gradle` plugin.

### Basic Requirements

The requirements of `HtmlSC` are listed in the following table:

![Figure 2.3: Basic requirements ](image/B15546_02_03.jpg)

###### Figure 2.3: Basic requirements

### Required Checks

`HtmlSC` provides the following checks in HTML files:

![Figure 2.4: Required checks ](image/B15546_02_04.jpg)

###### Figure 2.4: Required checks

### 1.2 Quality Goals

#### Note

You need to understand the quality goals (architecture goals) so that you can align your architecture and design decisions with these goals.

These (usually long-term) quality goals diverge from the (usually short-term) goals of development projects. Keep these differences in mind!

The quality goals are as follows:

![Figure 2.5: Quality goals ](image/B15546_02_05.jpg)

###### Figure 2.5: Quality goals

#### Note

Especially when checking external links, the correctness of links depends on external factors, such as network availability, latency, and server configuration, where `HtmlSC` cannot always identify the root cause of potential problems.

### 1.3 Stakeholders

#### Note

You want an overview of the persons, roles, or organizations that affect, are affected by, or can contribute to the system and its architecture. Make the concrete expectations of these stakeholders with respect to the architecture and its documentation explicit. Collect these in a simple table.

For our simple `HtmlSC` example, we have an extremely limited number of stakeholders. In real life, you will likely have many more stakeholders! Let's go over these here:

![Figure.2.6: Stakeholders ](image/B15546_02_06.jpg)

###### Figure.2.6: Stakeholders

## II.2 Constraints

#### Note

You want to know the constraints that restrict your freedom regarding design decisions or the development process. Such constraints are often imposed by organizations across several IT systems.

`HtmlSC` should be:

* Platform-independent and should run on the major operating systems (Windows, Linux, and macOS).
* Implemented in `Java` or `Groovy`.
* Integrated with the `Gradle` build tool.
* Runnable from the command line.
* Have minimal runtime and installation dependencies (a `Java` runtime may be required to run `HtmlSC`).
* Developed under a liberal open source license. In addition, all the required dependencies/libraries need to be compatible with a Creative Commons license.

## II.3 Context

You want to know the boundaries and scope of the system to distinguish it from neighboring systems. The context identifies the systems that have relevant external interfaces.

### 3.1 Business Context

#### Note

You want to identify all the neighboring systems and the different kinds of (business) data or events that are exchanged between your system and its neighbors.

The following table lists all the neighboring systems, along with a description of each:

![Figure 2.7: Business context ](image/B15546_02_07.jpg)

###### Figure 2.7: Business context

### 3.2 Deployment Context

#### Note

You will also want to know about the technical or physical infrastructure of your system, along with the physical channels or protocols.

The following diagram shows the participating computers (nodes) and their technical connections, along with the major artifact of `HtmlSC`, `hsc-plugin-binary`:

![Figure 2.8: Deployment context ](image/B15546_02_08.jpg)

###### Figure 2.8: Deployment context

The following table explains the purpose of each node in `hsc-plugin-library`:

![Figure 2.9: Description of the components of the deployment context ](image/B15546_02_09.jpg)

###### Figure 2.9: Description of the components of the deployment context

## II.4 Solution Strategy

#### Note

You need to provide a brief summary and explanation of the fundamental solution ideas and strategies. These key ideas should be familiar to everyone involved in the development and architecture process.

Briefly explain how you achieve the most important quality requirements.

The following steps will explain the basics of the solution design:

1. Implement `HtmlSC` mostly in the `Groovy` programming language and partially in `Java` with minimal external dependencies.
2. We wrap this implementation into a `Gradle` plugin so that it can be used within automated builds. Details are given in the `Gradle` user guide ([https://docs.gradle.org/current/userguide/userguide.html](https://docs.gradle.org/current/userguide/userguide.html)). The `Maven` plugin is still under development.
3. Apply the template method pattern ([https://sourcemaking.com/design_patterns/template_method/](https://sourcemaking.com/design_patterns/template_method/)) to enable multiple checking algorithms (see the concept of checking algorithms) and both HTML (file) and text (console) output.
4. Rely on standard `Gradle` and `Groovy` conventions for configuration and have a single configuration file. For the `Maven` plugin, this might lead to problems.

## II.5 Building Block View

The building block view explains and divides the static decomposition of the system into building blocks (modules, components, subsystems, and packages) and their relationships. It shows the overall structure of the source code.

This view is organized in a top-down hierarchy.

### 5.1 HtmlSanityChecker (Whitebox)

The following diagram shows the whitebox:
