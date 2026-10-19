

# The Telegraph Media Group

我在这本书中多次提及 *Telegraph* 的历程。简单来说，*Telegraph* 是一家主要的英国新闻出版商，它启动了一个项目来改善网站性能，重点关注用于广告和分析的第三方脚本。

## Problem

*Telegraph* 发现其自有的第一方代码仅占页面加载期间请求的约 5%。绝大多数请求来自用于广告、分析、社交媒体集成和其他服务的第三方脚本。这些第三方不在团队的直接控制之下，但对网站的加载性能产生了显著的负面影响。挑战还在于组织内不同的团队负责与不同的第三方供应商的关系，使得协调变得困难。

## Optimizations

*Telegraph* 工程师 Gareth Clubb 强调，最初，团队自身的代码优化仅影响了约“5% 的请求”。其余都是超出其直接控制范围的第三方。这一认识促使 *Telegraph* 工程团队将重点转向更广泛的组织参与。团队创建了一个性能特遣队，并设法延迟了几乎所有第三方脚本。

## Results

不仅网站速度有所改善，而且 *Telegraph* 的业务指标并未受损。事实上，其案例研究观察到，“我们可以有信心地说，延迟我们的 JavaScript 并没有扭曲任何现有分析，也绝对没有延迟任何广告。通过在广告代码中使用自定义性能标记，延迟 JavaScript 并减少包大小，First Ad Loaded 指标平均提高了四秒。” 这是对加快网站速度不会损害广告收入的有力验证；它可能极大地改善了用户参与度，以至于所有内容仍然可以正常追踪。

*Telegraph* 的方法将技术上的延迟与跨团队协作相结合，以定期审查标签。这是一个文化和技术齐头并进取得成功的典型例子。

# The Economic Times: Passing CWV at Scale

*Economic Times*（ET）是印度最大的新闻网站之一。

## Problem

在 2021 年，ET 的许多页面未能通过 Google 的 CWV 标准，这可能会对其搜索排名造成不利影响。ET 的页面内容丰富，并包含广告和社交嵌入等第三方元素。最初，只有约 62% 的页面浏览量处于“良好”的 CLS 范围；约 26% 为“差”（CLS 超过 0.25）。LCP 在第 75 百分位约为 4.5 秒，也很差。团队确定缓慢的服务器响应、未优化的字体和延迟加载的广告是主要问题。

## Optimizations

在九个月的时间里，ET 的开发团队对网站性能进行了全面改造，包括：

### Optimizing server response (TTFB)

ET 切换到更快的 DNS 提供商，并在前端模板中添加了缓存层（使用 Redis），以便重复访问或高流量激增时可以从缓存中提供服务，而无需每次都访问源服务器。

### Putting “critical requests first”

ET 在加载序列中优先考虑关键的 above-the-fold 内容。这涉及延迟初始渲染不需要的脚本，例如分析和 below-the-fold 小部件，以及使用 `<link rel="preload">` 为关键资源（如大图像和关键 CSS）预加载。ET 还通过尽可能打包来减少并发请求的数量，因为浏览器一次只能处理这么多请求。

### Switching the font-loading strategy

文本渲染因自定义网络字体而延迟。通过将默认 `font-display:auto` 改为 `font-display:swap`，团队允许文本立即以系统字体显示，然后在准备好时切换到自定义字体。这消除了空白文本闪烁，并使内容更快可读，从而改善了 LCP 和 CLS（因为延迟的字体交换可能会导致布局偏移）。

### Using Brotli compression

在服务器端，ET 团队将文本资产的压缩从 GZip 切换到 Brotli。Brotli 使 JavaScript、HTML、CSS 和字体文件减小了 15% 到 18%，从而减少了下载时间。

### Preconnecting strategically

团队审计了第三方域名，并为始终需要的域名添加了 `<link rel="dns-prefetch">` 或 `preconnect`。这节省了建立网络连接的宝贵时间。然而，团队谨慎地仅对肯定会使用的域名使用 preconnect，以避免浪费资源。

### Code splitting and deferring scripts

ET 团队将其 JavaScript 包拆分为更小的块，仅加载初始内容所需的内容，并将剩余内容延迟到以后。ET 还延迟了任何未使用的脚本；例如，重型分析或幻灯片脚本在主要内容之后加载。

### Reserving space for ads and embeds

ET CLS 问题的主要来源是广告加载并推动内容，因此团队为广告位和其他嵌入容器创建了带尺寸的样式占位符，以防止布局跳动。

### Adding explicit image dimensions

与广告类似，ET 为所有图像和视频容器添加了明确的宽度和高度属性或 CSS。这对于消除文章页面上许多内联图像逐步加载时的累积偏移至关重要。

### Optimizing long tasks (FID/INP)

虽然主要关注点是 LCP 和 CLS，但团队还拆分了长时间的 JavaScript 任务以改善交互性（FID）。团队使用了诸如 requestIdleCallback 和将重型循环拆分为更小块等技术。团队在可能的情况下将第三方脚本的加载延迟到用户交互之后，例如延迟加载社交分享按钮。团队还延迟加载 below-the-fold 广告，以便主线程不会在用户尝试交互时被广告加载占用。最后，团队修剪了旧浏览器的 polyfills 以减少 JavaScript 负载。

## Results

到 2021 年 7 月，ET 已取得了实质性的性能改进：

- 用户参与度指标大幅改善：ET 报告称更改后整体跳出率降低了 43%。换句话说，远 fewer 用户登陆页面后立即离开——可能是因为页面现在加载速度快且没有令人不安的偏移，鼓励用户留下阅读。
- LCP“差”情况下降了 33%，平均 LCP 改善了约 80%，从 p75 的 4.5 秒降至 2.5 秒。
- CLS“差”情况下降了 65%（从 25.9% 的会话降至仅 9%），平均 CLS 从 0.25 降至 0.09，改善了 90%，使该指标完全处于“良好”区域。
- 内部，团队还注意到每次会话的页面数和网站停留时间有所改善。更流畅的体验，尤其是在移动设备上（网络较慢），使读者在网站上停留更长时间。
- 因此，整个网站都通过了所有 CWV 阈值，这对 ET 的 SEO 和 Google 的页面体验更新来说意义重大。

*Economic Times* 的案例表明，通过从整体上解决性能问题（网络、服务器和前端），即使是大型新闻网站也能实现快速、类似应用的性能，并在用户留存方面获得回报。

# Yahoo! Japan News: Correlating CLS Fixes to Engagement

Yahoo! Japan News 是日本最大的新闻平台之一，每月约有 220 亿次页面浏览。通过修复文章页面的 CLS 问题，公司看到了明显的改善：每次会话的页面浏览量上升 ~15%，会话时长上升 13%，跳出率下降。

## Problem

通过持续监控，团队注意到 CLS 不佳的页面正在损害用户参与度。使用 Google Search Console 的 CWV 报告，Yahoo 的工程师确定文章详情页模板经常处于 CLS 的“差”区间（图 12-1）。Yahoo 然后使用 Lighthouse 的 CLS 审计（“Avoid large layout shifts”）来查找哪些 DOM 元素在移动。这揭示了某些组件——具体来说是文章标题区域和文章内的广告位——导致了显著的偏移。

Yahoo 还使用 web-vitals JS 库对网站进行仪器化，以在现场测量 CLS 并查看偏移在页面生命周期中发生的确切时间（包括用户滚动触发的偏移）。这种工具组合提供了完整的图景：主要罪魁祸首可能是未预留空间加载的图像或广告。

