

## StreamBuilder

StreamBuilder 的工作原理与 FutureBuilder 相似，但它会实时更新其内容以匹配给定数据流的内容，而不是像 FutureBuilder 那样只显示给定 Future 的结果：

```dart
StreamBuilder(
  stream: myStream(),
  builder: (context, snapshot) {
    if(!snapshot.hasData) return CircularProgressIndicator();
    return MyWidget(
      data: snapshot.data
    );
  }
)
```

## 构建聊天应用

既然我们已经了解了如何将 Firebase 用于所需功能（身份验证和数据库），并且掌握了一些更高级的 UI 概念，我们就可以开始构建我承诺过的聊天应用了。

### 规划应用界面

聊天应用需要用户在首次启动应用时执行多项操作：

-   我们需要用户输入电子邮件地址和密码，然后登录现有账户；如果还没有账户，则创建一个新账户。此时，用户还应收到一封要求其验证账户的电子邮件，但我们不会在已验证和未验证用户之间做出区分。
-   我们需要用户选择一个*显示名称*，以便在应用中向其他用户标识自己。
-   如果用户完成了以上所有步骤，他们就应该进入显示其他用户发送的所有消息的页面。

### 构建界面

我们将按照用户与界面交互的顺序逐个构建每个部分。让我们从注册/登录页面开始！

#### 登录/注册页面

和上一章一样，我们将使用 `TextEditingController`，以便在其自身的 `onSubmitted` 之外访问输入到 `TextField` 中的文本。

界面将如下所示（我们将在后面的“身份验证”部分实现 `logIn` 和 `signUp` 方法）：

```dart
// firebase/chatonfire/lib/main.dart

@override
Widget build(BuildContext context) {
  if(!_verificationComplete) {
    Navigator.pushReplacement(
      context,
      MaterialPageRoute(
        builder: (context) => ConfigPage()
      )
    );
  }
  return Scaffold(
    appBar: AppBar(
      title: Text("ChatOnFire Login"),
    ),
    body: ListView(
      children: <Widget>[
        Padding(
          padding: EdgeInsets.all(15.0),
          child: Text(
            "Log In Using Your Phone Number",
            style: Theme.of(context).textTheme.display1,
          ),
        ),
        Padding(
          padding: EdgeInsets.all(10.0),
          child: TextField(
            keyboardType: TextInputType.emailAddress,
            controller: _emailController,
            decoration: InputDecoration(
              labelText: "Email Address",
            ),
            autofocus: true,
          )
        ),
        Padding(
          padding: EdgeInsets.all(10.0),
          child: TextField(
            keyboardType: TextInputType.text,
            obscureText: true,
            controller: _passwordController,
            decoration: InputDecoration(
              labelText: "Password",
            ),
          ),
        ),
        Padding(
          padding: EdgeInsets.all(10.0),
          child: FlatButton(
            color: Theme.of(context).accentColor,
            textColor: Colors.white,
            child: Text("Log In".toUpperCase()),
            onPressed: () {
              logIn(
                _emailController.text,
                _passwordController.text
              ).then(
                (user) {
                  _user = user;
                  if (!_user.isEmailVerified) {
                    _user.sendEmailVerification();
                  }
                  _verificationComplete = true;
                  Navigator.pushReplacement(
                    context, MaterialPageRoute(
                      builder: (context) => ConfigPage()
                    )
                  );
                }
              ).catchError(
                (e) {
                  Scaffold.of(context).showSnackBar(
                    SnackBar(
                      content: Text(
                        "You don't have an account. Please sign up."
                      )
                    )
                  )
                }
              )
            }
          )
        )
      ]
    )
  );
}
```

登录后，用户必须输入显示名称，因此我们需要一个专门用于实现该目的的名为 `ConfigPage` 的屏幕，因为这一步需要先完成身份验证，并且是使用聊天页面的前提条件：

```dart
// firebase/chatonfire/lib/main.dart

@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(title: Text("Configure you account's basic information")),
    body: ListView(
      children: <Widget>[
        TextField(decoration: InputDecoration(
          labelText: "Display Name"
        ),
        onSubmitted: (displayName) =>
          setNameAndGoToChatPage(displayName, context),
        controller: _controller,
        ),
        FlatButton(
          child: Text("Submit"),
          onPressed: () =>
            setNameAndGoToChatPage(_controller.text, context),
        )
      ],
    );
  }
}
```

同样，我们稍后会实现 `setNameAndGoToChatPage`。我们只需要知道它一定会推入一个新屏幕，加载一个名为 `ChatPage` 的组件，我们现在就来实现它。

### 聊天页面

以下是我们将在本节中构建的内容：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_332_0.png)

`ChatPage` 将由一个 `Column` 组成，该 `Column` 显示一个用 `Expanded` 包裹的 `ListView`（使用传递给 `StreamBuilder` 的消息构建），以及一个 `Row`（显示在屏幕底部，因为 `ListView` 会被 `Expanded` 组件扩展），其中包含一个 `TextField` 和一个用于发送消息的 `IconButton`，并一如既往地使用 `TextEditingController`：

```dart
// firebase/chatonfire/lib/main.dart

@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text("ChatOnFire"),
    ),
    body: Column(
      children: <Widget>[
        Expanded(
          child: StreamBuilder(
            stream: getMessages(),
            builder: (context, snapshot) =>
              snapshot.hasData ?
                MessagesList(snapshot.data as QuerySnapshot)
                :
                Center(child: CircularProgressIndicator())
          ),
        ),
        Row(
          children: <Widget>[
            Expanded(
              child: Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextField(
                  controller: _messageController,
                  keyboardType: TextInputType.text,
                  onSubmitted: (txt) {
                    sendText(txt);
                    _messageController.clear();
                  }
                ),
              ),
            ),
            IconButton(
              icon: Icon(Icons.send),
              onPressed: () {
                sendText(_messageController.text);
                _messageController.clear();
              }
            )
          ],
        )
      ],
    ),
  );
}
```

和往常一样，`getMessages` 和 `MessagesList` 将一如既往地在后面实现与 Cloud Firestore 的接口时定义，见 *数据库* 部分。

