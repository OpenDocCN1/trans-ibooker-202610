# 22

# Software Testing and Debugging

Obviously, software needs to be tested prior to releasing it. There are several stages of testing that are executed sequentially to make for the most effective defect detection. First developers test their code, then it gets handed off to software quality assurance (SQA) to continue testing. Developer testing consists of unit tests and integration tests. SQA testing focuses on system testing. Unit tests exercise individual components in isolation. Integration testing checks the interaction between modules in a controlled manner. System testing exercises the entire software package. Testing should be done in the following order:

Unit testing → Integration testing → System testing

where each step is successfully concluded before the subsequent testing step is begun. Testing in this order is significantly more productive than attempting to run these tests in a different sequence. When individual components are tested first (during unit test) then the subsequent testing can focus on finding integration or system defects with the assurance that individual modules are relatively robust. If all the software is tested at once, it becomes very difficult to locate the root cause of a defect. Lots of time is spent trying to determine where a defect is located. This causes significant loss to productivity. Details about the different types of testing can be found in the following sections.

# 22.1 Unit Tests

Unit tests are run against the smallest testable unit of code, which are methods, functions, or procedures (depending on the language). The purpose of unit tests is to make sure that the lowest layer of code works properly before proceeding to deliver or test software at a higher level. Unit tests are run by the software developers. They should be run prior to delivering code into the code repository. They certainly should be run before handing software off to the SQA organization to test.

Generally, software developers write specific unit test code to exercise their low-level code. Typically, unit tests and product software development are an iterative process. A software developer will write product code, then execute their unit tests, find errors, fix their code, and repeat until all unit tests pass. In many cases, the unit tests are written prior to writing product code. There is a development methodology called test-driven design where all the tests are written before any of the code is developed as a means of