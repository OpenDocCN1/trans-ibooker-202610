

## 消息列表

在 MessagesList 小部件中，我们将获取消息列表并使用 ListView.builder 构造函数创建一个 ListView，将显示消息所需的数据（发送者、文本和时间戳）传递给稍后我们将构建的 Message 小部件。

ListView 的 reverse 参数使列表 *从底部开始*，意味着第一个元素将显示在底部，这将是滚动视图的默认位置，并且在添加新消息时会被保留。这是聊天应用的典型行为，我们希望复制它，因此我们将其设置为 `true`：

```
```dart
class MessagesList extends StatelessWidget {
  MessagesList(this.data);

  final QuerySnapshot data;

  bool areSameDay(Timestamp a, Timestamp b) {
    var date1 = a.toDate().toLocal();
    var date2 = b.toDate().toLocal();
    return
      (date1.year == date2.year)
      &&
      (date1.month == date2.month)
      &&
      (date1.day == date2.day);
  }

  @override
  Widget build(BuildContext context) =>
      ListView.builder(
        reverse: true,
        itemCount: data.documents.length,
        itemBuilder: (context, i) {
          var months = [
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
            "August",
            "September",
            "October",
            "November",
            "December"
          ];
          DateTime when = data
              .documents[i]
              .data["when"]
              .toDate()
              .toLocal();
          var widgetsToShow = <Widget>[
```

```
Message(
  from: data.documents[i].data["from"],
  msg: data.documents[i].data["msg"],
  when: when
);
if(i == data.documents.length-1) {
  widgetsToShow.insert(
    0,
    Padding(
      padding: const EdgeInsets.symmetric(vertical: 10.0),
      child: Text(
        "${when.day} ${months[when.month-1]} ${when.year}",
        style: Theme.of(context).textTheme.subhead,
      ),
    );
  } else if(
    !areSameDay(
      data.documents[i+1].data["when"],
      data.documents[i].data["when"]
    )
  ) {
    widgetsToShow.insert(
      0,
      Padding(
        padding: const EdgeInsets.symmetric(vertical: 10.0),
        child: Text(
          "${when.day} ${months[when.month-1]} ${when.year}",
          style: Theme.of(context).textTheme.subhead
        ),
      )
    );
  }
  return Column(
    children: widgetsToShow
  );
}
```

你可能已经注意到，我们使用允许我们显示消息发送日期的 Column 来构建 ListView，如果该日期与前一条消息的日期不同。这是许多聊天应用的首选方法，因为它只在对用户有意义时才显示日期。

一个缺点是这样做有点不直观且冗长：我们必须同时使用 if 和 else if，因为如果在第一个 if 中同时使用两个条件，当我们尝试访问索引 documents.length 的文档时，应用会崩溃，但情况更糟的是，列表是分两步创建的，这使得内容在初次查看时不够清晰。

```
```dart
?
Alignment.centerRight
:
Alignment.centerLeft,
child: Container(
  width: MediaQuery.of(context).size.width/3*2,
  child: Card(
    shape: StadiumBorder(),
    child: ListTile(
      title: user.displayName != null
          ? Padding(
              padding: const EdgeInsets.only(
                top: 8.0,
                left: 5.0
              ),
              child: Text(
                from,
                style: Theme.of(context).textTheme.subtitle
              ),
            )
          : Padding(
              padding: EdgeInsets.only(left: 5.0),
              child: Text(
                "You",
                style: Theme.of(context).textTheme.subtitle
              ),
            ),
      subtitle: Padding(
        padding: const EdgeInsets.only(
          bottom: 10.0,
          left: 5.0
        ),
        child: Text(
          msg,
          style: Theme.of(context).textTheme.body1
        ),
      ),
      trailing: Text("${when.hour}:${when.minute}"),
    ),
  ),
);
} else {
  return CircularProgressIndicator();
}
}
);
}
```

## 使用 Firebase 实现界面

上一节中对电子邮件/密码身份验证的强调 specifically 是为了让你为在我们的聊天应用中实现这种身份验证方法做好准备。

我们也将遵循本节的用户交互流程，并从为身份验证目的实现界面开始。

## 身份验证

在 *Writing Log-In and Sign-Up Methods* 中，我们看到了如何实现登录方法：

```
firebase/chatonfire/lib/main.dart

Future<FirebaseUser> logIn(
  String email, String password
) async =>
  _auth.signInWithEmailAndPassword(
    email: email,
    password: password
  );
```

以及注册方法：

```
firebase/chatonfire/lib/main.dart

Future<FirebaseUser> signUp(
  String email, String password
) async =>
  _auth.createUserWithEmailAndPassword(
    email: email,
    password: password
  );
```

我们缺少的是设置显示名称并重定向到聊天页面的方法，正如 ConfigPage 所需的那样。

这应该不会太难，因为我们已经在介绍 *The FirebaseUser* 时看到了如何更改数据，例如显示名称：

```
firebase/chatonfire/lib/main.dart

void setNameAndGoToChatPage(String name, BuildContext context) {
  FirebaseAuth.instance.currentUser().then(
    (user) {
      var newUserInfo = UserUpdateInfo();
      newUserInfo.displayName = name;
      user.updateProfile(newUserInfo);
    }
  );
  Navigator.pushReplacement(
    context,
    MaterialPageRoute(
      builder: (_) => ChatPage(),
    )
  );
}
```

## 数据库

发送消息是使用我们在 *Adding Data to the Cloud Firestore* 中谈到向 Cloud Firestore 添加文档时提到的 CollectionReference.add 方法完成的：

```
firebase/chatonfire/lib/main.dart
```

```
void sendText(String text) =>
  FirebaseAuth.instance.currentUser().then(
    (user) =>
      Firestore.instance.collection("Messages").add(
        {
          "from": user != null ? user.displayName : "Anonymous",
          "when": Timestamp.fromDate(DateTime.now().toUtc()),
          "msg": text,
        }
      )
  );
```

在章节开头讨论的用于从 Cloud Firestore 读取数据的几种方法中 (*Reading from the Cloud Firestore*)，我们需要获取一个 Stream 以便与 StreamBuilder 一起使用，因此我们使用 CollectionReference.snapshots：

```
firebase/chatonfire/lib/main.dart
```

```
Stream<QuerySnapshot> getMessages() =>
  Firestore
    .instance
    .collection("Messages")
    .orderBy("when", descending: true)
    .snapshots();
```

## 添加个人资料页面：在 Cloud Firestore 中存储用户数据

我们的应用存在一些问题：如果两个用户具有相同的显示名称会造成混淆，因为我们只在数据库中存储用户的显示名称，而没有存储其 ID，因此我们的应用会认为用户是消息的发送者，即使发送消息的用户实际上是另一个恰好具有相同显示名称的完全不同的用户。

此外，应用实际上并没有提供区分这两个用户的方法，这对每个人来说都是一个令人困惑的情况。一个好的开始是为用户提供设置一个短字符串来自定义其个人资料的方式（通常称为状态或简介），以及通过电子邮件联系聊天应用的另一位用户的方式，因为我们已经要求提供该信息。

## 为用户数据创建集合

让我们再创建一个集合！我们将它命名为 *Users*，其结构如下：

- 用户的 uid 将作为文档的 ID，以便更轻松地访问每个用户对应的文档。
- 用户的 displayName 将作为名为 displayName 的字符串。
- 用户的电子邮件地址将作为名为 email 的另一个字符串。
- 用户的简介将作为名为 bio 的字符串存储。

## 更改文档和应用

我们需要为我们应用的每个现有用户创建用户数据集合的条目，并更改消息集合中的每个条目，将显示名称替换为用户 ID。然后，我们将担心使应用与数据库中更改后的消息一起工作，使其显示从 Firestore 获取的显示名称，然后我们将着手构建个人资料页面。

## 更改数据

创建用户集合必须手动完成，通常最好有一个包含用户数据的集合：这在大多数“传统”数据库中就是这样，而且在 Firebase 中复制它是有价值的，除了我们不需要（也不应该，尤其是不以明文形式）将密码存储在我们可以访问的数据库中。

让我们从处理更改数据开始：当消息不多时，更改数据库中每条消息并替换值很容易，因为我们可以在 Firebase 控制台中手动更改它们，但对于更大的数据库，你必须编写一些代码（使用本章中我们学到的接口）来获取消息，找到具有给定显示名称的用户并用用户 ID 替换显示名称。

> **实现平滑过渡**
> 
> 在初始开发期间（在我们的例子中），应用没有用户，因此我们可以对数据库进行更改，这些更改会使应用的先前版本停止

