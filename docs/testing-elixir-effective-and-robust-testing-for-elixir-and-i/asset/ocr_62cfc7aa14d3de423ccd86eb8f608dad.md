Your setups can build on top of each other. If you have a setup that's common to an entire file but also need a setup for your describe block, the setup in your describe can accept values passed to it from the higher-up describe by adding a pattern to the test's parameters.

```
```elixir
setup context do
  additional_value = "example value"
  Map.merge(context, %{additional_value: additional_value})
end
```
```

Adding a pattern can be helpful, but be careful that you're selectively choosing what should be moved into the higher-level setup block, since you're starting to spread the logic needed to understand each test to several different areas of your file. There's a lot of value in being able to read and understand a single test without having to look in different places in the file or codebase.

Later we'll discuss test cases, another construct to help reduce duplication in your code. They provide an additional layer of setup, this time across multiple test files. Because that setup is defined outside of your current test file, it's definitely worth taking the time to consider how to organize your tests to make sure that everything is easy to follow. Even without nested describe blocks, we're getting close to a readability and maintenance nightmare if we don't think about that balance when organizing our tests.

One other feature of setup can help maintain clarity in your tests while still reducing duplication in your individual tests or multiple setups: setup can accept the name of a function or a list of function names instead of a block. Those functions should conform to a pattern that accepts the existing context (the data passed by previous setup blocks or functions) and returns an updated context so that they can be piped. As you can see in the following example, with_authenticated_user/1 expects a map of