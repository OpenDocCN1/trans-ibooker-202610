# 第 6 章

## 测试 Phoenix

虽然 Elixir 灵活到可以用无数种方式使用，但大多数编写 Elixir 的人最终会在某个时候使用 Phoenix 框架编写应用程序。框架提供了大量工具，可以相对轻松快速地构建 Web 服务器。Phoenix 在 Web 框架中很少见，因为它对应用程序的架构或文件结构没有强烈的偏好。它提供了一种标准化的方法来定义各种 Web 接口并将其连接到应用程序代码。本章将介绍可以测试 Phoenix 应用程序的不同方式，重点是通过 Web 和 WebSocket 接口测试整个应用程序。

虽然我们将介绍测试 Phoenix 特定模式的内容，但非常重要的一点是要理解，大多数情况下，测试 Phoenix 应用程序将使用我们在前几章中已经介绍过的一切：单元测试、集成测试、OTP 测试和 Ecto 测试（它们实际上只是单元和集成测试的特定变体）。我们不会在这里重新讨论这些想法。因此，本章并非完全独立。如果你不熟悉测试的基础知识，本章不会很有帮助。相反，我们强烈建议你先阅读前面的章节，以便熟悉单元测试、集成测试、测试 OTP 和测试 Ecto 的基础知识。

## Phoenix 在你的应用中的作用

我们所说的 Phoenix 框架实际上是多个库的组合：Ranch,[37] Cowboy,[38] Plug,[39] Phoenix,[40] 和 Ecto.[41] 这种组合提供了大量工具，可以相当轻松地启动 Web 服务器应用程序，借鉴了许多其他框架的历史，但它非常努力地不驱动你的应用程序设计。这最终意味着标准 Phoenix 应用程序可以分解为两个主要部分，即传入的 Web 接口部分（包括 Ranch、Cowboy、Plug 和 Phoenix）和应用程序的其余部分（主要是 Elixir 和 Ecto 代码）。在 Phoenix 的术语中，那就是 YourAppWeb 和 YourApp。虽然 Phoenix 的生成器和文档会推动用户使用上下文的概念来组织非 Web 代码，但事实是你可以按自己的方式编写和组织应用程序。因此，测试你的应用程序业务逻辑在任何方面都不是 Phoenix 特定的。它看起来就像任何 Mix 应用程序中的单元和集成测试一样。

Phoenix 允许用户以多种不同的方式与之交互：服务器渲染的 HTML、基于 JSON 的端点和 Phoenix Channels（WebSocket）。一个新的库 Phoenix LiveView 结合了服务器渲染 HTML 和 WebSocket 的元素来提供交互式客户端。它不在 Phoenix 核心中，因此我们不会在本書中介绍它。如果你想了解更多关于它以及如何测试它，请查看 Programming Phoenix LiveView [TD21]。

作为测试这些接口的一部分，我们还将涉及测试路由、plugs 和视图。使用 Internet 调用其他服务并在其中进行调用的代码不包括在内。该主题属于集成测试范畴，而该代码通常会位于应用程序的非 Web 部分。

测试每种接口之间有很多重叠。为了避免非常重复的示例，如果没有需要注意的地方，我们将跳过我们已经介绍过的内容。例如，如果我们在测试 JSON API 的一部分中介绍测试 plugs，我们就不会在测试服务器渲染的 HTML 端点时介绍 plugs。

## Plugs

Phoenix 库对任何想要编写基于 Elixir 的 Web 应用程序的人的生产力都非常有帮助，但了解它很大程度上归功于另一个库 Plug.[42] Plug 是一个基本规范和工具集，允许用户创建可组合的 Web 应用程序。在本章中，我们将提到“plugs”作为你在应用程序中可能使用的中间件的参考。此外，Plug 库提供了我们所需的某些测试工具。别担心：因为 Plug 是 Phoenix 框架的依赖项，如果你正在 Phoenix 应用程序中工作，你已经可以使用 Plug 及其测试工具了。我们将把任何模块，无论是由 Phoenix 提供还是自定义编写，只要符合 Plug 定义的接口，都称为 plugs。

## 如何使用提供的应用程序

随书附带了一个名为 NotSkull 的应用程序，它将作为本章和下一章的大部分代码示例。NotSkull 是一个功能齐全的应用程序，包含测试。

在本章学习过程中你将编写的测试都将在新文件中。如果遇到困难，你会发现一个完整的测试文件，其名称与我们要求你创建的文件相同，但文件扩展名是 .bak.exs 而不是 .exs。

应用程序中的一些功能在接口类型之间是重复的。例如，用户有两个控制器：一个用于服务器渲染的 HTML，另一个用于基于 JSON 的 API。不过功能并不完全一致，因为基于 JSON 的代码只是为了提供本书的示例而添加的。

就像在 测试 Ecto 模式 章节中一样，示例代码提供了一个 Makefile 来帮助启动数据库。需要注意的是，虽然应用程序代码和测试是好的，但数据库设置对于实际应用程序来说是不够的。重要的是要使用比我们的示例应用程序提供的更强的安
全实践。不要在实际应用程序中使用我们的数据库设置。

本章将只介绍应用程序中编写的代码和测试的一小部分，但应用程序已使用本书中介绍的相同概念进行了测试。随意使用这些示例作为你自己测试的起点。



## 测试基于 JSON 的 API

我们将从查看测试基于 JSON 的 API 开始。这对于 Phoenix 来说是一个非常常见的用例，也是我们将要测试的接口中最不复杂的。这将使我们能够专注于一些适用于我们将涵盖的所有接口类型的概念，而不会在接口本身的具体细节上迷失太多。

## 控制器测试需要覆盖的内容

我们力求让控制器尽可能小，分支逻辑尽可能少。这实现了两个目标：它将业务逻辑保留在应用程序的 Web 部分之外，并且将所需的测试用例数量降至最低。控制器测试是我们最昂贵的测试之一（就运行时间而言），因为它们通常需要最多的设置。理想情况下，我们在仍保持应用程序具有良好覆盖率的同时，尽可能少地拥有它们。

大多数控制器操作的逻辑可以用两个测试来测试：一个 happy path 测试和一个错误情况测试。对此总会有例外，但这应该是你的起点。

除了测试操作本身的逻辑之外，你可能还需要包含测试来验证某些 plugs 是否包含在应用程序的调用栈中。在本节中，我们将测试一个基于 JSON 的端点，包括 happy path 和错误情况，并测试 plug 的存在。

在开始编写测试之前，让我们先看看我们将要测试的代码以及它如何融入应用程序。

## 熟悉用户控制器

我们的应用程序是一个简单的游戏。它是经典游戏 Skull and Roses 的简化版本。在我们的版本中，玩家轮流猜测其他玩家是否展示了骷髅或玫瑰。我们的关注点从玩家在应用程序中注册为用户开始。用户随后能够创建新游戏或编辑自己的个人资料设置。虽然应用程序对用户注册和个人资料更新有 HTML 渲染的控制器操作，但基于 JSON 的 API 提供了相同的功能。我们将专注于测试编辑端点，这意味着我们的测试将从已注册的用户开始。让我们看看当用户点击更新端点以更改自己的个人资料时调用的控制器代码：

```
testing_phoenix/not_skull/lib/not_skull_web/controllers/json_api/user_controller.ex

1: defmodule NotSkullWeb.JsonApi.UserController do
-   @moduledoc false
-
-   use NotSkullWeb, :controller
5:
-   alias NotSkull.Accounts
-
-   def update(conn, params) do
-     with {:ok, user} <- Accounts.get_user_by_id(params["id"]),
         {:ok, updated_user} <- Accounts.update_user(user, params) do
10:
-
         conn
         |> put_status(200)
         |> json(user_map_from_struct(updated_user))
     else
         {:error, error_changeset} ->
15:
-
         conn
         |> put_status(422)
         |> json(errors_from_changeset(error_changeset))
     end
end
20:
-
-   defp user_map_from_struct(user) do
-   user
-   |> Map.from_struct()
    |> Map.drop([:__struct__, :__meta__])
25:
-   end
-   
-   defp errors_from_changeset(changeset) do
-   serializable_errors =
      for {field, {message, _}} <- changeset.errors,
30:
-       do: %{"field" => to_string(field), "message" => message}
-   %{errors: serializable_errors}
-   end
```

文件的开头看起来像任何其他控制器文件。update 函数是一个标准的控制器操作，接受一个 Plug.Conn 和一个 params 映射。有一个双条件 with 语句（第 9 行），如果成功则执行提供的块中的代码（第 11 行），如果出现问题则在错误处理（第 16 行）中执行。

只有现有用户才能调用此端点。他们必须登录，并且只能尝试编辑自己的信息。为此，应用程序具有 JSON Web Tokens (JWT) 的最小实现。[43] 调用更新用户的端点将需要一个签名 JWT，通过登录控制器登录获得。编码的 JWT 包含用户的 ID，使应用程序能够验证用户是否已登录。此解码和验证逻辑存在于自定义 plug 中（位于 lib/not_skull_web/plugs/validate_jwt.ex），这就是为什么它不会在此文件中看到的原因。还有另一个 plug 确保 JWT 中的用户 ID 与路径中的用户 ID 匹配。

这两个 plugs 都经过单元测试，这是可能的，因为 plugs 是纯函数式的。虽然本章不会涵盖所使用的 plugs 的测试，但它们在应用程序中是经过测试的。被测试的代码以及验证 JWT 的测试分别可以在

lib/not_skull_web/plugs/validate_jwt.ex 和 test/not_skull_web/plugs/validate_jwt_test.exs 找到。被测试的代码可以在 lib/not_skull_web/plugs/match_jwt_user_id.ex 找到，测试位于 test/not_skull_web/plugs/match_jwt_user_id_test.exs。除需要符合 plug 回调的标准（plugs 必须有一个接受 Plug.Conn 和参数的 call/2 函数）外，你可以看到测试的结构就像针对任何纯函数式代码一样，只关注返回值。

虽然我们不会花更多时间在 plugs 的测试上，但我们需要编写测试以确保它们被用于此端点。在围绕控制器本身的逻辑编写了一些测试后，我们可以学习如何测试 plugs 的存在。

让我们开始编写测试。

## 测试更新端点

在包含的仓库 NotSkull 中，在 test/not_skull_web/controllers/json_api/user_controller_test.exs 创建一个新的测试文件。我们在 json_api 下添加了嵌套，以避免与我们用于服务器渲染 HTML 端点的用户控制器的测试冲突。

以下代码将为你创建的文件添加基本结构和 setup 块：

```
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

1: defmodule NotSkullWeb.JsonApi.UserControllerTest do
  -   use NotSkullWeb.ConnCase, async: false
  -   alias NotSkull.Accounts.User
5:
  -   describe "PUT /api/users/:id" do
  -     setup context do
  -       {:ok, user} = Factory.insert(:user)
```
```elixir
conn_with_token =
  put_req_header(
    context.conn,
    "authorization",
    "Bearer " <> sign_jwt(user.id)
  )

Map.merge(context, %{user: user, conn_with_token: conn_with_token})
end
end
end
```

在启动新的 Phoenix 项目时，生成器会创建几个不同的 ExUnit 测试用例。其中一个，ConnCase，或我们在应用程序中的 NotSkull.ConnCase，将用作所有基于 HTTP 的接口的测试用例。它通过“使用”该模块引入，如第 2 行所示。这提供了许多助手，使我们更容易构建和操作 conns，或 Plug.Conn 结构体。这些助手是在测试 Web 接口时主要额外的组件。此外，使用测试用例还会导入我们在 NotSkullWeb.ConnCase 本身中定义的自定义函数。这允许我们在不同测试中多次需要的函数创建并使其在需要它们的测试中可用。

传递给 describe 的字符串打破了我们使用函数名和 arity 的模式，因为作为控制器测试，练习将命中端点而不是调用特定函数。因此，我们已根据 HTTP 操作和路径命名 describe 块："PUT /api/users/:id"。

## 利用 ConnCase

我们的测试将使用一个常见的 setup 块，因为大多数测试都需要两件事：数据库中存在一个用户以及该用户的签名 JWT。默认在每个测试中都有你的设置是很好的，这样测试在阅读时无需到别处查看。然而，如果有多个测试需要相同的设置，将这些通用代码移动到一个地方是有用的，在本例中是 setup 块。JWT 作为 Conn 中的一个头包含在内，如第 10 行所示。实际的 JWT 由 NotSkullWeb.ConnCase 中定义的函数创建（第 14 行调用），因为我们需要将其逻辑与登录控制器的其他控制器测试文件共享。虽然深入研究 JWT 如何创建并不是特别有帮助，但在这些测试中用 put_req_header（第 11 行）向请求头添加内容是非常常见的。该函数来自 Plug.Conn，并通过 NotSkullWeb.ConnCase 导入。

与我们在前几章中编写的大多数测试不同，我们的 setup 块接受一个 context。此 context 来自测试用例。生成的文件有一个看起来像这样的函数：

```
testing_phoenix/not_skull/test/support/conn_case.ex

1: setup tags do
  -   Mox.verify_on_exit!()
  - 
  -   :ok = Ecto.Adapters.SQL.Sandbox.checkout(NotSkull.Repo)
5:
  -   unless tags[:async] do
  -     Ecto.Adapters.SQL.Sandbox.mode(NotSkull.Repo, {:shared, self()})
  -   end
  - 
      {:ok, conn: Phoenix.ConnTest.build_conn()}
10:
  -   end
```

首先值得注意的行为是它将沙箱模式设置为 :shared（第 7 行）。此设置很重要，因为它会影响测试的运行方式。共享模式允许任何进程访问相同的

database transaction, behaving like a database would when running in production. Our interface (API) tests are integration-style, and there’s a good chance that many processes will want to access the same data, for example the user we seeded in the database. You’ll notice that there’s a conditional (unless) wrapped around the line setting the mode. Remember the async: false from the use line of our test file? This code evaluates the value passed for async. This unless block makes it so that any test using NotSkullWeb.ConnCase will default the sandbox mode to shared mode unless it’s overridden with async: true. We added async: false to our test file because being explicit is always good.

Also in that function is a call to the library Mox, on line 2, which we discussed in Chapter 2, Integration and End-to-End Tests. This is a good reminder that the setup block in a test case will be shared by all tests that use that file (unless specifically overridden). In this case, setting Mox to a specific mode doesn’t hurt the tests that don’t use Mox, so while it doesn’t pertain to the tests we’re looking at, it’s not a problem to have it there, either.

The setup block returns a new Plug.Conn because it’s assumed that all the tests using the file will need it.

## 定义错误测试

It’s common to have your success tests before your error tests, but that doesn’t mean you have to write them in that order. In our case, we’re going to start with the simplest test for this endpoint: our error case.

Add the following test inside of the describe block you already created:

```
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

1: test "error: does not update, returns errors when given invalid attributes",
   %{
     conn_with_token: conn_with_token,
     user: existing_user
5:   } do
   conn =
   put(conn_with_token, "/api/users/#{existing_user.id}", %{name: ""})
   
   assert body = json_response(conn, 422)

10:

   user_from_db = Repo.get(User, existing_user.id)
   assert user_from_db == existing_user
   
   actual_errors = body["errors"]
refute Enum.empty?(actual_errors)

15:

   
   expected_error_keys = ["field", "message"]
   
   for error <- actual_errors do
    assert_unordered_lists_are_equal(

20:

   actual: Map.keys(error),
   expected: expected_error_keys
   )
   end
  end
25:
```

As mentioned before, the test accepts a context from the setup block, in this case giving it a user and a Plug.Conn that contains a header with a JWT for that user (line 3). Because the setup is done before the test, the very first line of the test body is the call to exercise (line 7). It’s using the function put/3, which it has imported from Phoenix.ConnTest by “using” NotSkullWeb.ConnCase. Similar functions are available for each of the HTTP action types, some of which we’ll use later. In this case, the function takes a Plug.Conn (we’re passing the one with the JWT), the

path for the endpoint, and a PUT body. The return value from all of them is an updated Plug.Conn that contains the response data.

Line 9 uses json_response/2 to do two things: it checks that the HTTP status code on the returned Plug.Conn matches the code that’s passed as the second argument (we’re expecting a 422 in this case). If those values match, it parses the body field of the Plug.Conn from JSON into an Elixir map. While normally it’s not great to do two things at once, especially two unrelated things like an assertion and a transformation, this function has been around for years and has never caused any issues. When it fails, it provides errors that make it clear if it’s failing because of the status code or because it can’t parse the body to JSON.

The rest of the test is fairly straightforward. It checks that, because this is an error case, the data in the database hasn’t been changed as a result of the HTTP call (line 12). This is an assertion that’s often overlooked in error cases but needs to be there. Because the input was invalid, nothing should have been changed in the database. This is an extra check on top of the assertions on the return value. Unlike in most unit tests, endpoint tests, because they’re inherently integration tests, almost always includes assertions for the code under test’s return value and assertions on its side effects.

Speaking of return values, the test also verifies that the response body has errors that are maps with the keys "field" and "message" (line 19). The test does NOT check for specific errors, just that they are shaped correctly. We’ve already written tests to make sure that the correct error is handed back from our database code. This test is just focused on the controller and making sure that it calls out to the right code and can properly handle an error. Since our application standardizes the way we handle errors, we only need one somewhat generic test to confirm that our controller can handle any error correctly.

We are going to add three more tests, but we won’t dive as deeply into what’s happening in each of them, as they build on the basic structure that we just wrote.

## 测试成功路径

Our next test will be our happy path. In the same describe block, add this test, ideally before the error case that we just reviewed:

```
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

1: test "success: updates db returns record with good params", %{
  conn_with_token: conn_with_token,
  user: existing_user
} do
5: new_name = "#{existing_user.name}-updated"

conn =
  put(conn_with_token, "/api/users/#{existing_user.id}", %{
    name: new_name
  })

10:

  assert parsed_return = json_response(conn, 200)

  user_from_db = Repo.get(User, existing_user.id)

15:

  assert_values_for(
    expected: %{existing_user | name: new_name},
    actual: user_from_db,
    fields: fields_for(User) -- [:updated_at]
  )

20:

  assert DateTime.to_unix(user_from_db.updated_at, :microsecond) >
    DateTime.to_unix(existing_user.updated_at, :microsecond)

### checking that the updated record is what is returned from endpoint

assert_values_for(
  expected: user_from_db,
  actual: {parsed_return, :string_keys},
  fields: fields_for(User),
  opts: [convert_dates: true]
)
end
```

The test is structured very similarly to the error test we wrote before it.
The common elements are these:

- It accepts a context with a Plug.Conn that contains a valid JWT.
- It calls the endpoint using put/3, a helper from the Phoenix library.
- It uses json_response/2 to assert on the HTTP response code and to deserialize the JSON response.
- It makes assertions on the return value.
- It makes assertions on the side effects.

In this case, it differs from the error case at the assertions. Because it’s a happy path test, we should see that the HTTP response code was a 200. The return value should be a serialized version of the updated record.

The assertions on the return value and the side effect are very related here. Assuming the code under test is working correctly, the return value should be the same information that’s in the row in the database: the updated user. We also want to assert that the return value is the original record updated with the new params, in this case just the new

name. But it’s important to lock down that the only values that changed are the ones that were passed in through the endpoint. To assert that the values are correct for both the return value and the data in the database (the side effect), we’ll turn to a custom helper function, called assert_values_for/1.

## 我们的自定义测试助手

All of the code for the helper function is provided in code/testing_phoenix/not_skull/test/support/assertion_helpers.ex, and you have access to it in the tests you are writing via use NotSkullWeb.ConnCase. It’s more useful here to discuss *what* it does and *why* it does it than how. The function accepts two pieces of data for which Enumerable is implemented. It also takes a list of field names to check against. Assuming atom-based keys, but allowing for the option of string keys, it iterates through and checks that every field named has the same value in both the *expected* and the *actual* data. It also accepts an option to convert between Elixir DateTime structs and ISO 8601 strings. The *why* of this function is simple: most engineers don’t explicitly test all of the data available in their tests because writing assertions on every field is tedious. By creating an easy-to-use, easy-to-reason-around, thorough helper function, we’re attempting to eliminate this bad habit.

The first use of assert_values_for/1 is on line 16, where we define the expected to be the existing user’s values updated with the new name (from the parameters passed in the exercise call). A second helper function called fields_for/1 is used on line 19. It returns a list of all the fields for whatever you pass it. In our case, we’re giving it the User struct. Because it’s a list, we can easily remove any fields we want to skip by using Elixir low-level list subtraction, Kernel.--/2. In our case, we’re removing :updated_at from the list of values to check because it’ll have changed and we don’t have a way to know the new value from inside our test. We’ve tested enough of the fields to have confidence that our code called the correct function(s) under the hood. Because we emphasize thoroughness, though, we’ve added one last assertion to

同时覆盖 :updated_at as well，位于第 22 行。我们的代码调用的是经过充分测试的代码，因此此断言可以被视为可选。

我们对 assert_values_for/1 的第二次使用在第 26 行，在那里我们检查响应中的数据。由于我们已经断言数据库中的数据是正确的，现在可以将其视为预期数据。因为返回的数据和数据库中的数据来源相同，所以它们的值应该完全相同，除了由于序列化为 JSON 而导致格式发生变化的情况；键和日期时间将是字符串。assert_values_for/1 提供了选项，因此我们在测试中无需进行任何转换——辅助函数可以处理它们。

我们的测试现在已经检查了此场景相关的每个值。控制器代码将参数发送到正确的位置，引起正确的副作用，并且按预期渲染了所有数据。我们已完成 happy path 测试并测试了控制器正确处理错误的情况。这构成了您在应用程序的任何接口上想要的最低基本覆盖范围。不过，正如前面提到的，在命中更新端点时应调用两个插件。让我们看看如何测试这些调用是否发生。

## 测试插件的存在

虽然您编写的任何基本插件都应进行单元测试，但我们需要在测试套件中添加测试，以断言它们在包含它们的管道中的每个端点上都被调用。虽然每个 Plug 都不同，但在我们的用例中，两者都在调用栈中控制器之前运行。我们编写的现有 happy path 测试涵盖了事情正确的情况。我们只需要测试事情不正确的情况。这意味着对同一插件(s)存在性的多个端点进行测试会开始显得有点重复，因为设置（如果存在）和断言几乎完全相同。通常只有端点会发生变化。我们将添加两个测试来断言我们两个插件的存在。

在路由器（code/testing_phoenix/not_skull/lib/not_skull_web/router.ex）中，我们的更新端点位于包含我们两个自定义插件的管道中：ValidateJWT 和 MatchJWTUserId。让我们逐一查看它们，了解其作用，然后添加测试以确保其存在。

第一个 Plug，ValidateJWT，解码 JWT 并确保其有效。如果有效，Plug 会从 JWT 中取出用户的 ID 并将其添加到传入的参数中。如果 Plug 未检测到有效的 JWT，它将停止调用栈，返回 HTTP 状态码 401（“Unauthenticated”）和一个包含描述性错误的简单 JSON 响应。

在同一个 describe 块“PUT /api/users/:id”中，在您的错误测试之后添加以下新测试：

```
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

1: test "auth error: returns 401 when valid jwt isn't in headers", %{
  -   conn: conn,
  -   user: existing_user
  - } do
5:   conn =
  -   put(conn, "/api/users/#{existing_user.id}", %{
  -     name: "#{existing_user.name}-updated"
  -   })
  - 
    assert body = json_response(conn, 401)
10:
  - 
  -   assert %{
  -     "errors" => [
  -     %{"message" => "Invalid token.", "field" => "token"}
    ]
15:
  -   } == body
  -
```

```
- user_from_db = Repo.get(User, existing_user.id)
- 
  assert_values_for(
20:
-   expected: existing_user,
-   actual: user_from_db,
-   fields: fields_for(User)
- )
  end
25:
```

我们已经在 setup 中有代码确保我们拥有正确签名的 JWT；但在这种情况下，我们不想要它，因此我们的测试使用由 NotSkullWeb.ConnCase 中的 setup 块提供给它的原始 Plug.Conn。您可以看到它在第 2 行从上下文中获取它。添加一个带有错误编码 JWT 的标头也是可以的。两者在我们的 Plug 中解码时结果相同。我们选择了工作量稍少一点的选项。

该练习看起来像本 describe 块中我们其他测试的练习步骤，如第 6 行所示。我们使用有效的一组参数，因为即使这些数据永远不应该到达底层的更新和 changeset 功能，我们的测试也将我们的代码视为黑盒。因此，实现可能会发生变化，但我们需要我们的测试锁定外部行为。在这种情况下，如果 JWT 无效，我们不仅应收到错误响应，我们已对其进行测试，而且数据库中的数据也不应被更改。我们稍后会解决该断言。

我们的测试断言响应体是正确格式化的错误，如第 12 行所示。以前测试错误时，我们费尽心思不假设我们知道所有错误，而是只确保我们期望的错误存在。这需要列表推导和一些匹配的工作。在这种情况下，我们正在测试一个插件，只有在它之前执行的所有代码成功时才应运行。如果前面的代码失败，则不应运行任何其他代码。因此，错误是唯一应存在的一个，所以我们可以通过与整个响应体进行比较来节省一些步骤。

此测试的最后一部分相当标准。因为调用未通过身份验证，所以不应该有副作用。正如我们在几段之前提到的，我们需要确保数据库中的数据没有发生变化。在第 18 行，我们从数据库中获取行，然后使用 assert_values_for/1 确保它与调用前记录的相同，即绑定到变量 existing_user 的数据。

我们的 Plug 测试调用预期具有该插件的端点。它故意使插件中的检查失败，短路调用栈并强制返回一个值。然后测试对返回值和数据库中的数据（检查副作用）进行断言。这与我们在同一端点的其他测试非常相似。

我们对此端点的最后一个测试是我们的第二个 Plug 测试。这次，它是一个基本的插件，用于确保发出请求的<|message|> 用户与正在更新的记录是同一用户。打开同一个测试文件（test/not_skull_web/controllers/json_api/user_controller_test.exs）并在同一个 describe 块（“PUT /api/users/:id”）的底部添加此最后一个测试：

```elixir
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

test "auth error: returns 403 when path and jwt user ids don't match", %{
  conn_with_token: conn_with_token,
  user: existing_user
} do
conn =
  put(conn_with_token, "/api/users/#{Factory.uuid()}", %{
    name: "#{existing_user.name}-updated"
  })

assert body = json_response(conn, 403)

assert %{
    "errors" => [
    %{
        "message" => "You are not authorized for that action.",
        "field" => "token"
    }
    ]
} == body

user_from_db = Repo.get(User, existing_user.id)

assert_values_for(
    expected: existing_user,
    actual: user_from_db,
    fields: fields_for(User)
)
end
end
end
```

该测试本身的结构与我们其他 Plug 测试类似。不同之处在于它使用带有有效令牌（JWT）的 Plug.Conn，并且 URL 中的用户 ID 与令牌中编码的不同。断言类似，检查状态码（这次是 403），检查响应体中的错误(s)，最后断言没有副作用。

通过运行这些测试，我们已经为我们对其他类型应用程序接口的测试奠定了基本结构，可以在此基础上使用或构建。让我们接下来看看服务器渲染的 HTML 端点。

## 测试服务器渲染的 HTML 应用

测试服务器渲染的 HTML 端点与测试 JSON 端点非常相似，但响应是 HTML 文档而不是 JSON。我们将为处理服务器渲染的 HTML 端点的用户控制器编写测试。让我们先看看我们将要测试的控制器代码：

testing_phoenix/not_skull/lib/not_skull_web/controllers/user_controller.ex

```
defmodule NotSkullWeb.UserController do
  use NotSkullWeb, :controller

  alias NotSkull.Accounts
  alias NotSkull.Accounts.User
  alias NotSkull.ExternalServices.Email
  def new(conn, _params) do
    user = User.create_changeset(%{})
    render(conn, "new.html", changeset: user)
  end

  def create(conn, %{"user" => user_params}) do
    case Accounts.create_user(user_params) do
      {:ok, user} ->
        Email.send_welcome(user)

        conn
        |> put_session(:user_id, user.id)
        |> put_flash(:info, "Your account was created successfully!")
        |> redirect(to: Routes.user_path(conn, :show, user))

      {:error, %Ecto.Changeset{} = changeset} ->
        render(conn, "new.html", changeset: changeset)
    end
  end

end
```



这段代码是一个控制器，其内部调用的上下文模块与我们已测试过的 JSON 控制器相同。我们的测试将集中在调用 new 和 create 控制器动作的两个端点上。new 渲染用于添加用户的 HTML 表单，而 create 将接受新用户的参数，并在数据良好时创建该用户。我们将在编写测试之前深入了解每个端点的细节。

## 测试 New

开始时，在提供的应用程序中，在 test/not_skull_web/controllers/user_controller_test.exs 创建一个新的测试文件，并添加以下文件结构：

```
testing_phoenix/not_skull/test/not_skull_web/controllers/user_controller_test.bak.ex

defmodule NotSkullWeb.UserControllerTest do
  use NotSkullWeb.ConnCase, async: false

  alias NotSkull.Accounts.User
  end
```

这段代码应与我们针对 JSON API 端点的测试非常相似。我们使用完全相同的测试用例 NotSkullWeb.ConnCase 并别名 NotSkull.Accounts.User。我们没有包含任何设置，因为我们要测试的第一个端点——渲染表单——不需要设置。此外，由于控制器或其调用的代码中没有分支逻辑，我们只需要一个 happy path 测试。在你的文件中，添加以下 describe 块和测试：

```
testing_phoenix/not_skull/test/not_skull_web/controllers/user_controller_test.bak.ex

1:  describe "GET /users/new" do
2:    test "success: it renders the form", %{conn: conn} do
3:    conn = get(conn, Routes.user_path(conn, :new))
4:
5:    assert response = html_response(conn, 200)
6:
7:     assert response =~ "Create a new account"
8:   end
9: end
```

与我们的 JSON API 端点测试有很多共同之处。第 1 行显示我们使用 HTTP 方法和 URL 路径来描述测试块。我们在第 2 行接受来自测试上下文的 Plug.Conn。第 3 行有一个值得提及的差异。不是像之前那样向控制器测试的辅助函数 get/2 传递一个路径，而是传递 Routes.user_path(conn, :new) 的结果。此辅助函数由 Phoenix 框架提供。你可以选择是否用它代替路径。它可以在路径发生细微变化时使维护变得稍微容易一些，但它会使调用的是哪个端点变得不够清晰。如果你没有偏好，就像我们在 JSON API 测试中所做的那样使用路径，而不是辅助函数。

第 5 行显示我们使用了一个略有不同的辅助函数 html_response/2，它由 Phoenix 提供，就像 json_response/2 一样。它们的工作方式非常相似，首先断言 HTTP 状态码；但与解码后的 Elixir map 不同，此函数返回原始 HTML 字符串。这就引出了我们在第 7 行的最后一个断言，它使用模糊匹配运算符 Kernel.=~/2 来确保字符串 "Create a new account" 存在于 HTML 响应中。此测试假设如果 HTML 中存在该确切字符串，则整个表单一定已被返回。鉴于本书大部分内容都在鼓励非常全面的测试覆盖，这可能感觉有点轻量。它确实如此。开箱即用的 Phoenix 并没有提供验证整个 HTML 负载的方法。

稍后，当我们介绍错误情况时，我们会提供更明确的方法，但这是一个很好的时机提醒你，即使你的测试设计良好且通过，你仍应运行代码并亲眼验证应用程序是否按预期运行。在这种情况下，这意味着运行你的服务器 mix phx.server，并访问端点 http://localhost:4000/users/new。你应该能看到表单被渲染。即使我们对该端点的每个方面都在测试中进行了锁定，你仍应始终通过手动操作来确保代码按预期运行。

## 测试 Create 的 Happy Path

现在我们已经介绍了 new 端点的测试，该端点渲染用于创建用户的表单，让我们来看看 create 端点。这是被我们之前 new 端点渲染的表单所调用的端点。让我们再仔细看看代码并讨论我们需要测试的内容：

```
def create(conn, %{"user" => user_params}) do
  case Accounts.create_user(user_params) do
    {:ok, user} ->
      Email.send_welcome(user)

      conn
      |> put_session(:user_id, user.id)
      |> put_flash(:info, "Your account was created successfully!")
      |> redirect(to: Routes.user_path(conn, :show, user))

    {:error, %Ecto.Changeset{} = changeset} ->
      render(conn, "new.html", changeset: changeset)
  end
end
```

此函数中存在分支逻辑，取决于 Accounts.create_user(user_params) 的成功与否。因此，我们知道至少需要两个测试，一个 happy path 测试和一个错误测试。有四个我们之前未测试过的新代码元素：发送电子邮件（作为副作用的外部调用）、在会话中设置值、设置 flash 以及重定向调用。让我们先编写 happy path 测试，因为它将涵盖这些内容。打开你的测试文件并按如下方式添加一个新的 describe 块和测试：

```
1: describe "POST /users" do
  - test "success: creates_user, redirects to show page when user is created",
- %{conn: conn} do
- params = Factory.atom_params(:user)
5:
- expect_email_to(params.email)
- conn = post(conn, Routes.user_path(conn, :create), user: params)
- assert %{id: id} = redirected_params(conn)
10:
- assert redirected_to(conn) == Routes.user_path(conn, :show, id)
- assert %{"user_id" => ^id} = get_session(conn)
- assert get_flash(conn, :info) =~ "success"
15:
- user_from_db = Repo.get(User, id)
- fields_to_check = Map.keys(params) -- [:password]
- assert_values_for(
20:
- expected: params,
- actual: user_from_db,
- fields: fields_to_check
- )
25:
- assert user_from_db.password
- end
- end
```

像之前的测试一样，这个测试从测试设置中继承了 Plug.Conn。在第 4 行，我们从工厂获取有效参数。第 6 行调用自定义辅助函数 expect_email_to/1，它设置了一个期望，即我们将尝试发送电子邮件。当我们完成对该测试的审阅后，我们将回过头来看该辅助函数的代码。现在，我们可以从名称中假设它创建了一个期望，即电子邮件将被发送到我们参数中的地址。如果没有发生这种情况，我们将期望测试失败。

在执行行之后，该行使用 Phoenix 提供的 post/3 结合 Routes.user_path/2，我们可以看到我们没有将响应解析为 HTML。这是因为在用户创建成功时，控制器应返回重定向而不是页面。我们需要新创建用户的 ID，以便确保重定向到预期的路径并在数据库中找到该用户。Phoenix 提供了一个辅助函数 redirected_params/1 来提供重定向中使用的参数。我们的测试在第 10 行使用该函数来获取该 ID。

现在我们知道了新用户的 ID，我们可以断言服务器对成功创建的响应是重定向到“show”端点，如第 11 行所示。我们可以假设该端点也单独进行了测试，因此我们的测试可以仅限定在创建用户的控制器操作中的交互。

我们即将展示我们讨论过的第一章《单元测试》中测试黑盒的第一个，也是可能唯一的违反。应用程序中注册行为的一部分是将新用户的 ID 设置在服务器会话中，以便后续对 API 的调用无需每次都提供登录凭据即可使用它。如果我们编写的测试通过多个 API 调用练习整个流程，我们将利用会话，而如果会话中未设置该 ID，后续调用将失败。但我们是在隔离状态下测试此端点。我们仍然需要确保代码正在设置该 ID。第 13 行确保从 redirected_params/1 获取的 ID 以“user_id”为键设置在会话中。

由于响应是重定向，我们需要检查的唯一面向用户的元素是 flash 是否设置正确。第 14 行展示了如何使用

使用 get_flash/2 从 flash 中检索特定键（我们这里是 :info）的内容。然后我们断言消息是我们所期望的。

测试的最后一部分是验证副作用，即新用户的创建。我们使用 assert_values_for/1 来检查除 :password 之外的所有参数是否都已进入数据库。:password 的值无法直接检查，因为它不会是相同的值而是哈希值。我们无法预测该值会是什么。我们可以断言密码值已被设置。之后，我们可以确信我们的代码调用了预期的上下文函数，并且上下文函数已被测试得足够充分，以至于我们不需要在控制器测试中编写更明确的断言。

## 测试副作用——调用第三方

我们之前说过要查看断言邮件已发送的辅助函数。让我们看看这段代码，它已包含在示例应用程序中。它在 NotSkullWeb.ConnCase 中定义，名为 expect_mail_to/1。以下是代码：

```elixir
1:  def expect_email_to(expected_email_address) do
     Mox.expect(HttpClientMock, :request, fn method,
                                         url,
                                         _headers,
5:                                         json_body,
                                         _opts ->
     assert method == :post
     assert url == "https://api.sendgrid.com/v3/mail/send"

      decoded_body = Jason.decode!(json_body)
10:

     assert %{
         "personalizations" => [
         %{
             "to" => [
15:
     %{"email" => ^expected_email_address}
     ]
     }
     ]
} = decoded_body
20:

   end)
 end
```

如第 2 行所示，该函数是对预定义 Mox 调用的包装。名称足够具体，可以让我们知道它在做什么，而无需担心具体如何实现。这很有用，因为在测试中一个描述性强的函数调用可以比在测试中放置整个 Mox 函数设置更具可读性。当然，这也使其可重用。

请注意，我们没有在传递给 Mox.expect/3 的匿名函数中钉住参数。相反，我们在匿名函数内部进行了断言。这是因为当有多个必需值时，钉住值在失败时会抛出 FunctionClauseError，这并不会突出显示哪个值不匹配。将每个值放在单独的断言中会提供即时、清晰的反馈，指出哪个值不正确。这并不意味着如果需要你不能钉住值。你可以在第 16 行看到这一点。我们仍然可以在模式匹配中使用钉住值，但它发生在匿名函数内部，而不是函数头中。

我们的测试辅助函数确保向客户发送电子邮件的细节存在，但它并没有进一步断言电子邮件的内容。你可以自行决定是否认为额外的断言有帮助。发送电子邮件的模块需要单独进行单元测试。我们可以让辅助函数更不通用，传入一个字符串作为参数并确保该字符串在电子邮件正文中。我们选择不这样做，因为此测试只需要确保电子邮件已发送，而明确的测试留给单元测试。此外，
你的应用程序发送的任何电子邮件都应手动测试以确保其正确渲染。因此，在端点测试中变得更明确是多余的。

## 测试创建的错误情况

我们通过测试失败情况来完成对该端点和控制器操作的覆盖。在同一个 describe 块中，在之前的测试下方，添加以下内容：

```
1: test "error: does not insert, redirects to 'new' page w/invalid attributes",
   %{
     conn: conn
   } do
5: flunk_if_email_is_sent()
   expected_user_count = Repo.all(User) |> Enum.count()
   conn = post(conn, Routes.user_path(conn, :create), user: %{})
   assert html = html_response(conn, 200)
10:
   parsed_html = Floki.parse_document!(html)
   for field <- ["name", "email", "password"] do
     # using <> so that the first #(indicating an id) is easy to read
     field_as_id = "#" <> "#{field}-error"
15:
     span = Floki.find(parsed_html, field_as_id)
     assert Floki.text(span) == "can't be blank"
   end
   assert Repo.all(User) |> Enum.count() == expected_user_count,
20:
     "There should have been no records inserted during this test."
   end
```

该测试利用了一个名为 flunk_if_email_is_sent/0 的新自定义辅助函数，在第 5 行定义于 NotSkull.ConnCase 中，紧邻我们的其他辅助函数。与之前一样，我们在讨论完此测试的其余部分后会查看该代码。

由于我们的测试是失败情况，它需要确保没有新用户作为副作用被创建。在第 7 行，测试获取了数据库中用户在执行前的数量。这使我们能够在测试结束时断言（第 20 行）数量没有变化。虽然不是绝对可靠，但这给了我们相当高的信心认为没有任何变化。我们不断言数量为零，虽然由于沙箱它很可能为零，因为测试实际上并不关心数据库中是否没有数据。它只关心在执行被测代码后是否没有新数据。这样，如果添加新功能并且通用设置更新为为不同测试添加用户，我们的测试仍会通过并提供相同的信心，即没有添加新用户。这减少了测试套件所需的维护。始终值得寻找让测试易于维护的机会。

与我们的 happy path 测试相比，此测试的一个重大区别在于我们使用不同的方法对 HTML 中返回的值进行断言。在第 13 行，我们有一个列表推导式，遍历注册表单中三个已知的字段名称。由于我们的测试发布了空参数，渲染的 HTML 应包含每个字段为空的警告。在传递给列表推导式的代码块中，我们使用了一个名为 Floki 的较新库。[44] Floki 使你能够为特定选择器搜索 HTML，在我们的三种情况下为 #name-error、#email-error 和 #password-error。调用 Floki.text/1 并传递该元素将返回文本，如第 17 行所示，使我们能够对该值进行断言。

## 修改后的样板代码

![](img/c08a369b9a1934f7c9c3b485727eca8e_281_0.png)

当 Phoenix 生成新项目时，应用程序会有一个生成的 ErrorHelpers 模块，其中包含一个函数 error_tag/2，它间接创建表单错误的 HTML。它通常不允许传入 CSS ID。我们的示例代码已修改该函数以允许可选的 ID。你可以在包含的应用程序的 file not_skull/test/support/assertion_helpers.ex 中看到修改后的函数 error_tag/3。该函数是生成的，不是 Phoenix 库的一部分，专门用于允许这种针对你需求的定制。

使用 Floki，并确保我们的 HTML 结构良好且包含页面动态元素的 CSS ID，使我们能够非常具体地确认视图代码工作正确。我们现在可以确定我们的应用程序不仅渲染了正确的视图，而且该视图中的信息也是正确的。我们有效地测试了端点的返回值。

让我们通过查看辅助函数 flunk_if_email_is_sent/0 来结束这次测试探索。与 expect_mail_to/1 一样，该函数是对一些 Mox 功能的包装，如下所示：

```
def flunk_if_email_is_sent do
  Mox.stub(HttpClientMock, :request, fn _, _, _, _, _ ->
    flunk("An email should not have been sent.")
  end)
end
```

如果在测试执行期间出于任何原因发送了电子邮件，测试将失败。我们选择使用 Mox.stub/3 而不是 Mox.expect/4 with an

期望零次调用，因为这样可以通过在 ExUnit 的 flunk/1 中添加内容来编写自定义错误消息。这只是测试风格上的偏好。使用替代方案同样可行，但如果失败，错误信息会更模糊一些，无法更清楚地说明发生了什么。

在此测试中，我们通过使用 Floki 对 HTML 中的动态信息进行断言，从而对端点返回的值进行了断言。我们的测试还覆盖了副作用，具体来说是确保没有创建新用户且没有发送电子邮件。我们可以确信我们的测试能够保护我们免受回归的影响。

通过这些示例，我们涵盖了针对服务器端渲染 HTML 端点的控制器测试所特有的关注点。你可能会想到这里未涵盖的边缘情况。当你想到时，试着记住将大部分分支逻辑移出控制器，放入可以单独测试的小文件中。这将使你能够将更大、更昂贵的测试（即控制器测试）的范围限制在最小。控制器测试应仅关注确保“你的所有连接都没有接错”。换句话说，控制器测试确保你在单元测试中测试过的所有代码作为一个整体应用程序正确交互。

Phoenix 库的核心中还包含最后一个接口，即 Phoenix Channels。让我们看看如何测试它们。

## Testing Phoenix Channels

Phoenix Channels 是对 WebSocket 的抽象，它在某种开放的传输协议之上放置了一定量的框架代码。它们对如何使用 WebSocket 提供了足够的意见，使得入门变得更容易，并且有一个写得很好且得到良好支持的 Phoenix JavaScript 包，使得构建 JavaScript 客户端相对容易。[45] 幸运的是，Phoenix 还包含大量测试助手（称为 Phoenix.ChannelTest，并通过生成的 ChannelCase 模块引入），使测试 Channels 代码也变得相当直接。

与基于 JSON 的 API 和服务器端渲染的 HTML 不同，这里似乎有必要多谈一下 Channels 的实际用例，以设定我们将要测试的代码的上下文。Phoenix Channels 通常与一个完全开发的前端 JavaScript 客户端一起使用。本书中包含的应用程序使用非常少量的 JavaScript 来修改服务器端渲染的 HTML 视图。在任何一种情况下，服务器端的 Channel 代码都是本书关注的重点。我们不会专注于测试 JavaScript。

本节不会穷举测试 Phoenix Channel 代码的每一种方法，但它会涵盖足够的内容，使测试 Channels 的工作原理变得清晰。它还将深入探讨与库提供的特定于测试 Phoenix Channels 的测试工具相关的一些细微差别。

### Testing the UserSocket

当生成一个 Phoenix 项目时，它会附带一个样板 UserSocket 模块。我们已向我们的 UserSocket 模块添加了一些身份验证代码，使其看起来像以下代码：

```
testing_phoenix/not_skull/lib/not_skull_web/channels/user_socket.ex

1: defmodule NotSkullWeb.UserSocket do
- use Phoenix.Socket
- alias NotSkull.{Accounts, JWTUtility}
- require Logger
5:
- channel "lobby:*", NotSkullWeb.LobbyChannel
- 
- @impl true
- def connect(%{"token" => token}, socket, _connect_info) do
  jwt = URI.decode_www_form(token)
10:
- 
- with {:ok, user_id} <- JWTUtility.user_id_from_jwt(jwt),
-     {:ok, _valid_user} <- Accounts.get_user_by_id(user_id) do
-   socket = assign(socket, :user_id, user_id)
-   {:ok, socket}
15:
- 
- else
-   something_else ->
-     Logger.warn(inspect(something_else))
-   :error
- end
20:
- 
- end
- 
- def connect(_, _socket, _connect_info), do: :error
- 
- @impl true
25:
- def id(socket), do: "user_socket:#{socket.assigns.user_id}"
- end
```

与样板相比最大的变化在第 12、13 和 14 行。
在第 12 行，我们正在验证 JWT 并获取关联的用户 ID。在第 13 行，我们确保用户 ID 确实在我们系统的数据库中。你的身份验证选择可能有所不同，但重要的是要确保你的用户 socket 上有身份验证。如果这两个

检查都成功，该函数将返回一个 socket，其中从 JWT 中获取的用户 ID 设置在 `assigns` 下的键 `:user_id` 中。

我们对 happy path 的测试需要检查用户 ID 是否已添加到 `assigns` 中，以及由第 26 行的 `id/1` 函数确定的 socket ID。通常，我们不会在一个测试中覆盖两个公共函数的行为，但由于 Channels 的实现方式，我们必须在 `connect/3` 的测试中验证 ID 函数的行为，并且该函数不会在其他任何时候被调用。由于此代码路径可能失败的两个可能位置，我们需要确保对这两个代码路径都有测试。

### Avoid Logging Tokens

![](img/c08a369b9a1934f7c9c3b485727eca8e_285_0.png)

这与测试无关，但如果你的使用模式与我们示例代码中的模式相似，则很重要。JWT 身份验证令牌作为查询参数传递。即使你的应用程序使用加密协议，请求在服务器端以未加密形式被记录的情况也很常见。请确保你的日志不包含查询参数，因为记录未加密且未过期的 JWT 可能会在你的安全上打开一个漏洞。任何可以访问你的日志的人都可以冒充被记录 JWT 的用户。

要开始测试此代码，请创建一个名为 `test/not_skull_web/channels/user_socket_test.exs` 的文件，并使用以下测试结构：

```
testing_phoenix/not_skull/test/not_skull_web/channels/user_socket_test.bak.exs
```

```elixir
1: defmodule NotSkullWeb.UserSocketTest do
2:   use NotSkullWeb.ChannelCase
3:   alias NotSkullWeb.UserSocket
4:
5:   describe "connect/3" do
6:   end
7: end
```

此文件与我们所有其他测试的结构相同。最值得注意的是第 2 行，我们使用的是 NotSkullWeb.ChannelCase。这是一个新的测试用例，与我们其他用例非常相似，但它导入了 Phoenix.ChannelTest，即我们之前提到的 Channel 特定工具。它还包含一行 @endpoint NotSkullWeb.Endpoint，我们将在编写一些测试后讨论。添加你的第一个测试，一个 happy path，用于检查在传入有效 JWT 时是否会返回 socket，通过在 describe 块内添加以下代码：

```elixir
1: test "success: allows connection when passed a valid JWT for a real user" do
2:   {:ok, existing_user} = Factory.insert(:user)
3:   jwt = sign_jwt(existing_user.id)
4:
5:   assert {:ok, socket} = connect(UserSocket, %{token: jwt})
6:   assert socket.assigns.user_id == existing_user.id
7:   assert socket.id == "user_socket:#{existing_user.id}"
8: end
```

此测试使用了 Phoenix.ChannelTest（通过我们的 ChannelCase 模块）提供的第一个测试助手，在第 5 行称为 Phoenix.ChannelTest.connect/3。必须理解，这并不是直接调用我们被测模块上的 connect/3。调用该函数需要使用别名后的模块名调用，UserSocket.connect(UserSocket, %{token: jwt})。UserSocket.connect/3 没有可选参数，而 Phoenix.ChannelTest.connect/3 有，这就是我们的测试可以只传递两个参数调用它的原因。

这是测试 Channels 的一个明显细微差别。当应用程序生成时设置的样板代码

在 ChannelCase 模块中包含一行 @endpoint NotSkullWeb.Endpoint。此行设置了 Phoenix.ChannelTest.connect/3 宏使用的模块属性。你可以查看源代码以更好地了解其工作原理。[46] 此宏接受 socket 模块的名称，在我们的例子中是 NotSkullWeb.UserSocket，以及参数。它调用在 “使用” Phoenix.Socket 时由宏添加到 NotSkullWeb.UserSocket 中的函数，并最终调用我们在 NotSkullWeb.UserSocket 文件中定义的 connect/3 函数，将来自测试的参数和一个 Socket 结构体传递进去。主要的收获只是要理解第 5 行对 connect/3 的调用有效地是对你被测模块中函数 connect/3 的调用。

由于这是一个 happy path 测试，代码的返回值将是一个包含 :ok 和 Socket 结构体的元组，我们将其称为 socket。被测代码没有副作用，因此现在需要做的就是确保返回的 socket 具有正确的信息。第一个断言是用户 ID 已放置在 socket 的 assigns 中。这在第 6 行得到覆盖。如我们之前提到的，我们的代码还将为 socket 分配一个 ID。此 ID 是一个字符串，当用户需要向特定用户发送定向消息而不是向订阅该频道的每个人广播时就会使用。id/1 返回的字符串需要特定于 UserSocket，同时也包含用户的 ID。我们在第 7 行的断言检查了这一点。这是以一种迂回的方式检查我们模块中另一个公共函数 id/1，该函数与 connect/3 一样，是在 “使用” Phoenix.Socket 时所需实现的一部分。

我们的 happy path 测试覆盖了传入有效参数时的返回值。现在我们需要为可能出错的两种方式编写错误测试。在同一个代码块内，添加以下测试：

```elixir
@tag capture_log: true
test "error: returns :error for an invalid JWT" do
  assert :error = connect(UserSocket, %{token: "bad_token"})
```

```
end
```

这段代码中第一个检查是为了验证是否存在一个良好的 JWT。我们测试所需要做的就是传入一个已知的错误令牌值。在我们的例子中，“bad_token”就足够了，并且从阅读测试就能清楚地看出我们是如何导致失败的。我们函数的契约是，在这种情况下返回值应仅为原子 :error。我们的测试正在捕获日志，以便测试输出不会因代码中预期的日志而受影响。

包含无效用户 ID 的有效 JWT 的错误情况并没有复杂多少。在同一个 describe 块中，添加这个最后的测试：

```
@tag capture_log: true
test "error: returns :error if user doesn't exist" do
  jwt = sign_jwt(Factory.uuid())

  assert :error = connect(UserSocket, %{token: jwt})
end
```

此测试使用我们的 JWT 辅助函数创建一个包含错误用户 ID 的有效 JWT。我们可以通过为此负载专门创建一个新的 UUID 来保证用户 ID 会是错误的。它不可能存在于数据库中。就像之前的测试一样，我们能够在同一行中同时断言返回 :error 并执行被测代码。同样与之前的测试一样，日志被捕获，因为我们期望代码会产生错误日志。

通过这三个测试，我们现在已经覆盖了调用 connect/3 时可能出现的所有代码路径。UserSocket 模块将用于连接到我们的服务器上的 Channel，我们目前有 LobbyChannel，但也适用于未来可能需要的任何其他 Channel。我们已经覆盖了连接代码。接下来，让我们查看 LobbyChannel 中的代码并学习测试它的细微差别。

## 测试 Channel

我们在 NotSkull 示例应用中提供的 Phoenix Channel 示例 LobbyChannel 功能相当有限。在对其进行测试时，我们不会详尽地展示 Phoenix.ChannelTest 中包含的所有测试宏的用法，但我们会涵盖足够的内容，以便你了解如何使用它们所有。让我们来看一下文件：

testing_phoenix/not_skull/lib/not_skull_web/channels/lobby_channel.ex

```elixir
defmodule NotSkullWeb.LobbyChannel do
  use NotSkullWeb, :channel

  alias NotSkull.GameEngine.Game

  @impl true
  def join("lobby:lobby", _payload, socket) do
    {:ok, socket}
  end

  @spec broadcast_new_game(Game.t()) :: :ok | :error
  def broadcast_new_game(%Game{current_phase: :joining} = game) do
    NotSkullWeb.Endpoint.broadcast!("lobby:lobby", "new_game_created", %{
      game_id: game.id
    })
  end

  def broadcast_new_game(_) do
    :error
  end
end
```

此文件仅提供两个函数，join/3 和 broadcast_new_game/1。join/3 在第 7 行，是 Channel 框架的必需回调，并且简单地返回一个 socket，假设它是用有效参数调用的。因此，我们将在测试的设置部分覆盖此功能。第 13 行是 broadcast_new_game/1，它将广播包含新游戏 ID 的映射到连接到 Channel 的任何客户端。

broadcast_new_game/1 还有一个子句，如果传入的不是 %Game{} 结构体，或者游戏结构体 current_state 的值不是 :joining，则返回错误。我们需要一个 happy path 测试以及针对落入错误子句的每个状态的错误测试。在 test/not_skull_web/channels/lobby_channel_test.exs 创建一个新测试文件，并用以下代码填充测试文件：

```
testing_phoenix/not_skull/test/not_skull_web/channels/lobby_channel_test.bak.exs

1: defmodule NotSkullWeb.LobbyChannelTest do
-   use NotSkullWeb.ChannelCase
-   alias NotSkullWeb.{LobbyChannel, UserSocket}
-
5:   describe "broadcast_new_game/1" do
-   setup do
-     user_id = Factory.uuid()
-
-     {:ok, _, socket} =
        UserSocket
10:
-       |> socket("user_socket:#{user_id}", %{user_id: user_id})
-       |> subscribe_and_join(LobbyChannel, "lobby:lobby")
-
-       %{socket: socket}
    end
15:
-   end
-   end
```

Phoenix Channel 的典型测试将从假设用户已经通过 UserSocket 中的代码连接到 websocket，并且已加入正在测试的 Phoenix Channel 开始。

Phoenix.ChannelTest 提供了两个测试函数，我们可以在 setup 块中使用它们来满足这些条件。

第一个测试函数是 socket/3，我们在第 11 行调用它。它接受的参数是你要连接的 socket 的模块名称，在我们的例子中是 UserSocket，单个 socket 将拥有的 ID，以及应在 assigns 中期望的值。后两个参数需要明确了解 UserSocket 代码的作用。传入的 ID 应该与 UserSocket.id/1 生成的 ID 相同，并且 assigns 需要与 UserSocket.connect/3 中放入其中的内容相匹配。虽然这并不理想，但你会发现 UserSocket 代码通常变化很小。因此，在设置中设置这些值后，很少需要再次修改它们。

一旦 socket 创建完成，我们需要让该 socket 加入实际的 Phoenix Channel 本身并订阅来自 Channel 的事件。名为 subscribe_and_join/3 的函数，在第 12 行调用，提供正是此功能。它需要参数为：由前一个函数 socket/3 创建的 socket、Phoenix Channel 的模块名称，在我们的例子中是 LobbyChannel，以及模块下测试（LobbyChannel）的正确 join/3 函数的主题。我们之前提到过我们不会直接测试 join/3，但它会在我们的设置中被覆盖。这是针对测试函数构建方式的回应。如果你想在 LobbyChannel.join/3 中加入更多逻辑，最好为 join/3 创建一个 describe 块，并在你的执行步骤中使用其中一个 Phoenix.ChannelTest.join 函数（三种不同的 arity，具体取决于你的需求）。在我们的例子中，函数只有一种响应方式，因此专注于它会是过度测试。

在我们进行此调用后，了解“什么”已订阅到 Channel 很重要。当调用 subscribe_and_join/3 时，订阅的是测试进程本身。这意味着测试进程的邮箱是 Channel 消息到达的地方。我们即将描述的测试助手抽象了直接检查邮箱的需求，但了解幕后发生的事情是有用的。

这引导我们为 Channel 中的另一个函数 broadcast_new_game/1 编写测试。在内部，此函数使用 NotSkullWeb.Endpoint.broadcast!/3，它是从 Phoenix.Endpoint 导入的函数。让我们在文件中的 setup 之后添加以下测试，并查看如何测试输出：

```
test "success: returns :ok, sends broadcast when passed an open game" do
  open_game = Factory.struct_for(:game, %{current_phase: :joining})

  assert :ok = LobbyChannel.broadcast_new_game(open_game)

  assert_broadcast("new_game_created", broadcast_payload)
  assert broadcast_payload == %{game_id: open_game.id}

  assert Jason.encode!(broadcast_payload)
end
```

关于此测试需要注意的第一点是，在第 1 行，它没有接受来自 setup 的测试上下文 %{socket: socket}。我们的测试实际上不需要 socket，因为测试进程本身已订阅到 Channel。setup 返回 socket 是为了在我们想要为应用程序添加更多功能，从而添加更多测试时使其可用。

我们被测的函数从不访问数据库，因此我们传入的数据不需要存在于其中。在第 2 行，我们创建一个新游戏，传入对我们正在测试的函数重要的唯一覆盖，即将游戏的 current_phase 设置为 :joining。然后我们在第 4 行执行该函数，断言返回值是 :ok，这恰好是 NotSkullWeb.Endpoint.broadcast!/3 的返回值。被测代码只是将该响应传回测试代码。

被测函数的主要行为不是返回值，而是一个副作用。然而，与其他地方不同，我们需要测试双重来验证副作用是否发生，这里副作用是应该向测试进程的邮箱发送一条消息。如果有其他进程也订阅了同一个 Channel，它们也会收到广播。

在第 6 行，我们的测试调用 assert_broadcast/2。该函数接受参数为预期的事件名称 "new_game_created" 和一个模式，我们传入了一个未绑定的变量。这里通常会固定一个模式，但我们希望捕获整个负载以供后续断言。

我们在第 7 行确实断言负载是我们所期望的。我们测试的最后一个断言需要一点理解。正如你所回忆的，测试进程直接订阅到 Channel。我们的测试旨在关注客户端应用程序的交互，如果 JavaScript 客户端订阅了同一个 Channel。客户端的交互与我们的测试之间存在功能差异。当调用 socket/2 辅助函数时，它创建了一个 socket，跳过了真实的连接过程以提供便利。

如果你查看返回的 socket 中的数据，你会找到一个名为 :serializer 的键，其值为 Phoenix.ChannelTest.NoopSerializer。这个“无操作”序列化器不会以任何方式改变它发送到测试进程的值。如果我们的测试进程旨在代表在应用程序内部运行的另一个 Elixir 进程，这将很棒。然而，在我们的例子中，我们关注的是会发送给 JavaScript 客户端的内容。JavaScript 客户端的 socket 将具有不同的序列化器。在我们的应用程序中


在这种情况下，它将是 JSON 序列化库 Jason。测试工具不允许你覆盖此值，但我们在测试中多做一步可以帮助我们确保这种差异在尝试运行应用程序时不会引起问题。

在第 9 行，我们试图找到测试应用程序不会出现问题的最轻量级方法，只需确保事件负载*可以*被序列化。我们在序列化后的值处停止测试，因为我们应该对它们的外观有清晰的认识，而且我们不想测试 Jason 库本身。当处理不是固有可序列化为 JSON 的返回值时，这一步额外步骤非常相关，例如一个 Ecto schema 结构体。这将有助于确保你的 Channel 中的代码在 Phoenix Channel 框架尝试序列化数据之前处理必要的数据转换。

这两个最大的收获来自于此测试，应该是指 assert_broadcast 接受一个模式，并且你的负载不会被序列化。这些教训也适用于 assert_push/3 和 assert_reply/4，它们是我们没有涵盖的两个最常见的测试助手。如果记住测试过程本身已订阅到 Channel，并且始终使用 Phoenix.ChannelTest.NoopSerializer，那么它将有助于澄清测试 Phoenix Channels，以及为健壮的覆盖率建立基础。

## Wrapping Up

在本章中，我们介绍了测试控制器和 Phoenix Channels 作为我们基于 Phoenix 的 Web 应用程序的接口。控制器测试用于提供基于 JSON 的 API 端点以及服务器渲染的 HTML 端点的应用程序。Channel 测试涵盖了通过 WebSocket 的交互。你现在应该熟悉使用 Phoenix、Plug 和 ChannelTest 提供的最常见的测试助手函数。你还应该理解如何以及为什么要确保你的测试涵盖返回值和副作用。

在下一章中，我们将向你介绍基于属性的测试。我们将向你展示工具，这些工具将帮助你在代码行为最可能在其输入有很大变化的地方锁定其行为。

## Footnotes

- [37] https://github.com/ninenines/ranch
- [38] https://github.com/ninenines/cowboy
- [39] https://github.com/elixir-plug/plug
- [40] https://github.com/phoenixframework/phoenix
- [41] https://github.com/elixir-ecto/ecto
- [42] https://github.com/elixir-plug/plug
- [43] https://jwt.io/
- [44] https://hex.pm/packages/floki
- [45] https://www.npmjs.com/package/phoenix
- [46] https://github.com/phoenixframework/phoenix/blob/c8883af5582a38496e4b7e45e05d3a4d759a6caa/lib/phoenix/test/channel_test.ex#L269
