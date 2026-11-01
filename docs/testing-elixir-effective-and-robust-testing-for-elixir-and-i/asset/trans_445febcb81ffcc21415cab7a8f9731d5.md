
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
