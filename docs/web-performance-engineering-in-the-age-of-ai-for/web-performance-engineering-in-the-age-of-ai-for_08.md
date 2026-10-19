### Chapter 13. The Future of Web Performance with AI

证据很明确：性能不仅仅是一个技术细节，它是成功网络产品的关键组成部分。更快、更流畅、更稳定的体验直接带来更满意的用户、更高的参与度和更好的业务成果。虽然网络平台和优化技术不断发展，但核心原则保持不变：从用户体验开始，然后反向推导到技术。

构建高性能网站需要整体方法：理解用户感知、掌握技术优化、负责任地管理依赖关系，并培养持续测量和改进的文化。它要求对面临不同设备和网络限制的用户产生同理心，并致力于高效交付价值。

### The AI Revolution in Web Development

我们正处于网络开发的转折点。AI 辅助编码工具已从新奇事物转变为必需品，从根本上改变了我们编写代码的方式。大型语言模型可以在几分钟内生成整个应用程序，提供前所未有的生产力提升。然而，正如我们在整本书中探讨的那样，这场革命伴随着一个关键警告：AI 优化的是正确性，而不是性能。

这种悖论很明显。AI 可以比以往更快地生成代码，但这些代码往往体现了其训练数据中发现的“平均”实践——其中包含无数次性能模式不佳的示例。AI 会很乐意生成一个可工作的 React 组件，但会包含不必要的重新渲染、主线程上的繁重计算，或损害核心网页指标（CWV）的布局不稳定。代码通过了基本测试，但在真实设备上的真实用户面前却失败了。

这一现实要求我们对开发者角色的思考发生根本性转变。我们不仅仅是在编写代码——我们是在策划、验证和优化代码。当你使用 AI 辅助时，你必须成为性能倡导者，明确引导工具走向高效解决方案。问题不在于是否使用 AI（这艘船已经启航），而在于如何负责任地使用它，同时保持用户应得的性能标准。

好消息是，工具生态系统正在迅速发展。AI 助手开始理解性能上下文。像 Chrome MCP 服务器这样的 DevTools 集成现在允许 AI 代理记录跟踪，识别影响交互到下一次绘制（INP）的长任务，并提出有针对性的重构。当你要求 AI“改进性能”时，它越来越多地能够用实际的分析数据来支持这一点，而不是笼统的建议。

然而，截至今天，这些能力仍然需要人工监督。AI 可能正确识别性能瓶颈，但提出的修复方案在孤立情况下有效，却破坏了现有功能或制造了新问题。验证循环——使用 Lighthouse 分析、真实设备测试、使用 RUM 测量——仍然牢牢掌握在人类手中。将 AI 生成的优化视为你会如何对待初级同事的代码：有价值的输入，需要彻底验证。

我们也看到 AI 成为性能问题的一部分本身。生成代码的便捷性意味着总体上产生更多代码，如果不加控制，可能会使应用程序变得更重。行业调查证实，超过一半的开发团队在采用 AI 辅助开发后报告性能问题增加。速度是真实的，但风险也是如此。

### Streaming Text

当 AI 通过流式传输 token 到客户端生成文本响应时，传统的性能指标已不足够。

### Metrics

Time to first byte (TTFB) 告诉你流何时开始，但对流式体验的质量本身却说不上什么。你需要新的指标来捕捉用户对持续流的感知：

Time to first token

这衡量用户提交提示后，第一个非空白 token 到达客户端的时间。它类似于页面加载时的首次内容绘制：用户看到系统正在积极响应的那一刻。快速的首次 token 时间（TTFT）对于感知响应性至关重要。

Time to useful token

并非所有第一个 token 都同样有价值。如果 AI 回复“让我想想这个问题…”或以序言开头，用户实际上还没有获得效用。该指标捕捉第一个语义上有意义的 token 出现的时间：第一个项目符号、第一行代码，或第一句回答其查询的句子。这需要一些判断来定义，但它是用户感知价值的更好代理。

Tokens per second

每秒 token 数（TPS）是 token 到达的平均速率，关键是第 95 百分位数。用户会注意到 token 流动持续缓慢，而异常值（非常缓慢的突发）即使平均值良好，也会使体验感觉卡顿。

Stall rate



这是流式传输过程中没有 token 到达超过阈值时长（通常为 200 毫秒或更长）的时长占比。频繁的停顿会让 AI 感觉不可靠或吃力，即使总完成时间是合理的。流畅、稳定的输出会显得更智能、更有信心。

### 最后 token 的时间

最后 token 的时间（TTLT）是从提交提示到接收到最后一个 token 的总时间。虽然与传统页面加载相比不那么关键（因为用户能获得渐进式价值），但它对于完成和规划后续交互仍然很重要。

### UI 提交延迟

这是 token 从网络到达，到在屏幕上渲染显示的时间。高 UI 提交延迟意味着用户会看到文字卡顿或更新延迟，降低实时流式传输的感觉。

### 性能预算

基于用户感知研究和实际部署经验，以下是可为你的应用调整的初始预算：

### 首个 token 在 500 毫秒以内（热路径）

在会话中的后续交互中，当连接已建立且模型可能已缓存上下文时，目标是在半秒内发出首个 token。这能保持对话感。

### 首个 token 在 1 秒以内（冷路径）

在初始请求时，一切都是冷启动——没有连接复用、没有缓存上下文——一秒是一个合理的目标。用户在首次交互时通常更宽容。

### 简单提示的可用 token 在 1 秒以内

如果用户提出直接问题（“法国的首都是什么？”），他们应该在 1 秒内看到有用的答案开始出现。

- UI 提交延迟低于 50 毫秒（p95）

token 到达与屏幕更新之间的差距应几乎不可感知。更高的延迟会造成可见的卡顿并感觉滞后。

- 停顿间隔低于 200 毫秒（p95）

token 流中的间隙应很少且短暂。频繁或长时间的停顿会让 AI 感觉缓慢或卡住，即使平均 TPS 是可接受的。

这些预算会根据你的模型复杂度、用户的期望和你的用例而变化。创意写作助手如果质量更高，可能容忍更慢的生成，而代码补全工具则需要足够敏捷以维持开发者的流畅度。

### 渲染策略

渲染流式文本的朴素方法——即 token 到达时将其逐个追加到 DOM——会造成严重的性能问题。在快速模型和良好网络下，token 可能以每秒 20 到 100 个或更多速度到达。每秒触碰 DOM 100 次会触发同等次数的布局和绘制，可能占用主线程，使整个界面迟缓：

- 将 token 合并为微批次

不要渲染每个单独的 token，而是将其缓冲并批量提交。一个经验法则是每 50 到 100 毫秒更新一次显示，或在累积 5 到 10 个 token 后更新，以先到者为准。这能大幅减少 DOM 抖动，同时保持流畅流式传输的感知。每秒 50 次单独 DOM 变更与每 100 毫秒一次变更的区别，就是卡顿混乱与丝滑文本流的区别。

### 保持主线程空闲

文本流式传输通常涉及解析 markdown、应用语法高亮（针对代码响应）或格式化（针对结构化输出）。这些操作 CPU 密集，不应阻塞主线程。将此工作移至 web worker。使用 postMessage 配合可传输流将原始 token 发送到 worker，让其进行繁重处理，然后返回可直接渲染的 HTML 或虚拟 DOM 差异。主线程即可以最少工作应用这些，保持 UI 响应。

### 使用固定布局防止位移

随着文本流式传输并换行，内容高度会变化，可能导致布局偏移，损害累积布局偏移（CLS）分数并造成视觉上的突兀。为流式响应预留空间，使用固定的行高和合理的最大宽度估计。考虑使用可容纳典型响应的 min-height 容器。对于非常长的响应，考虑虚拟化，如下所述。

### 虚拟化长对话记录

在用户可能有数十次交互的对话界面中，渲染每条消息都会拖慢页面。实现虚拟化：仅渲染当前在视口中可见的消息（加上上下少量缓冲）。折叠或完全从 DOM 中移除视口外的内容。像 react-window 或 virtual-scroll 这样的库可以帮忙，但原则很简单：如果用户看不到，就不要支付渲染成本。

### 支持 AbortController

用户会改变主意。他们可能在流式响应进行到一半时意识到问错了问题，或想优化提示。实现通过 AbortController 的取消

### 感知速度模式

即使技术性能良好，你也能通过聪明的 UX 模式让流式传输*感觉*更快：

- 先发出快速大纲，然后流式传输细节

如果 AI 能快速确定响应的结构（例如“以下将涵盖三个要点”），立即发送该大纲，然后逐步填充每个部分。用户能立即看到进展和结构，即使详细内容需要更长时间。

- 先流式传输元数据

在检索增强生成（RAG）系统中，在散文开始流式传输前，先显示检索到的来源或引用。用户能看到相关信息已被找到且系统正在工作，即使综合需要片刻。

- 工具调用的乐观 UI

当 AI 决定调用工具或函数（搜索数据库、获取数据）时，立即显示占位符（“正在搜索文档…”或骨架 UI）。结果到达时再替换。这可防止空白期并保持感知动量。

### 流式 UI 组件

文本流式传输只是一种模式。现代 AI 应用越来越多地即时生成丰富的交互式 UI 组件：图表、交互式表格、表单和嵌入式小部件。这些不仅仅是文本；它们是需要逐步渲染并使其交互的部分组件树和数据岛。

在流式 UI 中，你不仅仅是将字符串追加到 DOM。你接收的是组件树的片段：“这里是包含前三项的搜索结果组件，这里是带初始数据的图表组件，这里是下一步的表单组件。” 一些框架（如 React Server Components）和库正在出现以原生支持此模式，但核心挑战相同：你需要在保持交互稳定的同时，部分渲染并演进 UI。

用户期望看起来就绪的界面部分实际上是就绪的。如果按钮出现在屏幕上，他们就会点击。如果它尚未交互，因为你仍在水合或等待更多数据，就会产生挫败感。关键是渐进增强：确保每个交互岛在可能的最快时间变得可用，即使 UI 的其他部分仍在加载。

### 流式 UI 指标

流式 UI 的关键指标包括：

- 首个可交互岛的时间

第一个可接受用户输入的组件何时真正变得可交互？这类似于页面的 TTI，但范围限定在流式组件。如果你在流式传输搜索结果页，何时用户可以点击第一个结果链接或在筛选框中输入？

- 首个有意义组件的时间

这是第一个真正帮助用户采取行动的组件。不仅仅是加载骨架或状态消息，而是实际有用的元素——第一个搜索结果、第一个带数据的图表、向导界面的第一步。这是你的流式等价物 LCP：用户感知到价值的时刻。

- 岛屿水合时间（按组件类别的 p95 和 p99）

在流式组件世界中，不同类型的组件可能在不同时间到达并具有不同的水合成本。跟踪每个组件类别（DataTable、Chart、FormInput）在到达后变得可交互所需的时间。高百分位值能告诉你最差的用户体验。

- Suspense 揭示延迟



在使用 suspense 边界的框架中，这衡量的是组件的数据到达到组件变得可见（替换其 fallback）之间的时间。这里的延迟过长意味着用户即使数据已在客户端，也仍盯着加载 spinner，这感觉很浪费。

### 流式组件的渲染策略

你可以使用的一些渲染策略包括：

- *先流式传输 shell，然后填充 islands*

发送一个最小的 HTML shell，其中包含布局、导航以及主要内容区域的占位符。这能快速让内容出现在屏幕上（改善感知性能），并允许用户开始与静态元素交互（滚动、阅读标题等）。然后在组件准备就绪时流式传输数据密集型或交互式 islands。关键是确保用户即使在其他组件仍在加载时也能与 shell 交互（例如点击“取消”按钮或调整筛选器）。

- *按优先级顺序 Hydrate*

并非所有组件都同等重要。输入字段、主要操作按钮和导航应优先 hydrate。丰富的可视化、分析小部件或装饰性元素可以稍后再处理。为组件标记优先级级别并按该顺序 hydrate。用户应能够立即开始与界面中最关键的部分交互，即使辅助部分需要更长时间才能完全交互。

- *使用 CSS containment 和 content visibility 属性*

现代 CSS 属性如 `content-visibility: auto` 和 `contain: layout size` 可以大幅降低离屏或折叠下方组件的渲染成本。它们告诉浏览器可以跳过对当前不可见元素的渲染工作。这是你浪费 CPU 周期并导致卡顿的情况。暂停或节流当前不可见的流。当用户滚动到该部分或展开它时，你可以恢复全速渲染。

### 因节点重新父化而产生的闪烁

如果你的 reconciliation 逻辑移动 DOM 节点（更改其父级或兄弟顺序），它会引起可见闪烁，尤其是在这些节点包含图像或 iframe 时。在流式传输期间尽量减少 DOM 重构。列表项上保持稳定的 key，并避免不必要的重新排序。如果必须移动节点，请以批处理、同步提交的方式进行，以减少可感知的闪烁。

### 流式内容和媒体

文本和 UI 组件并不是唯一会流式传输的东西。AI 应用可能会即时生成或获取图像、音频、视频或大型数据馈送。每种媒体类型都有其自身的性能考虑。

### 原地增长的列表和 Feed

想想无限滚动或随着用户到达底部而加载更多内容的 feed。在 AI 上下文中，这可能是逐步改进的搜索结果，或生成的项目 feed。这里的重要指标包括首次可见项的时间（用户不应盯着空白页面）、批次之间的时间（一致的节奏比零星的突发感觉更好），以及滚动卡顿率（在插入新内容时滚动是否仍然流畅？）。

为了解决这些问题，你可以进行服务器分页并在用户到达当前末尾之前预取下一批。回收列表项的 DOM 节点，而不是不断追加新节点（再次虚拟化）。至关重要的是，避免在滚动事件期间测量布局：使用 `IntersectionObserver` 来触发无限滚动，而不是滚动事件处理程序，后者会阻塞主线程并导致卡顿。

### 音频和视频生成或文本转语音 (TTS)

AI 生成的媒体如 TTS 音频或视频合成可以逐步流式传输，就像传统视频内容的自适应流一样。关键指标是首次可播放帧或音频块的时间（播放开始有多快）、停顿频率（播放是否在等待更多数据时暂停），以及缓冲区欠载次数（客户端多久一次耗尽数据）。

可以尝试的技术包括选择与网络变化相匹配的片段大小。太大，用户在播放任何内容之前需要等待很长时间；太小，你会在频繁的网络往返中浪费开销。保持少量客户端缓冲区（媒体的一两秒）以平滑网络波动，但优先选择低延迟编解码器以最小化生成和播放之间的时间。对于真正实时应用（如语音助手），优先考虑低延迟而非高质量。用户更喜欢即时、质量稍低的响应，而不是明显更慢的高质量响应。

### 逐步到达的图像

AI 图像生成服务通常在几秒内生成图像。你可以像渐进式 JPEG 一样逐步流式传输它们，或按图块传输。提前为它们预留空间。使用 width 和 height 属性或 CSS 在图像到达前预留其空间，防止布局偏移。对 hero 图像使用 loading="lazy" 和 fetchpriority="high" 来优先处理重要内容。

渐进式或分块交付也是一个选项。如果你的图像生成器支持它，先流式传输低分辨率预览，然后逐步细化它。或者如果图像非常大，按图块（像地图图块一样）交付。这让用户即使最终高分辨率版本需要时间也能立即看到内容。使用不透明度或滤镜过渡使细化图像平滑淡入，避免从模糊到清晰的生硬跳变。

### 负载下的调度与响应性

流式 AI 界面的最大挑战是在持续处理和渲染传入数据时保持响应性。主线程是你最受限制的资源。它负责处理用户输入、运行 JavaScript、计算布局和绘制。如果用流式工作占用它，整个界面会变得迟缓。推荐的策略包括：

- *将用户输入视为最高优先级*

用户输入如点击、轻触、键盘输入和滚动必须以最小延迟处理。在流式场景中，你不断更新 UI，很容易让渲染工作主导主线程，导致输入延迟。

在微批次之间始终检查是否有待处理的输入。在将另一批 token 或组件提交到 DOM 之前，yield 并检查是否有输入事件正在等待。如果有，先处理它。这确保用户的交互永远不会被渲染工作阻塞。

- *频繁 Yield*

如果任何渲染或处理任务花费超过 16 ms（60 fps 下的一帧），就将其拆分。使用 `requestAnimationFrame` 或 `scheduler.postTask`（如果可用）将工作拆分为更小的块并在它们之间 yield。这给浏览器机会处理输入、运行动画并保持流畅。

一个简单的模式：如果你在处理大量 token 或组件，将它们拆分为例如 50 个项目的组。处理一组，yield，处理下一组，yield，依此类推。这即使在有大量工作时也能保持主线程响应。

- *使用协作调度器*

为工作实现优先级队列。高优先级任务应包括处理用户输入和渲染可见流。中优先级

### 实现反压

如果服务器生成 token 的速度快于客户端渲染的速度，你可能会压垮客户端的渲染管道。监控客户端渲染队列的大小（有多少 token 或组件等待处理）。如果它超过阈值（例如 100 个待处理项），向服务器发出信号减慢生成速度。这称为 *backpressure*。服务器可以暂停生成、在块之间引入少量延迟，或将更多 token 批处理到更少的消息中。这可以防止客户端被无法跟上的数据淹没，否则会导致 UI 卡顿或挂起。

### 智能缓存

AI 响应通常是唯一的，但存在缓存机会：

- *语义缓存*

如果两个用户问本质上相同的问题（即使措辞不同），返回缓存的响应。这很棘手，因为它需要理解语义相似性，但可以大幅加速常见查询。

- *检索缓存*

在 RAG 系统中，为常见主题缓存检索到的文档或嵌入。当用户询问热门主题时，检索步骤可以是即时的，而不是搜索大型语料库。

- *非个性化组件的响应缓存*

如果你正在流式传输通用 UI 组件（如标准图表或表格格式），缓存组件模板。只有数据需要流式传输；结构可以重用。

### 深思熟虑地分块

优先考虑语义边界。不要发送固定数量的 token 或字节，而是按逻辑单元分块——文本的句子、数据的行、段落



for prose. 这减少了客户端解析和显示连贯内容所需的工作量。如果令牌在句子中间到达，客户端可能需要缓冲并等待句子结束才能合理地渲染。发送完整的句子或代码行可以简化客户端逻辑并改善用户体验。

同时也要为分块速率设置硬上限。在网络非常快且服务器功能强大的情况下，你可能会生成并发送令牌的速度过快，导致客户端跟不上。将发送分块的速率限制在上限（例如，每 10 ms 不超过一个分块），以确保客户端有时间处理每个批次而不被压垮。

### CWV in a Streaming World

CWV（LCP、INP 和 CLS）是为传统页面加载设计的。在流式 AI 界面中它们仍然很重要，但解释方式略有不同：

*LCP*

这衡量最大初始内容何时出现，在流式界面中仍然很重要。这可能是聊天输入区域、对话中的第一条消息，或流式响应区域的骨架 UI。

优化你的 shell：初始页面加载（在任何流式传输开始之前）应该很快。快速将聊天界面、输入框和任何静态 UI 显示在屏幕上。这建立了信任和就绪感。一旦用户提交提示，流式性能就会接管，但 shell 的首次绘制决定了基调。

*INP*

INP 成为主要的安全护栏。它衡量整个页面生命周期中用户交互的延迟。在流式界面中用户会持续交互：输入后续提示、点击按钮、滚动、突出显示文本。

在活动流式传输期间测量 INP，而不仅仅是页面开始时。你的目标是确保即使在界面正在主动接收和渲染流式数据时，INP 仍保持较低。如果在流式会话期间 INP 飙升，这表明你正在用渲染工作阻塞主线程。专门分析这些场景并优化渲染管道（批处理、Web Worker、yielding），以保持 INP 较低。

### CLS

CLS 很容易退化。这一核心网页指标衡量意外的布局移动。在流式界面中，内容不断被添加，如果处理不当很容易导致位移。

为避免这种情况，为流式内容预留空间。如果你知道响应即将到来，在它到达之前就分配空间。使用 `min-height` 或固定高度容器。避免在用户当前滚动位置上方追加内容而没有仔细处理，否则会将所有内容向下推移，损害 CLS。

此外，避免在不考虑高度的情况下在首屏上方追加内容。如果你插入的内容位于视口上方或用户正在查看的位置上方，你可能会移动他们的视图。使用滚动锚定（现代浏览器支持）或手动调整滚动位置以保持用户视图稳定。

除了 CWV 之外，还为你的流式指标定义 SLO，特别是首个令牌时间、每秒令牌数、停顿率和 UI 提交延迟。在生产环境中跟踪这些指标并设置阈值。将发布同时基于 CWV 和你的流式 SLO 进行门控。改进初始加载速度但降低流式性能的更改不是净收益。

### Observability and Debugging Streaming Systems

流式 AI 界面复杂且动态，使可观测性至关重要。你需要了解生成、传输和渲染循环中发生的情况才能诊断问题。

### Instrument the Streaming Path with Performance Marks

使用 performance API 在流式生命周期的关键点添加标记：

- `mark("prompt-sent")` 当用户提交请求时
- `mark("first-token")` 当第一个令牌到达时
- `mark("useful-token")` 当第一个有意义的令牌出现时
- `mark("batch-commit")` 每次将一批更新提交到 DOM 时
- `mark("last-token")` 当流完成时

测量这些标记之间的间隔以计算你的流式指标。将它们记录到你的分析或 RUM 系统中。对用户进行聚合以了解 p50、p95 和 p99 性能。

跟踪队列深度：监控渲染管道中任何时刻待处理的项目数量。如果队列变得很大，说明客户端落后了。对此条件发出警报并调查是否需要限制生成、优化渲染或两者兼而有之。

务必在流式传输期间捕获输入延迟。仪器在流活动期间发生的任何用户交互（例如点击和按键）。测量从事件到视觉反馈的时间。这里的延迟高表明主线程竞争，需要优化。

### Build a Stream Trace View

在开发和暂存环境中，构建一个可视化整个流式过程的工具。将网络分块链接到 UI 提交以及主线程上的长任务。该跟踪应显示：

- 每个分块何时从服务器发送
- 它何时到达客户端
- 它在渲染队列中停留了多久
- 它何时提交到 DOM
- 此时发生的任何长任务

此可视化可帮助你识别瓶颈：服务器生成缓慢吗？网络增加了延迟吗？客户端的渲染管道是否积压？

### Alert on Stall Patterns

设置停顿率的监控（具有超过阈值的间隙的流的比例——例如 200 ms）。停顿率上升表明模型性能下降、网络问题或服务器过载。重复的长时间间隙会让用户沮丧，应触发警报以便调查和补救。同样，如果 UI 提交延迟超过你的预算，请发出警报。这表明客户端渲染问题。

### Accessibility

流式界面除了原始性能之外还带来独特的挑战，包括可访问性。一些有用的提示如下：

*Use ARIA live regions for streamed text*

对长篇 AI 响应使用 `aria-live="polite"`。这允许屏幕阅读器在更新到达时宣布更新，而不会不断打断用户。将 aria-live="assertive" 保留用于简短的关键更新（如错误消息或非常简短的确认）。使用实际屏幕阅读器测试以确保体验连贯且不过载。

### Provide stop and pause controls

用户——尤其是使用辅助技术的人——需要能够在流式传输过快或需要时间处理时停止流。提供清晰、可通过键盘访问的“停止”按钮。响应稳定后，提供“复制为已渲染”选项，以便用户在不与实时更新内容斗争的情况下提取文本。

### Internationalization

对于从右到左阅读的语言（阿拉伯语、希伯来语），确保你的流式逻辑尊重文本方向。使用双向文本和换行，并使用实际的从右到左内容进行测试。结合适当的 unicode-bidi CSS 属性和 dir 属性。中文、日文和韩文以及一些具有复杂脚本的其他语言需要适当的换行和字体渲染。确保你的分词和批处理逻辑不会 awkward 地拆分单词或短语。这可能意味着根据检测到的语言调整你的微批处理大小。

尽早处理复杂脚本。令牌可能会在阿拉伯语和印地语等语言中拆分字素簇，甚至是表情符号。不要渲染部分字符。在你拥有完整的字素簇之前缓冲传入的令牌，然后再渲染它们。这可以防止视觉故障并确保文本在所有书写系统中可读。

### Privacy

避免逐字符流式传输敏感输入。某些应用程序在用户输入时将用户输入发送到服务器（用于自动完成或实时

更多的计算意味着更多的功耗。长任务和繁重的渲染会损害电池寿命。

### GPU time (if doing on-device inference)

一些应用程序使用 WebGPU 或 WebAssembly 在本地运行模型。这可能非常耗电。

对这些进行仪器化并记录它们。将它们与用户反馈或电池使用报告相关联，如果你的应用分析中可用。

成本也是一个因素。模型推理不是免费的。每个生成的令牌都会产生计算和 API 费用成本。在高容量应用程序中，成本会迅速 spirals，因此预算这些每令牌成本。使模型的速率和精度可配置可以帮助。提供用户“快速”模式，使用较小的、更便宜的模型进行快速查询，以及“高质量”模式，使用较大的模型进行复杂任务。让他们根据自己的需求和你的成本限制进行选择。

对于使用移动设备或低功耗模式的用户，考虑降低流式速率，使用较低质量的模型，或提供摘要而不是完整的详细响应。这尊重用户的设备限制和你的基础设施预算。

### Design Patterns to Prefer

某些 UX 和架构模式特别适合流式 AI 界面：

### Outline, then enrich

首先生成快速的高级大纲或摘要（在前 500 ms 至 1 s 内），然后在每个部分下方流式传输详细内容。用户立即看到结构，了解即将出现的内容，并且可以在详细信息仍在填充时导航到他们关心的部分。例如，对于多部分答案，首先



stream “I’ll cover: 1) X, 2) Y, 3) Z,” and then fill in each section progressively. This provides orientation and context.

### Guarded tool use

When the AI needs to call a tool or query a database (which are common in agentic workflows), run the tool call asynchronously but reveal its status quickly. Show a placeholder: “Fetching data from database…” or “Running calculation…” If the tool is slow, give the user a retry or cancel option. When the results arrive, replace the placeholder smoothly. This keeps the user informed and in control, even when backend operations take time.

### Pipelined RAG

In RAG systems, you first retrieve relevant documents or data then generate a response based on them. Instead of waiting for retrieval to complete before showing anything, stream the retrieved sources first as they arrive (“Found 3 relevant documents: A, B, C…”), then stream the synthesized answer below. Finally, highlight or link to specific parts of the sources as the answer references them. This gives users confidence in the data and lets them explore sources while the answer is still being generated.

### Streaming diffs for code or docs

When the AI is generating or modifying code or documents, render a stable base version, then apply diffs in place. For example, show the original code with a subtle highlight around the area being changed, then animate the change smoothly. This avoids the entire code block rerendering and jumping around, which would cause scroll jumps and CLS. Users can follow along with the changes without losing their place.

arrival gracefully. Use buffering, adaptive batching, and backpressure signals to smooth out the variability.

### A Practical Checklist for Production

Before launching a streaming AI interface, ensure you’ve addressed these key areas.

Before launch:

- [ ] Preconnect to model and data endpoints. Use `<link rel="preconnect">` or early hints to establish connections before the user submits their first prompt.
- [ ] Ship a minimal, stable shell. The initial page load should be fast (good LCP) and provide a clear, interactive input area. Reserve space for where streaming responses will appear.
- [ ] Add streaming performance marks. Instrument your code with marks for prompt-sent, first-token, useful-token, batch-commits, and last-token.
- [ ] Set up dashboards for token timing and UI commit latency. Have visibility into p50, p95, and p99 for your streaming metrics before you launch.
- [ ] Test on low-end devices and slow networks. Don’t just test on your dev machine. Throttle your network, use an old phone, and ensure the experience is acceptable.

During implementation:

- [ ] Use web workers for parsing and formatting. Keep markdown parsing, syntax highlighting, and other heavy text processing off the main thread.
- [ ] Microbatch token updates. Commit to the DOM every 50 to 100 ms, not on every single token.

understanding performance implications, tools will become more sophisticated at catching issues automatically, and best practices will evolve.

But the fundamental responsibility remains unchanged: to advocate for users. Every millisecond matters to someone on a slow device or constrained network. Every layout shift frustrates a user trying to click a button. Every long task creates the perception that the web is broken.

AI is a powerful amplifier. It amplifies productivity, yes, but it also amplifies our decisions, both good and bad. Use it to amplify your commitment to performance. Make it part of your toolkit for delivering faster, more efficient experiences. But never let it replace your judgment, your empathy for users, or your standards for quality.

As you apply the knowledge gained from this book, remember that performance is an ongoing journey, now intertwined with AI assistance. Use the tools at your disposal (both traditional and AI-powered), implement the strategies, measure the impact relentlessly, and never stop advocating for the users’ time and attention.

The web’s future will be shaped by how we handle this transition. Will AI lead to bloated, slow applications because we prioritized velocity over quality? Or will it enable us to build faster, more accessible experiences because we learned to guide it effectively? The choice is ours.

By combining AI’s capabilities with rigorous performance engineering, by treating the CWV as nonnegotiable requirements rather than nice-to-haves, and by building cultures that value speed and efficiency alongside features, we can deliver on the promises of both AI-assisted development and user-centered design.

The stakes are high, but so is the opportunity. Build fast. Test thoroughly. Optimize relentlessly. And remember: in the end, performance is still about people. By prioritizing performance in this AI-driven era, you contribute not only to the success of your own projects but also to a faster, more accessible, and more delightful web for everyone.
