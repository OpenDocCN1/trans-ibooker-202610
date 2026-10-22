The testing diamond focuses the most on integration tests. When the active record pattern is used, the system’s business logic is, by definition, spread across both the service and business logic layers. Therefore, to focus on integrating the two layers, the testing pyramid is the more effective choice.

# Reversed Testing Pyramid

The reversed testing pyramid attributes the most attention to end-to-end tests: verifying the application’s workflow from beginning to end. Such an approach best fits codebases implementing the transaction script pattern: the business logic is simple and the number of layers is minimal, making it more effective to verify the end-to-end flow of the system.

Figure 10-6 shows the testing strategy decision tree.

![](bbox=[0.14, 0.46, 0.86, 0.87])

Figure 10-6. Testing strategy decision tree