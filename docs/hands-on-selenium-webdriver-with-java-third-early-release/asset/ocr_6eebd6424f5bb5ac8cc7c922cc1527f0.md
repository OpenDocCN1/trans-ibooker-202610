We can extend a typical CI pipeline in two ways (see **Figure 1-8**):

+   - *Continuous Delivery (CD).* After CI, the build server deploys the release to a test environment and executes the automated acceptance tests (if any).
+   - *Continuous Deployment:* The build server deploys the software release to the production environment as the final step.

![](bbox=[0.143, 0.234, 0.857, 0.473])

*Figure 1-8. Continuous Integration, Delivery, and Deployment pipeline*

Close to CI, the term DevOps (development and operations) has gained momentum. DevOps is a software methodology that promotes the communication and collaboration of different teams in a project software to develop and deliver software efficiently. These teams include developers, testers, QA (quality assurance), operations (infrastructure), etc.

# Test Automation Tools

We need to use some tooling to implement, execute, and control automated tests effectively. One of the most relevant categories for testing tools is the *unit testing frameworks*. The original framework in the unit testing family (also known as *xUnit*) is SmalltalkUnit (or SUnit). SUnit is a unit test framework for the Smalltalk language created by Kent Beck in 1999. Erich Gamma ported SUnit to Java, creating JUnit. Since then, JUnit has been very popular, and it has inspired other unit testing frameworks. **Table 1-9** summarizes some of the most relevant unit testing frameworks in different languages.