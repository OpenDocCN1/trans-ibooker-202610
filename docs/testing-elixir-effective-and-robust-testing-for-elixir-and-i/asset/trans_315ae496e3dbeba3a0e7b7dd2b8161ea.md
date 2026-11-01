
在每个测试中使用不同的磁带，你就能保证在这方面不会搞乱。

让我们回顾一下本节讨论的内容，并看看哪种方法最适合不同的使用场景。

## What Approach Is Best for Testing External Services?
## 测试外部服务的最佳方法是什么？

We saw three possible approaches to testing third-party HTTP services: making real requests, running a test server that mocks the service, or recording real requests and replaying them. As is often the case in our industry, there’s no definitive best approach among these. Each one fits some use cases better.
我们看到了三种可能的第三方 HTTP 服务测试方法：发出真实请求、运行模拟服务的测试服务器，或记录真实请求并重放它们。正如我们行业中经常出现的情况一样，这几种方法中没有绝对的最佳方法。每种方法都更适合某些使用场景。

If you can get away with making real requests to a service, then it’s often a good idea to do that. Many HTTP services provide sandbox environments specifically for testing or are idempotent, meaning that you can make requests without affecting the state of the service. Even in these cases, though, you might want to avoid the real requests approach if you don’t want to rely on an Internet connection in your tests. Nowadays it’s rare to not have an Internet connection available, but there are situations (like being on a flight) that take your Internet connection away. Relying on an Internet connection can also cause all sorts of weird problems if the connection is unstable. Personally, we’re fans of a workaround that gives you a nice way to deal with this: we tag our tests that use the Internet as @tag :requires_internet. This way, if you don’t have a connection available, you can run `mix test --exclude requires_internet` and still run all the tests that don’t require the connection. This is a good workaround if you’re rarely without a connection. One thing to consider is that this approach doesn’t necessarily let you test different kinds of responses from an external service (such as temporary errors), so you might have to mix this in with other approaches.
如果可以发出真实请求到服务，那么通常这样做是个好主意。许多 HTTP 服务提供专门用于测试的沙箱环境，或是幂等的，这意味着你可以发出请求而不影响服务的状态。即便如此，如果你不想在测试中依赖互联网连接，你可能仍想避免真实请求的方法。如今没有互联网连接的情况很少见，但确实存在（例如在飞行中）会失去互联网连接的情况。依赖互联网连接如果连接不稳定也会导致各种奇怪的问题。个人而言，我们喜欢一种变通方法，可以很好地处理这个问题：我们将使用互联网的测试标记为 @tag :requires_internet。这样，如果你没有可用的连接，可以运行 `mix test --exclude requires_internet` 仍然运行所有不需要连接的测试。如果你很少没有连接，这是一个不错的变通方法。需要考虑的一点是，这种方法不一定能让你测试外部服务的不同响应类型（例如临时错误），因此你可能需要将其与其他方法混合使用。

If you want tight control over how the third-party API should behave, then building a test server is the way to go. This lets you test different responses from the API and gives you flexibility. However, the trade-off is that you have to build the test server and then keep it up to date with the real API.
如果你想对第三方 API 的行为有严格控制，那么构建测试服务器是正确的方法。这让你可以测试 API 的不同响应并提供灵活性。然而，代价是你必须构建测试服务器，然后将其与真实 API 保持更新。

If you want to mimic the API perfectly and use one of the supported HTTP clients, then ExVCR is a good alternative. The trade-off is that you have to keep cassettes up to date, but that’s something that you can only avoid by going with the real requests approach anyway.
如果你想完美模拟 API 并使用其中一种受支持的 HTTP 客户端，那么 ExVCR 是一个不错的替代方案。代价是你必须保持磁带更新，但这只能通过采用真实请求方法来避免。

## Testing Non-HTTP Services
## 测试非 HTTP 服务

In the previous sections, we mostly talked about testing HTTP-based third-party services. We explored how to build doubles for those services and then how to test the interaction with the actual services. The concepts we discussed mostly apply to non-HTTP services as well. If you’re talking with a service that exposes a binary protocol, for example, you can build an interface (with a behaviour) that specifies how to interact with the service. Then you can use dependency doubles to test components that talk to the service through the interface. However, testing the actual interaction with a service that doesn’t expose an HTTP interface is a bit more complex because fewer tools are available. If you want to build a test server, you’ll often have to implement that yourself on top of lower-level protocols like TCP. If you want cassettes, you’ll have to implement all of the infrastructure needed for them yourself because there isn’t an ExVCR counterpart for protocols other than HTTP.
在前面的章节中，我们主要讨论了基于 HTTP 的第三方服务的测试。我们探讨了如何为这些服务构建双件，然后如何测试与实际服务的交互。我们讨论的概念大多也适用于非 HTTP 服务。例如，如果你与一个暴露二进制协议的服务通信，你可以构建一个接口（带有一个行为）来指定如何与服务交互。然后你可以使用依赖双件来测试通过接口与服务通信的组件。然而，测试与不暴露 HTTP 接口的服务实际交互会复杂一些，因为可用工具更少。如果你想构建测试服务器，你通常必须在 TCP 等低级协议之上自行实现。如果你想要磁带，你必须自行实现所有所需的基础设施，因为除了 HTTP 之外没有 ExVCR 的对应物。

Luckily, most third-party services provide HTTP APIs. Usually, the external systems that do not provide HTTP APIs are the ones we have control over, such as databases or message queues.
幸运的是，大多数第三方服务提供 HTTP API。通常，不提供 HTTP API 的外部系统是我们控制的系统，例如数据库或消息队列。

Now that we have a good mental image of how to test external services, let’s quickly discuss end-to-end tests.
现在我们对如何测试外部服务有了良好的概念，让我们快速讨论端到端测试。

## End-to-End Tests
## 端到端测试

*End-to-end tests* are tests that sit one level higher than integration tests. Instead of testing the integration between components of the system or between the system and external dependencies, end-to-end tests exercise the whole system *from end to end* as it might be used by the final user.
*端到端测试* 是比集成测试高一级的测试。它们不是测试系统组件之间或系统与外部依赖之间的集成，而是像最终用户使用那样对整个系统进行 *从头到尾* 的演练。

Let’s look at the weather API example from a wider perspective and think about the whole system. End-to-end tests should test the system from the perspective of a user. In this case, an end-to-end test would spin up the system and perform assertions on the observable changes. For example, the end-to-end test could start the system and then wait for an alert about rain.
让我们从更广的角度来看天气 API 示例并思考整个系统。端到端测试应从用户角度测试系统。在这种情况下，端到端测试会启动系统并对可观察的变化进行断言。例如，端到端测试可以启动系统然后等待关于降雨的警报。

The principle behind end-to-end tests is to test the real system, so using dependency doubles rarely makes sense. However, the architecture induced by dependency doubles is still useful, since we can swap the doubles for the real external dependencies when running end-to-end tests.
端到端测试背后的原则是测试真实系统，因此使用依赖双件很少有意义。然而，依赖双件带来的架构仍然有用，因为在运行端到端测试时我们可以将双件替换为真实的外部依赖。

Unfortunately, it’s hard to find patterns for end-to-end tests when it comes to interfacing with external dependencies, since finding patterns often depends on the nature of the external dependency as well as how the system’s allowed to use it.
不幸的是，就接口与外部依赖而言，很难找到端到端测试的模式，因为找到模式通常取决于外部依赖的性质以及系统被允许如何使用它。

It’s also hard to find patterns when it comes to an end-to-end test pretending to be a user of the system under test. As with the interaction with external systems, this heavily depends on the nature of the system we’re testing. For example, if we’re testing a system that exposes an HTTP API, then we might want to use an HTTP client in the end-to-end tests and perform actual requests to the systems like a client of the API would. If our system is a website that you can visit from the browser, we’d use tools that can script human-like browser interactions. We’ll have a deeper look at some of these approaches in Chapter 6, [Testing Phoenix](Testing Phoenix).
在端到端测试假装成为被测系统的用户时，也很难找到模式。就像与外部系统的交互一样，这在很大程度上取决于我们测试的系统的性质。例如，如果我们测试的系统暴露了 HTTP API，那么我们可能希望在端到端测试中使用 HTTP 客户端并像 API 客户端那样对系统执行实际请求。如果我们的系统是一个可以通过浏览器访问的网站，我们会使用可以脚本化类人类浏览器交互的工具。我们将在第 6 章 [测试 Phoenix](Testing Phoenix) 中更深入地研究其中一些方法。

## Wrapping Up
## 总结

We had a look at the practice of integration testing and end-to-end testing. We discussed in-depth ways to write integration tests for parts of the system that interface with external systems, and we looked at some Elixir-specific tooling for doing so. In the next chapter, we’ll look at testing one of the most peculiar tools in the Elixir and Erlang landscapes, the OTP set of abstractions. There we’ll see some of the things mentioned in this chapter being used in more practical scenarios.
我们研究了集成测试和端到端测试的实践。我们深入讨论了为与外部系统接口的系统部分编写集成测试的方法，并查看了一些 Elixir 特定的工具。在下一章中，我们将研究 Elixir 和 Erlang 生态系统中一个最特别的工具，即 OTP 抽象集合。在那里我们将看到本章提到的一些内容在更实际的场景中被使用。

## Footnotes
## 脚注

- [14] https://github.com/plataformatec/mox
- [15] https://github.com/elixir-plug/plug
- [16] https://github.com/ninenines/cowboy
- [17] https://github.com/PSPDFKit-labs/bypass
- [18] https://github.com/parroty/exvcr
- [19] http://erlang.org/doc/man/httpc.html
- [20] https://github.com/benoitc/hackney

## Chapter 3
## 第 3 章

### Testing OTP
### 测试 OTP

In the previous two chapters, we established a mental framework to think and work with different levels of testing. We now have the tools to test isolated pieces of code through unit testing, test parts of the system that interact together through integration testing, and test the whole system through end-to-end testing. We covered many patterns that are specific to Elixir but also talked about testing practices in general. In this chapter, we’re going to focus on something that’s specific to Elixir: the OTP set of abstractions. We’re going to dive deeper into testing a few OTP abstractions, such as GenServers and supervisors, as well as talk about some patterns and things to pay attention to when testing asynchronous code.
在前两章中，我们建立了一个思维框架来思考和处理不同层次的测试。我们现在拥有了通过单元测试测试孤立代码片段、通过集成测试测试相互交互的系统部分、以及通过端到端测试测试整个系统的工具。我们涵盖了许多特定于 Elixir 的模式，也讨论了一般的测试实践。在本章中，我们将专注于 Elixir 特有的东西：OTP 抽象集合。我们将更深入地研究一些 OTP 抽象的测试，例如 GenServer 和 supervisor，以及讨论一些模式和在测试异步代码时需要注意的事情。

We’re going to start this chapter by discussing how to test GenServers, which are usually considered the *fundamental* abstraction in OTP.
我们将从讨论如何测试 GenServer 开始，本章通常被认为是在 OTP 中 *基本* 的抽象。

### Testing a GenServer
### 测试 GenServer

Let’s refresh our memory: a GenServer is a process that holds state and optionally provides an interface to read or update that state. OTP abstracts all of the common parts of a GenServer, like spawning the process, having a receive loop, and sending messages. What’s left to the user is to implement callbacks that implement code that’s specific to a particular GenServer.
让我们刷新记忆：GenServer 是一个保存状态并可选地提供读取或更新该状态的接口的进程。OTP 抽象了 GenServer 的所有通用部分，如生成进程、拥有接收循环和发送消息。留给用户的是实现回调来实现特定 GenServer 的代码。

We’ll see how we can make use of a GenServer in the Soggy Waffle application, but we’d like to start off with a self-contained example. It’s going to make things easier when illustrating some concepts. We’ll use a GenServer that provides a simple rolling average of the last N numeric measurements given to it. We can start with a simple implementation by looking at the public API exposed by the GenServer:
我们将看到如何在 Soggy Waffle 应用中使用 GenServer，但我们希望从一个自包含的示例开始。这将在说明一些概念时使事情更简单。我们将使用一个 GenServer，它提供最后 N 个给定数值测量的简单滚动平均值。我们可以通过查看 GenServer 暴露的公共 API 来开始一个简单的实现：

```
testing_otp/rolling_average/overloaded_genserver.ex

def start_link(max_measurements) do
  GenServer.start_link(__MODULE__, max_measurements)
end

def add_element(pid, element) do
  GenServer.cast(pid, {:add_element, element})
end

def average(pid) do
  GenServer.call(pid, :average)
end
```

We have a function to start the GenServer, a function to store a new element in the GenServer, and a function to get the average of the elements that the GenServer is storing at a given time. From the public API, we know that this is going to be a GenServer (because of the calls to GenServer functions), but we don’t know what the internal state of the
我们有一个用于启动 GenServer 的函数、一个用于在 GenServer 中存储新元素的函数，以及一个用于获取 GenServer 在给定时间存储的元素平均值的函数。从公共 API 我们知道这将是一个 GenServer（因为调用了 GenServer 函数），但我们不知道内部状态的
