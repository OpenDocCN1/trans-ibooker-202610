# End-to-End Tests

*End-to-end tests* are tests that sit one level higher than integration tests. Instead of testing the integration between components of the system or between the system and external dependencies, end-to-end tests exercise the whole system *from end to end* as it might be used by the final user.

Let’s look at the weather API example from a wider perspective and think about the whole system. End-to-end tests should test the system from the perspective of a user. In this case, an end-to-end test would spin up the system and perform assertions on the observable changes. For example, the end-to-end test could start the system and then wait for an alert about rain.

The principle behind end-to-end tests is to test the real system, so using dependency doubles rarely makes sense. However, the architecture induced by dependency doubles is still useful, since we can swap the doubles for the real external dependencies when running end-to-end tests.

Unfortunately, it’s hard to find patterns for end-to-end tests when it comes to interfacing with external dependencies, since finding patterns often depends on the nature of the external dependency as well as how the system’s allowed to use it.

It’s also hard to find patterns when it comes to an end-to-end test pretending to be a user of the system under test. As with the interaction with external systems, this heavily depends on the nature of the system we’re testing. For example, if we’re testing a system that exposes an HTTP API, then we might want to use an HTTP client in the end-to-end tests and perform actual requests to the systems like a client of the API would. If our system is a website that