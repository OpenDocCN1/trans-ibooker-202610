

> 如果你不熟悉扩展方法，扩展方法类必须是静态的，你正在扩展的项由参数前的 this 关键字指示。

## Button_Clicked

最后，让我们更改事件处理程序，以正确处理更新后的方法：

```
private async void Button_Clicked(object sender, EventArgs e)
{
    var image = await TakePicture();
    DisplayedImage.Source = ImageSource.FromStream(() =>
    image.GetStream());
    var emotionData = await GetEmotionAnalysis(image.GetStream());
    DisplayData(emotionData);
}
```

正如你所见，我们需要将处理程序设为 async，因为我们正在调用异步方法。
我们通过从 TakePicture 的结果中提取流来设置图像源。
最后，我们将从 GetEmotionAnalysis 返回的面孔列表传递给 DisplayData 方法。

我们现在有了一个可运行的应用程序：

![](img/762492a8287f369cc1ac4621a212f193_436_0.png)

如果你按下按钮，你会看到模拟器会通知你正在使用相机，但实际上并未提供对我们的应用有意义的图像。我们现在需要讨论如何在实体设备上测试此应用。

## 在实体设备上测试和运行

由于本程序的基础是使用相机，因此在实体设备上测试应用至关重要。

此外，出乎意料的是，在实体设备上测试实际上可能比在模拟器上测试更快。

在本节中，我们将介绍在 Android 设备上设置此应用所需的基础知识；但是，我强烈建议你参阅 Xamarin 官方关于此主题的指南：https://docs.microsoft.com/en-gb/xamarin/android/get-started/installation/set-up-device-for-development。

该指南更加全面，并且由于经常更新（而本书则未更新），因此是最新的指南。

此处的说明涉及 Android 8.x (Oreo)，但也应适用于更高版本（撰写本书时，最新版本为 9.x，即 Pie）。

> 以下屏幕截图来自模拟器；但是，已在实体设备上核对过，以确保其针对此 Android 版本的准确性。

为了配置你的手机：

- 1. 第一步是配置你的手机；首先加载设置屏幕：

![](img/762492a8287f369cc1ac4621a212f193_438_0.png)

- 2. 在“系统”中，你应当能看到一个名为“关于手机”或类似的菜单选项：

![](img/762492a8287f369cc1ac4621a212f193_439_0.png)

- 3. 找到“编译号”：

![](img/762492a8287f369cc1ac4621a212f193_439_1.png)

- 4. 你需要轻点七次。你将看到以下消息（别眨眼，否则会错过！）：

![](img/762492a8287f369cc1ac4621a212f193_440_0.png)

- 5. 在**系统**菜单中将出现一个新菜单，即 **开发者选项**：

![](img/762492a8287f369cc1ac4621a212f193_441_0.png)

- 6. 此新菜单允许你确定测试软件时有用的各种设置；但是，有一个特定设置需要更改，即 USB 调试：

![](img/762492a8287f369cc1ac4621a212f193_442_0.png)

其他设置为可选。你可能会发现“保持唤醒”很有用，但如果使用手机，之后别忘了将其关闭。

下一步是检查 Android SDK 管理器。对于 Oreo (8.1)，你应当（至少）安装以下组件：

![](img/762492a8287f369cc1ac4621a212f193_443_0.png)

如果和我一样有多个更新，你肯定应该更新。

基本知识就讲这些了。具体取决于你的手机，你可能需要安装一些 USB 驱动程序。如果是这种情况，请参阅前面的链接。

当你连接手机时，请选择传输文件。

Visual Studio 应当会自动识别手机已连接，且运行图标应会从模拟器更改为实际手机：

![](img/762492a8287f369cc1ac4621a212f193_444_0.png)

现在，当你运行项目时，它应当会在实体设备上启动，而非模拟器上。

## 摘要

在本章中，我们了解了如何处理两个相对复杂的问题——跨平台开发和机器学习——并利用一些易于访问的服务，创建一个应用程序，而几年前，编写该应用程序需要专家团队花费数年时间。

当然，该应用程序 *确实* 需要专家团队花费数年时间来编写——只是你现在可以使用他们的劳动成果。

许多公司都尝试过跨平台开发：Microsoft 首先将 Xamarin 引入公司（通过收购），而现在它出现在代码库中（2019 版本宣布，2020 年 .NET 将合并 Mono、.NET Core 和 .NET Framework 为单一 .NET —— 撰写本书时，这简称为 .NET 5）。

机器学习正吸引数十亿美元的研究投入：想象一下在不久的将来，你的手机能够监控你的情绪。想象一下商店里的摄像头能够检测到顾客因不满意而离开；想象一下街头的摄像头能够仅凭行人的面部表情就检测出其是否正准备犯罪。

在下一章中，我们将更详细地探讨机器学习，并创建一个使用 Microsoft 语言理解智能服务来模拟与人类对话的聊天机器人。

## 10 面向 21 世纪的 Eliza - UWP 与 MS Bot 框架

图灵测试由艾伦·图灵开发，旨在回答机器是否可能被视为拥有思考能力的问题。在 20 世纪 60 年代，创建了名为 Eliza 的程序，它是首批尝试通过此测试的程序之一。

图灵测试诞生于机器学习概念尚处于萌芽期之时（事实上，图灵本人也为该领域做出了贡献）。如今，我们拥有使用机器学习检测情感的系统，拥有能够理解语言的复杂系统，并且会接到来自机器人的电话，与之进行完整的口头对话却从未意识到它们并非人类。

Eliza 的设计旨在模拟心理治疗师，其理念是提取关键字和短语，并将其转化为问题，从而让人类相信自己正在与另一个人类交谈。

在本章中，我们将创建 Eliza 的一个版本。我们将学习如何使用 .NET Core 3 在 UWP 中创建客户端应用程序。然后，我们将学习如何设置和配置 LUIS，最后，我们将学习如何将其集成到 MS Bot 框架中。

本章将介绍以下主题：

- 聊天机器人
- Microsoft 聊天机器人框架
- LUIS (Microsoft 语言理解服务)
- 在 UWP 中创建应用程序

聊天机器人可用于从 IT 支持到预订航班的各种用途（以下图片显示了一个示例）：

![](img/762492a8287f369cc1ac4621a212f193_447_0.png)

## 技术要求

在本章中，我们将使用创建使用 Bot Framework 的 Microsoft 机器人的默认模板。撰写本书时，尽管 .NET Core 3 已发布，但 Bot Framework 项目仍在 .NET 2.2 中创建。在后续章节中，我们将了解如何将其升级到 .NET Core 3；但是，到本书出版时，情况可能会有所变化。

在本章中，你需要一个 Azure 订阅（我们在第 2 章“使用 Cosmos DB 记录任务错误的 ASP.NET Core MVC 应用”中简要介绍了如何创建一个）。

本章代码可在 GitHub 上找到：https://github.com/PacktPublishing/C-8-and-.NET-Core-3-Projects-Using-Azure-Second-Edition。

## 创建聊天机器人

让我们先从聊天机器人开始。撰写本书时，聊天机器人无处不在，无论你是在网站上寻求帮助，还是在致电呼叫中心。本质上，聊天机器人只是一个复杂的状态机，而 Microsoft Azure Bot Framework 之类的框架只是向你隐藏了这种复杂性。请想象一下人类与机器（H 和 M）之间的以下对话：

```
M: Hello, how can I help?
H: Hello, I'd like to check my balance, please.
M: I can help you with that. Which account is this for?
H: Current.
M: I'm sorry, I didn't get that. Which account is this for?
H: Current account.
M: What is the number of your current account?
```

好的，如果我们查看这次交互，可以看出是由机器人发起的。客户回答说他们想查询余额——因此这里机器人正在监听关键字，而余额显然就是其中之一。现在机器需要知道客户想查询哪个账户的余额。在幕后，它可能已检查客户名下是否注册了多个账户。此时，客户说的是“当前账户”，机器人需要澄清，因此会循环返回并再次询问。当它最终理解答案后，就会继续。

我们将使用模板创建我们的机器人，你需要从 https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderdev4 安装该模板。

