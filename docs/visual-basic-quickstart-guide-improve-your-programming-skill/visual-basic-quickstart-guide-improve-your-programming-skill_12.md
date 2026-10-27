## The Request and Response Model

In **Classic ASP** and **ASP.NET**, the Request and Response model describes how client-server communication is handled. Both Classic ASP and ASP.NET are web application frameworks that allow you to build dynamic web applications using server-side technologies. The Request and Response model is at the core of this communication process.

The following is an explanation of the Request part of the model:

- The request represents the information sent by the client (usually a web browser) to the server. It includes **HTTP** headers, form data, query parameters, cookies, and more.
- When a user accesses a web page or sends a form submission, the client generates an HTTP request and sends it to the server.
- The server-side code in your application processes this request and responds accordingly.
- Classic ASP and ASP.NET provide various objects and properties to access the request data.

The following is an explanation of the Response part of the model:

- The response represents the information sent by the server back to the client in response to the client’s request.
- The server-side code in your Classic ASP and ASP.NET applications generates the appropriate response, which is then returned to the client.
- The response typically includes **HTML** content, **CSS** styles, **JavaScript**, and other resources necessary for rendering a web page.
- Classic ASP and ASP.NET provide objects and methods to manage the response.

Classic ASP and ASP.NET use a combination of `Request` and `Response` objects to process client requests, execute server-side logic, and generate dynamic HTML or other content to be sent back to the client. This model enables the creation of interactive and dynamic web applications.

In this chapter, we’re going to cover the following main topics:

- Classic ASP `Request` object
- ASP.NET `Request` object
- Classic ASP `Response` object
- ASP.NET `Response` object

### Classic ASP Request object

In Classic ASP, the **Request** object is a built-in server-side object that’s used to retrieve information sent by the client (usually a web browser) to the web server. The **Request** object allows you to access various data types, including form data, query parameters, cookies, and more, making it essential for processing client requests and building dynamic web applications.

Here are some common properties and methods of the **Request** object in Classic ASP:

- **Form collection (`Request.Form`)**: Provides access to the data sent to the server using the **HTTP POST** method (usually from HTML forms).
  
  Example: `Request.Form("username")` returns the value submitted with the input field named **“username”**。
- **Query string (`Request.QueryString`)**: Provides access to the data sent to the server using the **HTTP GET** method (usually from URLs).
  
  Example: `Request.QueryString("id")` returns the value of the **“id”** parameter from the URL.
- **Cookies collection (`Request.Cookies`)**: Provides access to the client’s cookies sent with the request.
  
  Example: `Request.Cookies("username").Value` returns the value of the **“username”** cookie.
- **ServerVariables collection (`Request.ServerVariables`)**: Provides access to server-related information, such as server name, server port, client IP address, and more.
  
  Example: `Request.ServerVariables("REMOTE_ADDR")` returns the client’s IP address.
- **HTTP method (`Request.ServerVariables("REQUEST_METHOD")`)**: Returns the HTTP method used in the request (for example, GET or POST).
- **Path (`Request.ServerVariables("PATH_INFO")`)**: Returns the virtual path of the requested ASP file. When used for web development, this information comes from the URL.
- **Query string (`Request.ServerVariables("QUERY_STRING")`)**: Returns the raw query string from the URL.

You don’t need to create the **Request** object in Classic ASP to use it explicitly; it’s automatically available in ASP pages. You can access its properties and methods directly within your ASP code.

Here’s a simple example that demonstrates the use of the **Request** object in Classic ASP:

```asp
<%
Dim username
username = Request.Form("username")
If Len(username) > 0 Then
    Response.Write "Hello, " & username & "!"
Else
%>
    <form method="post">
<label for="username">Enter your name:</label>
<input type="text" name="username" id="username">
<input type="submit" value="Submit">
</form>
<%
End If
%>
```

In this example, when the user submits the form with their name, Request.Form("username") retrieves the value from the form data, and the personalized greeting is displayed using Response.Write. If no name is provided, the form is displayed for the user to input their name.

Next, we will examine the ASP.NET Request object.

### ASP.NET Request object

In ASP.NET, the Request object is a server-side object that’s used to retrieve information sent by the client (usually a web browser) to the web server. It provides access to various data types, including form data, query parameters, cookies, headers, and so on. The Request object is essential to processing client requests and building dynamic web applications in ASP.NET.

The Request object is accessible in all ASP.NET page types, including ASPX pages, code-behind files, and custom handlers. It is available by default, and you can use it directly without creating an instance.

Here are some common properties and methods of the Request object in ASP.NET. They are very close to the Classic ASP properties and methods:

- **Form collection (Request.Form)**: Provides access to the data sent to the server using the **HTTP POST** method (usually from HTML forms).
  
  Example: Request.Form["username"] returns the value submitted with the input field named "username".
- **Query string (Request.QueryString)**: Provides access to the data sent to the server using the **HTTP GET** method (usually from URLs).
  
  Example: Request.QueryString["id"] returns the value of the "id" parameter from the URL.
- **Cookies collection (Request.Cookies)**: Provides access to the client’s cookies sent with the request.
  
  Example: Request.Cookies["username"].Value returns the value of the "username" cookie.
- **Headers collection (Request.Headers)**: Provides access to the HTTP headers sent by the client.
  
  Example: Request.Headers["User-Agent"] returns the User-Agent header of the client’s browser.
- **HTTP method (`Request.HttpMethod`)**: Returns the HTTP method used in the request (for example, `GET`, `POST`).
- **Path (`Request.Path`)**: Returns the virtual path of the requested ASP.NET page.
- **QueryString (`Request.QueryString.ToString()`)**: Returns the raw query string from the URL.
- **InputStream (`Request.InputStream`)**: Provides access to the raw request body as a stream, which helps handle non-form data requests.

To use the **Request** object in ASP.NET, you can directly access its properties and methods within your ASPX pages or code-behind files.

Here’s a simple example that demonstrates the use of the **Request** object in an ASP.NET page:

```aspx
<%@ Page Language="VB" AutoEventWireup="false"
CodeBehind="default.aspx.vb" Inherits="WebApplication.Default" %>
<!DOCTYPE html>
<html>
<head>
    <title>ASP.NET Request Object Example</title>
</head>
<body>
    <form id="form1" runat="server">
        <div>
            <asp:Label ID="lblMessage"
                runat="server"></asp:Label>
            <asp:TextBox ID="txtUsername"
                runat="server"></asp:TextBox>
            <asp:Button ID="btnSubmit" runat="server"
                Text="Submit" OnClick="btnSubmit_Click" />
        </div>
    </form>
</body>
</html>
```

Here’s the **VB.NET** code-behind (`Default.aspx.vb`):

```vbnet
Imports System
Namespace WebApplication
    Public Class _Default
        Inherits System.Web.UI.Page
        Protected Sub btnSubmit_Click(ByVal sender As
            Object, ByVal e As EventArgs)
            Dim username As String = Request.Form
                ("txtUsername")
            If Not String.IsNullOrEmpty(username) Then
                lblMessage.Text = "Hello, " & username &
                    "!"
            End If
        End Sub
    End Class
End Namespace
```

In the previous example, the ASPX page contains a simple form with a text box and a submit button. When the user enters their name in the text box and clicks the **Submit** button, the `btnSubmit_Click` event handler in the VB.NET code-behind is executed – `Request.Form("txtUsername")` retrieves the value from the form data (note that we are using the `txtUsername` control ID to access the value). The


如果提供了姓名，网页上会通过 `lblMessage` 显示个性化问候语。

在 VB.NET 代码隐藏文件中，`Request` 对象会自动可用，你可以访问其属性（例如 `Request.Form`、`Request.Cookies` 等）来处理客户端数据并构建动态的 ASP.NET 应用程序。

接下来，我们将研究 Classic ASP 的 `Response` 对象。

## Classic ASP Response 对象

在 Classic ASP 中，`Response` 对象是一个服务器端对象，用于将服务器输出作为对客户端请求的响应发送回客户端（通常是 Web 浏览器）。`Response` 对象提供了方法和属性，用于设置 HTTP 头、发送内容（如 HTML、文本或二进制数据）、管理 Cookie，以及执行与生成响应相关的各种其他操作。

`Response` 对象在 Classic ASP 中自动可用，无需创建实例。你可以直接在 ASP 代码中使用它来自定义发送给客户端的响应。

以下是 Classic ASP 中 `Response` 对象的一些标准方法和属性：

- `Write` 和 `WriteText`：`Write` 方法向客户端发送输出，例如 HTML 内容、文本或变量值。`WriteText` 方法向客户端发送二进制数据（例如图像）。
- `Redirect`：`Redirect` 方法将客户端重定向到不同的 URL。
- `AddHeader`：`AddHeader` 方法向响应添加自定义 HTTP 头。
- `ContentType`：`ContentType` 属性设置响应内容的 MIME 类型。
- `Cookies` 集合（`Response.Cookies`）：管理发送回客户端的 Cookie。
- `Clear`：`Clear` 方法在发送响应之前清除任何缓冲的 HTML 内容。
- `BinaryWrite`：`BinaryWrite` 方法直接将二进制数据（例如图像或文件）发送到客户端。

下面是一个简单示例，演示了 Classic ASP 中 `Response` 对象的使用：

```
<%
Dim username
username = Request.Form("username")
If Len(username) > 0 Then
    Response.Write "Hello, " & username & "!"
Else
%>
    <form method="post">
        <label for="username">Enter your name:</label>
        <input type="text" name="username" id="username">
        <input type="submit" value="Submit">
    </form>
<%
End If
%>
```

在上面的示例中，当用户提交包含姓名的表单时，**Response.Write** 方法会向客户端发送个性化的问候语。如果未提供姓名，则会向用户发送一个 HTML 表单，以便他们输入姓名。

**Response** 对象是在 Classic ASP 中控制发送给客户端输出的重要工具。**Response** 对象使开发人员能够构建动态且交互式的 Web 应用程序。

接下来，我们将研究 **ASP.NET Response** 对象。

## ASP.NET Response 对象

在 ASP.NET 中，**Response** 对象是一个服务器端对象，用于将服务器输出作为对客户端请求的响应发送回客户端（通常是 Web 浏览器）。**Response** 对象提供了方法和属性，用于设置 HTTP 头、发送内容（如 HTML、文本或二进制数据）、管理 Cookie，以及执行与生成响应相关的各种其他操作。

**Response** 对象在所有 ASP.NET 页面类型中自动可用，包括 ASPX 页面、代码隐藏文件和自定义处理程序。你可以直接在 ASP.NET 代码中使用它来自定义发送给客户端的响应。

以下是 ASP.NET 中 **Response** 对象的一些标准方法和属性。同样，这些与 Classic ASP 的 **Response** 方法和属性非常相似：

- **Write**：与 Classic ASP **Response** 对象一样，**Write** 方法向客户端发送输出，例如 HTML 内容、文本或变量值。
- **WriteFile**：**WriteFile** 方法直接将二进制数据（例如图像或文件）发送到客户端。
- **Redirect**：**Redirect** 方法将客户端重定向到不同的 URL。
- **AddHeader**：**AddHeader** 方法向响应添加自定义 HTTP 头。
- **ContentType**：**ContentType** 属性用于设置响应内容的 MIME 类型。
- **Cookies 集合（Response.Cookies）**：管理发送回客户端的 Cookie。
- **Clear**：**Clear** 方法在发送响应之前清除任何缓冲的 HTML 内容。
- **BinaryWrite**：**BinaryWrite** 方法直接将二进制数据（例如图像或文件）发送到客户端。
- **输出缓存**：**Response.Cache** 属性允许你为响应配置缓存选项，通过向后续请求提供缓存内容来实现更好的性能。

下面是一个简单示例，演示了 **ASP.NET** 页面中 **Response** 对象的使用。
