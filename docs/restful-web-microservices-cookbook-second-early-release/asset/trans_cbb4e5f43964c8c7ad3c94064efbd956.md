
在服务 API 的实现阶段，最好记录你用来将内部模型转换为外部消息的规则。这将有助于与其他开发人员共享算法，并更轻松地调试你在此过程中发现的任何错误。然而，将这种表示描述发布给 API 消费者并不是一个好主意。他们不需要知道你接口背后的内部数据或对象模型。

随着时间推移，你可能会遇到服务内部模型发生变化的情况。在许多情况下，这些变化**不应该**导致外部消息发生变化。例如，当内部模型属性 `user.id` 更改为 `user.identifier` 时，没有必要更改外部属性。

然而，一些内部模型更改会包含新属性（`user.hatsize`），如果这些更改不会导致破坏现有 API 消费者，那么这些新属性可能会反映到外部消息中。在 `text/csv` 行的末尾添加一个字段可能没问题，但重新排列行中的字段顺序（例如，将新字段插入到行的第一个字段）很可能是个坏主意。

> **提示**
> 
> 如果你预计内部模型会频繁更改，请务必选择一种旨在减少此类更改影响的表示格式。例如，Collection+JSON 适用于多变的模型（所有属性都表示为 `data.name` 和 `data.value`），但 SIREN 和 HAL（因为它们直接绑定到对象属性）则不适用。

虽然你可以“硬编码”服务接口以**始终输出**相同的格式（HAL、SIREN 等），但更好的做法是遵循 HTTP 内容协商过程，允许 API 消费者在运行时使用 `Accept` 请求头表明其格式偏好。有关如何处理客户端媒体类型偏好的详细信息，请参见技巧 4.5。

在服务 API 中支持多种表示格式是一个好主意。我通常至少支持两种，并根据情况尽量支持三种或更多。首先，最好选择一种 Hypermedia Factor²⁵ 得分高的媒体类型。有关 H-Factors 的更多信息，请参见**技巧 3.9**。例如，Collection+JSON 或 SIREN。其次，最好支持一种简单的序列化格式，如 JSON、XML 和/或 CSV，因为许多 API 消费者能够快速使用这些简单格式。如果你想支持其他格式，可以添加任何其他你或你的 API 消费者喜欢使用的格式。

如果合适，为你的服务 API 支持 HTML 表示也会很有帮助。它可以在服务开发人员测试接口设计时提供帮助。它还可以帮助那些想要“不投入大量时间编写 API 客户端就能试用 API”的 API 消费者。最后，HTML 格式可以作为服务集合中高 H-Factor 媒体类型之一的一个可靠选择。

## 相关

- 第 2 章
- 第 3 章
- 技巧 3.6
- 技巧 3.9
- 第 4 章
- 技巧 4.5
- 技巧 4.17
- [链接待添加]

## 4.4 将内部函数表达为外部动作

一旦你采用了使用注册媒体类型标准化响应的做法，你还需要应对如何正确表达接口背后服务内部功能的挑战。要做到这一点，你需要依赖一种不仅表达服务数据模型，还表达该服务动作模型——即需要通过外部 API 访问的服务功能——的做法。

### 问题

如何决定服务中的哪些内部函数需要通过 API 暴露？以及如何以一致的方式表达这些方法，以便 API 消费者能够按预期找到并操作它们？有哪些格式和/或模式可用于将内部服务函数转换为外部接口动作？

### 解决方案

将服务内部函数作为外部动作暴露的最直接方式是使用超媒体控件来表示这些函数（参见技巧 3.9）。例如，在 HTML 中，函数名称（`<form name="approveUser" ...>`）、输入参数（`<input>` 控件）和输出返回值可以用来表示内部服务函数。然而，将外部动作直接绑定到内部服务操作并不总是可行的（甚至不是一个好主意）。

在表达内部服务函数时，通常最好依赖既定的外部词汇表。例如，内部数据参数可能命名为 `firstName`。但同样的参数在外部接口中会被表达为 `givenName`（参见“依赖语义词汇表”）。API 代码可以处理内部和外部名称之间的转换。这样做的好处是，外部接口可以免受服务内部变更的影响。再进一步举例，服务更新可能导致 `firstName` 变为 `fNameValue`。在这种情况下，只需更新中间件代码，而无需更新实际的外部接口。

最后，一些内部动作——尤其是涉及复杂计算或过程的动作——最好以更简化的方式表达。一个服务（`onboardingSvc`）可能需要联系多个依赖服务（`userSvc`、`customerRelationsSvc` 和 `accountSalesSvc`），并在它们之间传递数据以完成任务。通常最好在外部将此表达为单个动作（`sendOnboardingData`），而不是多个依赖动作（详见下面的示例）。

本质上，将 API 流程交互的设计视为一项独立活动。你可以尽情发挥创造力和想象力。设计一个好的接口；不要只照搬服务内部实现。要假设在 API 交互和内部服务创新之间可能存在一个“转换”步骤。最重要的是，努力保持底层服务与你发布的服务接口之间的松散耦合。

### 示例

在从内部函数到外部动作的直接转换中，你可以从一个服务函数的示例开始：

```javascript
function approveUser(userId, nick, approver, level) {
  var approval = {};

  approval.userId = userId;
  approval.nick = nick;
  approval.approver = approver;
  approval.level = level;

  return data.create(approval);
}
```

然后将这些内部元素转换为响应消息中的 HTML FORM 对象：

```html
<form name="approveUser" enctype="application/x-www-form-urlencoded"
  method="post" action="http://api.example.org/users/approvals"
  target="approvalDisplay">
  <input name="userNick" type="string" value="mork" />
  <input name="approverName" type="string" value="Mr. Roboto" />
  <input name="approveLevel" type="string" value="nominal" />
  <input type="submit" value="Approve User" />
</form>
<iframe id="approvalDisplay" />
```

然而，正如我们上面已经讨论过的，直接转换通常不是一个好主意。这里有一个示例，它依赖一个众所周知的外部词汇表（本例中来自 Schema.org 库的值）来使用标准化的词汇表表达外部动作，即使底层服务并不支持该词汇表。

考虑这个用于更新用户对象的简单内部方法：

```javascript
function updateUser(id, fname, lname, email) {
  ...
  return userObject
}
```

现在，让我们看看接口动作（表示为 Collection+JSON 模板）：

```json
{
  "collection": {
    "template": {
      "data": [
        {"name": "identifier", "value": "q1w2e3r4", "prompt": "User ID"},
        {"name": "givenName", "value": "Mark", "prompt": "First Name"},
        {"name": "familyName", "value": "Morkelsen", "prompt": "Last Name"},
        {"name": "email", "value": "mork@example.org", "prompt": "Email"}
      ]
    }
  }
}
```

最后，接受更新动作的代码可以处理内部和外部名称之间的转换：

```javascript
function updateAction(identifier, givenName, familyName, email) {
  var user = data.read(identifier);
```

