
# 访问令牌

请注意，`firstname` 和 `lastname` 是可选的。它们并不总是存在于 `User` 模型中，这里也不必强制包含它们。

为了存储验证 `JWT` 所需的信息，我们还需要添加令牌的过期时间以及创建时间。

### 请注意这三个变量

```swift
let status: Int = 0
let exp: TimeInterval
let iat: TimeInterval
```

这三个属性包含验证 `JWT` 所需的信息。

### 为了验证，payload 具有以下函数

```swift
func verify(using signer: JWTSigner) throws {
    let expiration = Date(timeIntervalSince1970: self.exp)
    try ExpirationClaim(value: expiration).verifyNotExpired()
}
```

在这种情况下验证非常简单：如果过期日期（`exp`）不在过去，则令牌有效。你也可以添加进一步的验证声明，并根据其他标准检查令牌。

为了签发令牌，我们需要一个构造器，它使用来自 `User` 类的正确信息并设置正确的内部值。

### 查看类的以下构造器

```swift
init(id: Int, email: String) {
    self.id = id
    self.email = email
    self.firstname = nil
    self.lastname = nil
    self.exp = String(Date().addingTimeInterval(60*60*24).timeIntervalSince1970)
    self.iat = String(Date().timeIntervalSince1970)
}
```

我们使用当前时间（`Date()`）并由此构建 `exp` 和 `iat` 变量。其余的值只是从用户那里复制过来的。请注意，我们省略了用户配置文件中的信息（最重要的是 `password`）。从技术上讲，你可以将整个 `User` 模型用作 payload，但由于敏感信息（如 `password`）的存在，你绝不应该这样做。

`iat` 变量在我们的服务中完全未被使用。不过，客户端可能需要它来验证令牌何时即将过期。

好的。你现在已经看到了访问令牌所需的一切。现在让我们看看刷新令牌。

# 刷新令牌

刷新令牌与访问令牌非常相似。我们不需要任何特定于用户的信息，除了 `ID`。原因是刷新令牌仅用于获取新的访问令牌。刷新令牌永远不会发送给除用户服务之外的任何服务。刷新令牌的过期时间比访问令牌长，因此默认过期时间会更长。

在 `Sources/App/Models` 中创建一个 `RefreshToken.swift` 文件并插入以下内容：

```swift
import Vapor
import JWT

struct RefreshToken: JWTPayload {
    let id: Int
    let iat: TimeInterval
    let exp: TimeInterval
    
    init(user: User, expiration: TimeInterval = 24 * 60 * 60 * 30) {
        let now = Date().timeIntervalSince1970
        
        self.id = user.id ?? 0
        self.iat = now
        self.exp = now + expiration
    }
    
    func verify(using signer: JWTSigner) throws {
        let expiration = Date(timeIntervalSince1970: self.exp)
        try ExpirationClaim(value: expiration).verifyNotExpired()
    }
}
```

此代码包含用于签发 `RefreshToken` 的 `struct`。`RefreshTokens` 允许用户在无需重新提交密码或电子邮件的情况下刷新其 `AccessToken`。

请注意，`RefreshToken` struct 符合 `JWTPayload`——就像之前展示的 payload 一样。

`verify` 函数与访问令牌的相同，`iat` 和 `exp` 的功能也保持不变。

# 响应模型

正如前面章节所讨论的，你几乎总是会有 I/O 模型。提醒一下，这些模型代表数据库模型的数据，但仅包含特定用例所需的数据。

由于这些模型本身较小，我们可以将它们全部放在同一个文件中。如果你想创建单独的文件，也可以。所有 I/O 模型都符合 `Content` 协议，以便它们可以作为 JSON 返回。为了将它们标记为响应模型，我们将它们命名为 `NameOfClassResponse`。

请执行以下步骤：

1. 在 `Sources/App/Models/Responses/` 中创建一个 `UserResponse.swift` 文件并插入第一个模型，如下所示：

```swift
import Vapor

struct UserResponse: Content {
    let id: Int?
    let firstname, lastname: String?
    let email: String
    let addresses: [AddressResponse]?
}
```

`UserResponse` 模型用于每当我们想要返回一个 `User` 时。因此，它包含我们希望在返回用户时看到的信息。由于此服务仅返回已通过身份验证的用户，我们可以包含电子邮件地址等信息。如果我们想返回公共用户列表，我们将创建另一个仅包含姓氏首字母或类似信息的模型。

请注意，我们在这里引用的是 `AddressResponse` 而不是 `Address`。原因是我们想使用 `AddressResponse` 而不是 `Address`，以正确地仅使用响应模型。

`Content` struct 或 class 的所有属性也必须符合 `Content` 协议。

返回用户时，我们可能希望包含一个状态，指示请求成功。

2. 继续创建一个新的 `UserSuccessResponse.swift` 文件，内容如下：

```swift
import Vapor

struct UserSuccessResponse: Content {
    let status: String = "success"
    let user: UserResponse
}
```

请注意，此 struct 引用了我们的 `UserResponse` struct。

我们还需要在用户登录时返回访问令牌和刷新令牌。

3. 在另一个名为 `LoginResponse.swift` 的文件中输入以下 struct：

```swift
import Vapor

struct LoginResponse: Content {
    let status = "success"
    let accessToken: String
    let refreshToken: String
    let user: UserResponse
}
```

最后，每当返回新的访问令牌时，我们都需要返回它。

4. 然后我们需要地址响应。创建一个 `AddressResponse` 文件并输入以下内容：

```swift
import Vapor

final class AddressResponse: Content {
    var id: Int?
    var street: String
    var city: String
    var zip: String
    var userId: Int
    var createdAt: Date?
    var updatedAt: Date?
    var deletedAt: Date?
}
```

我们在这里使用了 `Address` 的所有属性，因为我们可能需要它们。

5. 添加最后一个文件 `RefreshTokenResponse.swift`，内容如下：

```swift
import Vapor

struct RefreshTokenResponse: Content {
    let status = "success"
    let accessToken: String
}
```

所有 `status` 属性都已定义为 `"success"`。原因是如果发生错误，我们不会返回这些 struct，所以它们——根据定义——只会在成功时返回。

定义完响应模型后，让我们开始定义它们的对应物：输入模型！

# 输入模型

输入模型将接收处理请求所需的所有信息。我们只需要四个模型：

- `AddressInput`：传递地址信息时。
- `NewUserInput`：注册新用户时。
- `EditUserInput`：编辑现有用户时。
- `LoginInput`：登录时。

请执行以下步骤以创建模型：

1. 在 `Sources/App/Models/Inputs/` 中创建一个 `AddressInput.swift` 文件并插入第一个模型，如下所示：

```swift
import Vapor

struct AddressInput: Content {
    let street: String
    let city: String
    let zip: String
}
```

该模型仅反映我们从模型中需要的属性。它符合 `Content` 协议。

2. 创建一个新的 `NewUserInput.swift` 文件，如下所示：

```swift
import Vapor

struct NewUserInput: Content {
    let firstname: String?
    let lastname: String?
    let email: String
    let password: String
}
```

同样，我们只获取 `firstname`、`lastname`、`email` 和 `password` 属性。注册新用户只需要这些。

3. 创建一个 `RefreshTokenInput.swift` 文件，内容如下：
