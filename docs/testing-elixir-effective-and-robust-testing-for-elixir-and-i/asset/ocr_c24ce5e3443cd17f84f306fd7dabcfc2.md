# Chapter 1

# Unit Tests

For most engineers, the testing experience starts with the unit test. Unit tests are the easiest to write since they have a limited focus and therefore usually deal with less complexity than tests that involve large portions of an application, such as integration tests. While unit tests are less complicated, the testing tools and skills we use for them form the basis for all testing, making unit tests the perfect place to start our exploration.

A lot of engineers coming to Elixir have experience with unit testing in some other language with various testing frameworks. Most of that experience transfers over pretty well to Elixir, but implementing some of the concepts in Elixir can be a little different. For example, stubbing, a technique common in interpreted languages is more complex in Elixir, which is compiled. The functional nature of Elixir allows us to open up the definition of what constitutes a “unit,” giving us new flexibility in our test design.

The ExUnit tools we use in unit tests are the basis of all of our testing. While integration tests may introduce additional tooling, all of our tests will still be structured similarly to unit tests and use the same assertions. In this chapter, we’re going to learn how to define the scope of our tests, write tests for functional code, learn how to