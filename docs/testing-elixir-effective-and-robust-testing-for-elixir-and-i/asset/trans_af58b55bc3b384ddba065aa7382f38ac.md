
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
