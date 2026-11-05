```
<IncludeAssets>runtime; build; native; contentfiles;
    analyzers; buildtransitive</IncludeAssets>
    <PrivateAssets>all</PrivateAssets>
</PackageReference>
</ItemGroup>

<ItemGroup>
    <ProjectReference
        Include="..\CalculatorLib\CalculatorLib.csproj" />
</ItemGroup>
</Project>
```

8. Build the CalculatorLibUnitTests project.

# Writing unit tests

A well-written unit test will have three parts:

+   - **Arrange**: This part will declare and instantiate variables for input and output.
+   - **Act**: This part will execute the unit that you are testing. In our case, that means calling the method that we want to test.
+   - **Assert**: This part will make one or more assertions about the output. An assertion is a belief that, if not true, indicates a failed test. For example, when adding 2 and 2, we would expect the result to be 4.

Now, we will write some unit tests for the Calculator class:

+   1. Rename the file UnitTest1.cs to CalculatorUnitTests.cs and then open it.
+   2. In Visual Studio Code, rename the class to CalculatorUnitTests. (Visual Studio prompts you to rename the class when you rename the file.)
+   3. Import the Packt namespace.
+   4. Modify the CalculatorUnitTests class to have two test methods for adding 2 and 2, and adding 2 and 3, as shown in the following code:

```
using Packt;
using Xunit;

namespace CalculatorLibUnitTests
{
    public class CalculatorUnitTests
    {
```