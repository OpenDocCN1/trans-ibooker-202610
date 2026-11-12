

位置 82
异地冗余/多区域写入 82

### 配置 Cosmos DB 实例 83

### 将您的 ASP.NET Core MVC 应用程序连接到 Cosmos DB 86

- 添加 NuGet 包 89
- 创建 MongoDbRepository 类 96

### 读写 MongoDB 数据 98

- 创建接口和 WorkItemService 100
- 创建视图 103
- 修改 HomeController 105
- 添加工作项 106
- 重定向到工作项列表 112

### 清理资源 118

- 摘要 120

## 第 3 章：ASP.NET Azure SignalR 聊天应用程序 121

### 技术要求 122

### 介绍 SignalR 122

### SignalR 项目 123

- 配置 Azure SignalR 123
- 创建项目 125
- 设置项目 126
- 添加 SignalR 库 130
- 构建服务器 133
- SignalR Hub 子类 133
- 配置更改 134
- 创建客户端 136
- 包含的库 138
- JavaScript 函数 138
- 命名部分 141
- 聊天输入 142
- 存档功能 143

### 构建并运行项目 144

- 运行应用程序 145
- 开始聊天 146
- 存档聊天 147

### Azure 服务 148

### 清理 149

- 摘要 150

## 第 4 章：使用 Entity Framework Core 的 Web 研究工具 151

### Entity Framework (EF) Core 历史 152

### 代码优先与模型优先与数据库优先方法 152

### 开发数据库设计 153

### 设置项目 154

### 安装所需包 157

- EF Core SQL Server 158
- EF Core 工具 158
- 代码生成设计 158
- 创建模型 160
- 配置服务 161
- 创建数据库 163
- 使用测试数据对数据库进行填充 167
- 创建控制器 170
- 运行应用程序 171

### 测试应用程序 174

- 安排/执行/断言 179
- 内存数据库 179
- 我们在测试什么？ 179
- 速度 180

### 部署应用程序 181

- Microsoft Azure 应用服务 181
- 自定义目标 191
- FTP 193
- Web Deploy 194
- Web Deploy 包 195
- 文件夹 195
- 导入配置文件 196

### 摘要 197

## 第 5 章：使用 Azure 逻辑应用和函数构建 Twitter 自动化活动管理器 198

### 技术要求 199

- 工作流引擎、逻辑应用和函数概述 199

### 创建 Excel 表 200

### 在 Azure 门户中构建逻辑应用 201

### 在 Visual Studio 中构建逻辑应用 203

- Azure 开发工作负载 203
- 逻辑应用扩展 205

### 创建资源组 207

### 创建工作流 210

- 步骤 1 – 选择触发器 210
- 步骤 2 – 读取 Excel 文件 212
- 新 Azure 函数 216
- 部署和测试函数 220
- 遍历电子表格 225
- 发送推文 226
- 添加条件并删除当前行 229
- 向函数传递参数 232
- 回到我们的条件 237
- 删除行 237
- 发布逻辑应用 240
- 故障 242
- 清理 Azure 资源 245
- 摘要 247

## 第 6 章：使用 Identity Server 和 OAuth 2 的存货检查器 248

### 技术要求 249

- Visual Studio Installer – 新工作负载 249

### 身份和权限 251

### 项目概览 252

### 存货检查器应用程序 API 253

- 设置 253
- 添加控制器和路由 255
- 读取库存水平 258
- 更新库存水平 263

### 权限 264

### 客户端应用程序 265

- OnPropertyChanged 268
- 命令和 API 调用 269
- 帮助器类 270

### IdentityServer 4 272

### IdentityServer 275

- 保护 API 275
- 客户端配置 277
  - 登录屏幕 278
  - 调用 IdentityServer 281
  - 功能 283
- 设置 IdentityServer 284
- 调用 API 287
- 授权类型 288
- 创建和使用有效密钥 289

### 授权 292

- 用户和角色 292
- IdentityServer 293
- 客户端 295
  - 逻辑更改和 UI 更改 296
  - 登录和导航更改 299
  - 服务器调用 300
  - 当你看不到时如何更新数量 302

- 摘要 304
- 建议的改进 304
- 建议阅读 305

## 第 7 章：使用 Windows 服务和 Azure 存储构建照片存储应用 306

### 技术要求 307

### Windows 服务和 Windows 兼容包 307

- Windows 服务 307
- Windows 兼容包 309

### 项目概览 309

### 配置 Azure 存储 309

- 热存储和冷存储 312
- 存储冗余 312
- 存储帐户 313

### 创建我们的 Windows 服务 315

- 测试类 318

### 使用 Azure 存储客户端 321

- 配置我们的服务以访问 Azure 存储 323
- 完成云存储客户端 326
- 进一步配置 326
- 日志记录 330
- 仅上传图像 331
- 上传现有图像 333

### 安装 Windows 服务 336

- 代码更改 336
- 安装命令 337

### 测试和调试 339

- 步骤 1 – 检查服务是否正在运行 340
- 步骤 2 – 检查日志文件 341
- 步骤 3 – 检查事件查看器 341

- 摘要 343

## 第 8 章：使用 Docker 和 Azure Kubernetes 服务的负载均衡订单处理微服务 344

### 技术要求 347

- JMeter 347

### 微服务 347

- 探索 Docker 349
- Kubernetes 和编排 350

### 创建我们的微服务 351

- 队列 352
- 销售订单生成器 356
- 服务总线帮助程序库 361
- 测试我们的销售订单生成器和 JMeter 362
- JMeter 364
- 日志记录 367
- 创建新微服务 368
- 创建我们的新 Docker 容器 369
- 创建微服务逻辑 372
- 创建新 Azure SQL 数据库 375
- Entity Framework Core 378
- 创建销售订单 380
- 存储队列 382

### Azure Kubernetes 服务

- 构建 Docker 镜像 384
- Azure 容器注册表 385
- Azure 容器注册表 385
- Azure Kubernetes 服务 388
- Kubernetes 部署 389
- 负载均衡 395
- 清理 397
- 摘要 399

## 第 9 章：情绪检测器移动应用 - 使用 Xamarin 窗体和 Azure 认知服务

### 技术要求 401

### 概念概览 401

### 机器学习 402

### 跨平台和移动开发 402

### 项目概览 403

### 配置 Azure 认知服务 403

### 创建 Xamarin 应用程序 407

- 拍照 412
- Xamarin 插件 413
- 媒体插件 413
- TakePicture() 416
- GetEmotionAnalysis() 417
- DisplayData() 419
- CreateLabel() 421
- GetStrongestEmotion() 422
- Button_Clicked 423
- 在实体设备上测试和运行 424
- 摘要 431

## 第 10 章：21 世纪的 Eliza - UWP 和 MS Bot Framework

### 技术要求 434

### 创建聊天机器人 435

- 机器人模拟器 438
- 回声机器人 - 除了 Hello 442

### 介绍 LUIS 445

### 将 LUIS 集成到 Bot Framework 中 449

- 将模板从 .NET Core 2.1 升级到 3.0 453
- UseMvc 454
- AllowSynchronousIO 454
- 意图/响应矩阵 456
- 发布机器人 460
- 创建通道注册 461
- MicrosoftAppId 和 MicrosoftAppPassword 464

## 目录

### 创建 UWP 应用程序 467

- MainPage 468
- 行定义 469
- ListView 470
- 消息和命令绑定 471
- 数据绑定和视图模型 471
- ObservableCollection 474
- 命令绑定 474
- DirectLineWrapper 475
- 模型 476
- RelayCommandAsync 477
- 通道 479
- Direct Line 和 Web Chat 480
- 机器人客户端 482
- 摘要 486

## 附录 A：WebAssembly 487

### 为什么选择 WebAssembly 487

- 原因一 – 静态类型 488
- 原因二 – 编译型 488
- 原因三 – 速度 488
- 原因四 – 您熟悉的语言/前端和后端使用同一种语言 489
- 原因五 – 现有代码 490
- 原因六 – 部署 490
- 原因七 – 安全性 490

### 编写 WebAssembly 491

- 理解 Blazor 492

## 您可能喜欢的其他图书 498

## 索引 501

