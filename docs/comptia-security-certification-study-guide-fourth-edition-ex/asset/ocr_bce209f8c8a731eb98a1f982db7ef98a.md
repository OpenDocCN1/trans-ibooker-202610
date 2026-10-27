There are different software development life cycle models to choose from. Two of the more popular models are the waterfall model and the agile model. They have the same phases but differ in how you move from one phase to the other:

+   - **Waterfall** With the waterfall model, you progress through each phase of the SDLC without the capability of going backward in phases at any time. The point is that you must be sure to complete each phase to its fullest so that there is no need to go back (which was viewed as costly when this model was created).
+   - **Agile** With the agile model, you can take a more iterative approach and go to a previous phase if you realize something was missed or needs to be reworked. For example, you could do some requirements gathering, do some design, and then implement a prototype for the customer to review. Then, based on feedback from the customer, you may have to revisit the first few phases to complete requirements gathering and the design of the application. During testing, you may find that you need to change the design or change the code. The agile model views the capability to go back to a phase as critical to application development.

# Application Environments

It is critical to run your newly created software through a large number of tests to ensure the software was developed following secure programming concepts. This means that the application is to move through different stages or environments within its lifetime. The following is the order of environments the application moves through:

+   1. **Development** The development environment is where the application is created and debugged to fix any application errors.
+   2. **Test** After the application is developed, it should be deployed to a test environment, which has all of the essential elements, such as a test database server, for the application to use as it runs in this environment.
+   3. **Staging** After testing, the application moves to a staging environment, which is as close to the production environment that you can get (think of it as a “fake production” environment). This is essentially the last testing environment for the application before it moves to production.
+   4. **Production** Once the application has been tested within all the environments, including the staging environment, you can then deploy the application to the production environment—which means the application is live and ready to use!
+   5. **Quality assurance** The quality assurance (QA) environment is used to test that the application is meeting customer expectations. The quality assurance environment is where all QA testing occurs.

> exam watch
> For the Security+ exam, remember the order of deployment to the application environments.