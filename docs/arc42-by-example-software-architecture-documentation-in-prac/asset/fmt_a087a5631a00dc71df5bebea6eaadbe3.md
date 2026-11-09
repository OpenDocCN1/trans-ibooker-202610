
The default configuration is available in `src/main/resources/application.properties`.

During development, these properties are merged with `src/main/resources/application-dev.p`. Additional properties can be added through the system environment or through `application-*.properties` in the current JVM directory.

During tests, an additional `application-test.properties` can be used to add or overwrite additional properties or values.

The following are the biking2-specific properties:

![Figure 4.35: biking2 properties ](image/B15546_04_35.jpg)

#### Figure 4.35: biking2 properties

## Internationalization

The only supported language is English. There is no hook for doing internationalization in the frontend and there are no plans for creating one.

## Migration

biking2 replaced a Ruby application based on the Sinatra framework. Data was stored in a SQLite database that had been migrated by hand to the H2 database.

## Testability

The project contains JUnit tests in the standard location of a Maven project. At the time of writing, those tests cover more than 95% of the code written. Tests must be executed during the build phase and should not be skipped.

## Build Management

The application can be built with Maven without external dependencies outside of Maven. `gpsbabel` must be on the path to run all the tests, though.

## IV.9 Architecture Decisions

We will now look into the various factors that will influence the architectural decisions to be made while building the application.

### Using GPSBabel to Convert TCX into GPX Format

**Problem**

Popular JavaScript mapping frameworks allow us to include geometries from GPX data on maps. Most Garmin devices, however, record track data in TCX format, so I needed a way to convert TCX into GPX. Both formats are relatively simple and, especially in the case of GPX, are well-documented formats.

**Constraints**

* Conversion should handle TCX files with single tracks, laps, and additional points without any problems.
* The focus of this project has been on developing a modern application backend for an AngularJS SPA, not parsing GPX data.

**Assumptions**

* Using an external, non-Java-based tool makes it harder for people who just want to try out this application.
* Although well-documented, both file types can contain a variety of information (routes, tracks, waypoints, and so on), which makes them hard to parse.

**Considered Alternatives**

* Writing my own converter.
* Using an existing Swiss-Army knife for GPS data; that is, GPSBabel ([https://www.gpsbabel.org](https://www.gpsbabel.org)). `GPSBabel` converts waypoints, tracks, and routes between popular GPS receivers such as Garmin or Magellan and mapping programs such as Google Earth or Basecamp. Literally hundreds of GPS receivers and programs are supported. It also has powerful manipulation tools for such data. These are used for filtering duplicates points or simplifying tracks, for example. It has been downloaded and used tens of millions of times since it was first created in 2001, so it's stable and trusted.

**Decision**

biking2 uses `GPSBabel` for the heavy lifting of GPS-related data. The project contains a README file stating that `GPSBabel` must be installed. `GPSBabel` can be installed on Windows with an installer and on most Linux systems through the official packet manager. For OS X, it is available via MacPorts or Homebrew.

### Using Local File Storage for Image and Track Data

**Problem**

biking2 needs to store "large" objects; that is, image data (biking and gallery pictures) as well as track data.

**Considered Alternatives**

* Using cloud storage, such as S3
* Using the local filesystem

**Decision**

I opted for my local filesystem because I didn't want to put too much effort into evaluating cloud services. If biking2 is being run in a cloud-based setup, we have to create an abstraction over the local filesystem that's currently being used.

## IV.10 Quality Scenarios

The following are the various quality scenarios that need to be taken care of while building the application.

### 10.1 Quality Tree

![Figure 4.36: Quality tree ](image/B15546_04_36.jpg)

#### Figure 4.36: Quality tree

### 10.2 Evaluation Scenarios

**Testability/Coverage**

Using `JaCoCo` during development and the build process ([https://info.michael-simons.eu/2014/05/22/jacoco-maven-and-netbeans-8-integration/](https://info.michael-simons.eu/2014/05/22/jacoco-maven-and-netbeans-8-integration/)) ensures that we get at least 95% code coverage.

**Testability/Independent from External Services**

The architecture should be designed in such a way that algorithms that depend on external services can be tested without having the external service available; that is, all the external dependencies should be mockable.

For example, `FetchBikingPicturesJob` needs a resource containing an RSS feed. Retrieving the resource and parsing it are at least two different tasks. Fetching the resource through a separate class, `DailyFratzeProvider`, makes testing the actual parsing independent from an HTTP connection and thus relatively simple.

## IV.11 Risks

biking2 has been up and running for nearly 2 years now, and the architecture contains no known risks for my usage scenario.

There is a possibility that the H2 database could be damaged due to the unexpected shutdown of the VM (that is, OS or hardware failure). This risk is mitigated by regularly backing up the serialized database file:

## IV.12 Glossary

![](image/B15546_04_37.jpg)

#### Figure 4.37: Glossary terms

# Chapter 5

# V - DokChess

By **Stefan Zörner**

"*Someday computers will make us all obsolete.*" - *Robert ("Bobby") Fisher, World Chess Champion 1972-1975, in 1975.*

This chapter describes the architecture of the chess program `DokChess`. I originally created it as an example for presentations and training on software architecture and design. Later on, I implemented it in Java and refined it for a German book on documenting software architectures (see [http://www.swadok.de](http://www.swadok.de)). The source code is available on GitHub and more information can be found at [http://www.dokchess.de](http://www.dokchess.de):

![Figure 5.1: DokChess virtual product box ](image/B15546_05_01.jpg)

#### Figure 5.1: DokChess virtual product box

In the following architectural overview, you will learn about the important design decisions of DokChess. It shows the requirements that relate to the structure and design, basic solutions to problems, the structure of the software, and the interaction of key elements. The outline of the content follows the arc42 template.

The target audience of this architectural overview is primarily software architects seeking advice and examples on how to document architecture appropriately.

Software developers who plan to create a chess program of their own will learn some valuable tips and will learn about software architecture too.

## V.1 Introduction and Requirements

This section introduces the task and outlines the objectives pursued by DokChess.

### 1.1 Requirements Overview

**DokChess**

The following are the distinct features of DokChess:

* DokChess is a fully functional chess engine.
* It serves both as an easily accessible and attractive case study of software architecture design, evaluation, and documentation.
* The understandable structure invites developers to experiment and to extend the engine.
* A high level of chess ability is not the goal. Nevertheless, DokChess manages to play games that casual chess players will enjoy.

**Essential Features**

Here are some of the essential features of DokChess:

* Complete compliance with the FIDE Laws of Chess
* Supports games against human opponents and other chess programs
* Masters fundamental chess tactics, such as fork and skewer
* Works with modern graphical chess frontends

### 1.2 Quality Goals
