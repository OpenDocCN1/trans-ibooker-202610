| Test lifecycle callbacks | To include custom logic during the test lifecycle | `BeforeAllCallback` `BeforeEachCallback` `BeforeTestExecutionCallback` `AfterTestExecutionCallback` `AfterEachCallback` `AfterAllCallback` |
| Parameter resolution | To inject parameters in test methods or constructors | `ParameterResolver` |
| Test templates | To implement tests using `@TestTemplate` | `TestTemplateInvocationContextProvider` |
| Conditional test execution | To enable or disable tests depending on custom conditions | `ExecutionCondition` |
| Exception handling | To handle exceptions during the test and its lifecycle | `TestExecutionExceptionHandler` `LifecycleMethodExecutionExceptionHandler` |
| Test instance | To create and process test class instances | `TestInstanceFactory` `TestInstancePostProcessor` `TestInstancePreDestroyCallback` |
| Intercepting invocations | To intercept calls to test code (and decide whether or not these calls proceed) | `InvocationInterceptor` |