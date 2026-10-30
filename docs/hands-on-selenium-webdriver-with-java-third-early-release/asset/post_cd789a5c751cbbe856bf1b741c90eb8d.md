
(determining user rights and privileges), among others.

- *Usability testing* evaluates how user-friendly a software application is. This assessment is also called User eXperience (UX) testing. A subtype of usability testing is:
  - *A/B testing* consists of comparing different variations of the same application to determine which one is more effective to its end-users.
- *Accessibility testing* evaluates if a system is usable by people with disabilities.

> **TIP**
>
> We use Selenium WebDriver primarily to implement functional tests (i.e., interacting with a web application UI to assess the application behavior). Still, it is unlikely to use WebDriver to implement structural tests. In addition, although it is not its principal usage, we can use WebDriver to implement non-functional tests, e.g., for load, security, accessibility, or localization (assessment of specific locale settings) testing (see Chapter 9).

## Testing Methodologies

The *software development lifecycle* is the set of the activities, actions, and tasks required to create software systems in software engineering. The moment at which software engineers design and implement test cases in the overall development lifecycle depends on the specific development process (such as iterative, waterfall, or agile, to name a few). Two of the most relevant testing methodologies are:

- *Test Driven Development (TDD)*. TDD is a methodology in which we design and implement tests before the actual software design and implementation. At the beginning of the XXI century, it became popular with the rise of *agile* software development methodologies, such as extreme programming (XP). In TDD, a developer writes first an (initially failing) automated test for a given feature. Then, the developer creates a piece of code to pass that test. Finally, the developer refactors the code to achieve or improve readability and maintainability.
- *Test Last Development (TLD)*. TLD is a methodology in which we design and implement tests after the implementation of the SUT. This practice is typical in traditional software development processes, such as waterfall (sequential), incremental (multi-waterfall), spiral (risk-oriented multi-waterfall), or the Rational Unified Process (RUP).

Another relevant testing methodology is *Behavior-Driven Development (BDD)*. BDD is a testing practice derived from TDD, and consequently, we design tests at the early stages of the software development lifecycle in BDD. To that aim, conversations between the final user and the development team (typically with the project leader, manager, or analysts) occur. These conversations formalize a common understanding of the desired behavior and the software system. As a result, we create acceptance tests in terms of one or more *scenarios* following a *Given-When-Then* structure:

- Given: Initial context at the beginning of the scenario.
- When: Event that triggers the scenario.
- Then: Expected outcome.

> **TIP**
> TLD is a common practice used to implement Selenium WebDriver. In other words, developers/testers do not implement a WebDriver test until the SUT is available. Nevertheless, different methodologies are also possible. For instance, BDD is a common approach when using WebDriver with Cucumber (see Chapter 9).

Closely related to the domain of testing methodologies, we find the concept of *Continuous Integration* (CI). CI is a software development practice where members of a software project build, test, and integrate their work continuously. Grady Booch first coined the term CI in 1991. Nowadays, it is a popular strategy to create software. As you can see in Figure 1-7, there are three separate stages in CI. First, we use a *source code repository*, a hosting facility to store and share the source code of a software project. We typically use a *Version Control System* (VCS) to manage this repository. A VCS is a tool that keeps track of the source code and who/when made each change (sometimes called *patch*). *Git*, initially developed by Linus Torvalds, is the preferred VCS today. Other alternatives are *Concurrent Versions System* (CVS) or Subversion (SVN). On top of Git, several *code hosting platforms* (such as GitHub, GitLab, or Bitbucket) provide collaborative cloud repository hosting services for developing, sharing, and maintaining software.

![](img/b67ec8e49249e340add6ed66ab413d78_44_0.png)

*Figure 1-7. CI generic process*

Developers synchronize a local repository (or simply, *repo*) copy in their local environments. Then, they do the coding work using that local copy, committing new changes to the remote repository (typically daily). The basic idea of CI is that every commit triggers the build and test of the software with the new changes. The test suite executed to assess if a patch does not break the build receives the name of *regression tests*. A regression suite can contain tests of different types, including unit, integration, end-to-end, etc.

When the number of tests is too numerous for regression testing, we typically choose only a part of relevant tests from the whole suite. There are different strategies to select these tests, for instance, *smoke testing* (i.e., tests that ensure the critical functionality) or *sanity testing* (i.e., tests that evaluate the basic functionality). Lastly, we can execute the complete suite as a scheduled task (typically nightly).

We need to use a server-side infrastructure called *build server* to implement a CI pipeline. When the regression tests fail, the build server usually reports the problem to the originating developer. Table 1-8 provides a summary of several build servers.

**Table 1-8. Build servers**

| Name | Description | License | Maintainer | Website |
|---|---|---|---|---|
| Bamboo | Easy to use with Jira (issue tracker) and Bitbucket | Commercial | Atlassian | https://www.atlassian.com/software/bamboo |
| GitHub Actions | Integrated build server in GitHub | Free for public repositories | Microsoft | https://github.com/features/actions |
| GitLab CI/CD | Integrated build server in GitLab | Free for public repositories | GitLab | https://docs.gitlab.com/ee/ci/ |
| Jenkins | Open-source automation server | MIT | Eclipse Foundation | https://www.jenkins.io/ |

> **TIP**
>
> I use a GitHub repository ([https://github.com/bonigarcia/selenium-webdriver-java](https://github.com/bonigarcia/selenium-webdriver-java)) to publish and maintain the test examples presented in this book. GitHub Actions is the build server for this repo (see [Chapter 2](#)).

We can extend a typical CI pipeline in two ways (see **Figure 1-8**):

- *Continuous Delivery (CD).* After CI, the build server deploys the release to a test environment and executes the automated acceptance tests (if any).
- *Continuous Deployment:* The build server deploys the software release to the production environment as the final step.

![](img/b67ec8e49249e340add6ed66ab413d78_47_0.png)

*Figure 1-8. Continuous Integration, Delivery, and Deployment pipeline*

Close to CI, the term DevOps (development and operations) has gained momentum. DevOps is a software methodology that promotes the communication and collaboration of different teams in a software project to develop and deliver software efficiently. These teams include developers, testers, QA (quality assurance), operations (infrastructure), etc.

## Test Automation Tools

We need to use some tooling to implement, execute, and control automated tests effectively. One of the most relevant categories for testing tools is the *unit testing frameworks*. The original framework in the unit testing family (also known as *xUnit*) is SmalltalkUnit (or SUnit). SUnit is a unit test framework for the Smalltalk language created by Kent Beck in 1999. Erich Gamma ported SUnit to Java, creating JUnit. Since then, JUnit has been very popular, and it has inspired other unit testing frameworks. **Table 1-9** summarizes some of the most relevant unit testing frameworks in different languages.
