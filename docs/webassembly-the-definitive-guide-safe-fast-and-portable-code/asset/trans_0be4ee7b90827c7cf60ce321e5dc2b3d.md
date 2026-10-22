

在尝试将无服务器 FaaS 环境和基于区块链的系统货币化时非常有用。我们将在第 16 章中看到 Wasmtime 也具有这种能力。

WasmEdge 团队是 Jamstack 概念的坚定倡导者，这是一套包含 Javascript、API 和 Markup²⁰ 的技术套件。其基本思想是前端在构建过程中被转换为优化的、静态的单页和资源。这使得它们的运行时性能具有降低的复杂性和负载负担。虽然前端可能被转换为静态资源，但它们通过与后端 API 和超媒体系统交互来保持动态行为。

其后端可通过标准 HTTP 客户端（如浏览器或命令行工具如 HTTPie²¹）进行干净的查询。

执行其 FaaS 上部署的函数与以下操作一样简单，但请注意。我省略了编译后的 WebAssembly 代码的字节。

```
brian@tweezer ~> http
https://rpc.ssvm.secondstate.io:8081/api/executables/1
{
  "wasm_id": 1,
  "wasm_sha256": "0xfb413547a8aba56d0349603a7989e269f3846245e51804932b3e02bc0be4b665",
  "wasm_description": "Function as a Service (FaaS)",
  "wasm_as_buffer": {
    "type": "Buffer",
    "data": [
      48,
      44,
      57,
      55,
      .
      .
      .
    ]
  },
  "wasm_state": "{}",
  "wasm_callback_object": "{}"
}
```

正如你所见，可以通过操作资源来定义新的端点。要调用端点的行为以发出友好的、参数化的问候，你可以发出以下命令。

```
brian@tweezer ~> echo "from the future" | http -f POST
https://rpc.ssvm.secondstate.io:8081/api/run/161/say Content-Type:text/plain
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 22
...
Content-Type: text/html; charset=utf-8
Date: Tue, 17 Aug 2021 18:48:13 GMT
ETag: W/"16-kf3Sg+1nPhqMOp7M01tMU9CkSVk"
...

hello from the future
```

WasmEdge 提供的其他功能包括基于能力的安全性、RockDB 支持的存储机制、WASI 集成、TensorFlow 功能等。

一个使用此功能性的示例在此描述 https://www.secondstate.io/articles/faas-face-detection。该示例使用在 FaceNet²² 数据集上训练的多任务级联卷积网络（MTCNN）模型。

该示例包含指向实际代码的 GitHub 仓库的链接，以及通过命令行工具和浏览器构建、部署和调用此功能的过程的描述。提交 1927 年索尔维会议²³ 的一张著名图片的结果显示在图 15-6 中。

## 检测人脸

使用 MTCNN Tensorflow 模型。参见代码教程

请上传一个图像文件（示例）

选择文件 solvey

查找人脸

图 15-6. 在一张著名图片上调用 MTCNN 模型

从 REST API 调用复杂功能本身并不具有开创性。但能够在便携的、高性能的沙箱环境中安全地部署你想要运行的代码则是如此。事实上，你也可以在 Node.js 或其他形式中本地运行相同的行为，这对于测试而言同样有用。

除了基本的 WASI 支持外，WasmEdge 还支持第 12 章中讨论的多个提案，甚至还有一个 WASI socket 扩展用于添加网络支持。WasmEdge 还具有类似于我们从 Wasmer 和 Wasmtime 看到的 API，用于将 WebAssembly 行为嵌入到任意其他应用程序中。

如果你访问 SecondState 网站，你会找到其他关于在区块链、汽车基础设施中使用 WasmEdge 的示例，

以及各种其他场景。我们将在第 16 章中涉及其中一些内容，但现在我们将专注于我们最终的 WebAssembly 应用程序基础设施运行时。

## wasmCloud

wasmCloud 项目²⁴ 是一个复杂且不断发展的技术集合，以前称为 waSCC（代表 WebAssembly Secure Capabilities Connector）。它最近被选为官方 CNCF 应用程序运行时项目，并有望帮助开发人员在我们本章讨论的整个位置范围内构建基于参与者的模型并编排能力。我想重点关注它，因为我认为他们有效地解决了解决图 15-1 最后几列之谜所需的各种工程决策。这当然不是唯一可能做到这一点的项目，但目前它是该愿景最稳健、最全面的提炼。

wasmCloud 提供了对基于云的基础设施的全面抽象。参与者的使用使开发人员能够基于消息处理和基础设施提供的能力来产生业务功能。能力只是非功能性需求的术语，参与者需要应用程序运行时来满足其期望。清晰的分离允许在不同环境中重用，以及在同一环境中热交换底层实现。

与 wasmCloud 相关的工具和框架旨在解决 Liam 在上述演讲中概述的将多语言代码库部署到日益多样化的平台集的复杂性。它们专注于仅添加所需的部分，这就是为什么它们利用 WebAssembly 和 WASI 的原因。团队最近做出了向 Elixir 和 OTP 平台迁移的决定，而不是在该生态系统中手工制作并发解决方案。他们很早就决定将他们的连接基础设施基于 NATS²⁵ 工具链和框架，用于自适应边缘和分布式通信。

最终你得到的是一个跟踪我们行业演进的技术栈，能够满足各种运行时配置和场景的需求。让我们快速浏览一下涉及的层。

栈的底部是 WebAssembly 和 WASI。从这里我们拥有一个可移植且安全的架构，可以在各种能力和资源不同的运行时和环境中运行。与之前的沙箱环境不同，这给了我们语言选择的自由以及高性能结果的好处。此基础层显示在图 15-7 中。

![](img/16b7990eb0dbf589db177bdbd2f4a189_342_0.png)

图 15-7. wasmCloud 栈的 WebAssembly 基础

wasmCloud 在 WebAssembly 基础上扩展了签名包、减少样板代码、可组合的参与者模型以及水平和垂直可扩展性。它支持多租户无状态和隔离的已部署业务功能的执行。此层在图 15-8 中突出显示。

![](img/16b7990eb0dbf589db177bdbd2f4a189_343_0.png)

图 15-8. wasmCloud 云层扩展了 WebAssembly 基础

wasmCloud 基础之上的能力部分是参与者履行其执行模型所需的热插拔非功能性依赖项。wasmCloud 包含广泛的预定义能力，如数据存储、提供网络资源、消息队列等。这些在图 15-9 中可视化。

![](img/16b7990eb0dbf589db177bdbd2f4a189_344_0.png)

图 15-9. 能力为参与者提供非功能性需求

开发人员将在这些能力之上构建并根据需要提供自己的提供者以满足专业需求。存在于此层之上的参与者将以隔离或组合的业务功能存在，可以在更丰富的层中垂直增长。底层的分发模型（基于 WebAssembly 的参与者）保持相当轻量级，并且可以利用其他环境中类似配置的、受约束合适的栈。与 Kubernetes 和容器之间的紧密耦合不同，这为组织提供了更多重用选项。我们在图 15-10 中展示了开发人员花费大部分时间所在的层。

![](img/16b7990eb0dbf589db177bdbd2f4a189_345_0.png)

图 15-10. 开发人员花费大部分时间创建和组合参与者

最后，所有这些都连接到一个自形成和自愈的 Mesh Lattice Network。它使用全局发现机制、可调拓扑以及跨各种云提供商的离线和在线执行模式支持。这种最终的灵活性在图 15-11 中展示。

