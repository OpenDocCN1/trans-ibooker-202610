## 第四部分 管理依赖项和维护质量

OceanofPDF.com

### 第 8 章 第三方脚本简介

*第三方脚本*是指网站从其自身代码库以外的外部来源包含的任何脚本。虽然第 7 章专注于你编写的 JavaScript 相关的成本，但这些外部脚本通常会带来更大的性能挑战。这包括你手动编写的代码和 AI 工具生成的代码：集成第三方服务的 AI 生成代码仍然继承这些外部依赖的相同性能特征和约束。

第三方脚本允许开发人员和网站所有者快速添加由他人提供的功能或服务，范围从分析和广告到社交媒体 feed 和交互式小部件。实际上，它们通常通过引用脚本 URL 或指向不同域的 <iframe> 来嵌入。例如，一个营销网站可能会包含用于跟踪的 Google Analytics、用于社交分享按钮的 Facebook 软件开发工具包（SDK）、用于嵌入视频的 YouTube iframe，以及来自客户支持提供商或 AI 客户支持聊天机器人的聊天小部件。这些脚本提供有价值的功能，而无需网站所有者从头构建——但包含它们是有代价的。

正如我之前写过的，如果你已经优化了自己的代码，但网站仍然加载太慢，那很可能是第三方脚本的错。换句话说，一旦你尽一切可能加快自己的应用程序代码，任何剩余的性能瓶颈很可能来自第三方内容。第三方脚本超出了你的直接控制范围，但它们会减慢页面加载速度，引入安全和隐私风险，并表现出不可预测的行为。依赖它们意味着信任外部提供商不会降低你自己的用户体验。

本章首先分解第三方脚本是什么、它们在哪里（无处不在），以及它们如何影响你的网站。我将查看关键工具



然后通过介绍一套原则来指导你在决定如何最好地优化其存在时来完成本章。

### 第三方代码无处不在

几乎每个现代网站都以某种形式使用至少一个第三方脚本——根据 HTTP Archive Web Almanac 的数据，比例超过 94%。事实上，桌面端的中位网站从 23 个第三方域名获取资源，而移动端的中位为 21 个。在热门网站上，数量甚至更高：根据同一项研究，前 1,000 名网站平均使用 40 到 47 个第三方集成。而且 Web Almanac 中的一项研究指出，平均网站上加载的总脚本中约有 33% 是第三方脚本。它们无处不在——不是边缘情况，而是网络生态系统的核心组成部分。表 8-1 显示了一些第三方脚本最常见的用例。

表 8-1. 第三方脚本在不同类型网站中承担多种用途

| 第三方内容类型 | 示例 | 脚本功能/用途 |
|---|---|---|
| 分析和跟踪 | Google Analytics, Mixpanel, Facebook Pixel | 记录用户行为和流量来源；衡量转化和再营销 |
| 广告网络 | Google AdSense, 程序化广告网络 | 竞价、广告渲染、跟踪展示次数 |
| 媒体嵌入 | YouTube, Vimeo, Instagram, SoundCloud | 播放和显示视频和音频内容 |
| 社交分享小部件 | Facebook “Like” 按钮 | 社交媒体互动 |
| 聊天和支持小部件 | Intercom, Zendesk | 渲染聊天小部件；处理消息 |
| 标签管理器 | Google Tag Manager | 集中管理分析、A/B 测试和广告标签 |
| A/B 测试和个性化 | Optimizely 或 Google Optimize | 替换或修改页面元素以进行不同测试变体；个性化内容 |
| 实用库 | date-fns, Moment.js | 一个日期选择器库、来自 CDN 的视频播放器库 |
| 电商和 SaaS 集成 | Shopify, Discus | 评论、推荐小部件、分析、外部评论系统 |

在 Web 应用、内容管理系统（CMS）网站（如 WordPress）和电商平台（如 Shopify）中，模式相似：第三方脚本为常见需求提供即插即用的功能。Shopify 商家可能会添加一个用于 upsell 弹出窗口的应用（会注入脚本），而 WordPress 博主可能会粘贴来自 SoundCloud 的嵌入代码来分享音频。现代单页应用（SPA）也使用第三方 SDK。例如，一个 React 应用可能会动态加载 Google Maps API 用于地图组件，或使用 Stripe 的 JS 库进行支付。简而言之，第三方脚本无处不在，存在于每一类网站中。

### 为什么第三方脚本会影响性能

不幸的是，第三方代码的便利性伴随着显著的性能影响。引入浏览器必须从不同源获取并执行的脚本，会将页面加载的关键路径扩展到你自己的代码之外。第三方脚本往往会减慢网页速度的主要原因包括：

*额外网络请求*

每个第三方资源通常都需要向不同服务器发出额外的 HTTP 请求。这意味着额外的 DNS 查找、TLS 握手和数据传输。每个新域名（分析、广告等）都会增加延迟。正如 web.dev 团队所说，“站点需要进行的请求越多，加载所需的时间就可能越长。” 页面可能从自己的服务器快速加载自己的代码，但随后会因等待对广告网络、分析端点和内容分发网络（CDN）进行多次调用而停滞，以获取第三方内容。

#### 大型脚本负载

单个嵌入拉取数百千字节甚至几兆字节的 JavaScript 并不罕见：典型的社交媒体 feed 嵌入或视频播放器很容易包含多达 500 KB 的脚本。这可能占页面总字节数的很大一部分。对于连接较慢或数据计划有限的用户来说，下载并非你网站核心功能的一部分的代码可能会付出高昂代价。

#### 主线程执行成本

在浏览器主线程上解析和执行 JavaScript 会延迟交互性。正如你在第 7 章中所学，当浏览器遇到脚本时，默认情况下必须暂停解析 HTML 以执行脚本代码。第三方脚本通常在加载时运行大量的代码块，这可能会阻止你自己的页面渲染。即使以异步方式加载，大型脚本在运行时仍可能占用 CPU。这些“启动”时间会增加交互时间（TTI），并可能导致卡顿。web.dev 的分析显示，第三方脚本经常阻塞主线程，延迟用户交互。如果脚本执行繁重的工作（例如分析处理或构建小部件 UI），它可能会引入明显的延迟甚至帧丢失。

#### 资源竞争

第三方内容与第一方内容竞争带宽和 CPU。如果广告脚本开始进行大量网络调用或执行布局，它就会与你自己的图像和脚本竞争主线程（以及其他资源）。如果没有仔细的排序，第三方资源可能会消耗本可用于获取主应用脚本或 CSS 的重要图像或 API 数据的网络资源，从而可能延迟初始渲染或绘制。

#### 渲染阻塞

一些第三方嵌入，尤其是较旧的嵌入，使用过时的技术直接阻止页面渲染。某些广告或跟踪脚本仍在使用臭名昭著的 document.write()，它可以暂停解析并同步插入内容，导致性能风险。即使使用现代异步技术，第三方 iframe 在其服务器响应缓慢时仍可能延迟 window.onload 事件。换句话说，外部脚本可能“异步”加载，但由于浏览器对缓慢 iframe 加载的处理方式，仍可能导致加载事件或其他页面里程碑被阻塞。

当你包含多个第三方小部件时，还会产生累积影响。每个小部件可能会带来自己的微型框架或库。例如，你可能在自己的代码中包含了一个日期选择器库，然后 A/B 测试脚本又单独注入了其自己的类似库的副本——字节和工作量翻倍。可能最终多次加载 jQuery，或因两个嵌入各自获取自己的副本而加载两个不兼容的框架版本。这种重复浪费资源并加剧性能问题。

第三方代码也可能不可预测。由于每次都是从第三方服务器获取，它可能会在你不知情的情况下发生变化，或自行加载其他脚本（所谓的第四方脚本）。Andrew Welch 指出，通常，当你包含一个“标签”（实际上是第三方 JavaScript 的片段）时，它本质上可以在用户浏览器中随意执行，包括加载更多脚本。例如，标签管理器可能会加载五个标签，每个标签随后调用其他服务，导致请求数量激增。这种级联效应会以难以预见或监控的方式降低性能。

依赖外部服务器会带来风险，即如果其中一个服务器缓慢或宕机，你的页面可能会挂起或损坏。这就是所谓的单点*failure*（SPOF）问题。如果第三方服务器未响应或超时，它可能会阻止你网站的部分内容加载。在最坏的情况下，关键的第三方脚本（例如 CDN 托管的库）加载失败甚至可能停止你网站的功能。

第三方嵌入通常会异步地向你的页面注入内容（如大小不明的 iframe）。一个经典例子是广告在文章文本加载后出现，移动文本。这种变化会损害用户体验。如果你不提前为其预留空间，这可能会推动你的内容移动，导致布局偏移；事实上，许多第三方 iframe 是导致 CLS 较差的重要因素（第 2 章中介绍）。

虽然这本身不是性能问题，但值得注意的是，第三方脚本可以通过开放网站遭受潜在跨站脚本（XSS）攻击的风险来引入安全漏洞，以及通过跟踪用户或访问敏感数据而引发隐私问题。第三方标签不仅应从速度角度审查，还应符合隐私法规和安全要求。

总之，第三方脚本使网络更具动态性、交互性和互联性——但使用它们是有风险的。它们通常在网站所有者控制之外执行，并可能显著影响页面加载速度和稳定性。由于如此多的代码来自外部，性能优化不能忽视这一部分堆栈。

我记得一个 WebPageTest（WPT）新闻网站的跟踪显示，跟踪和营销脚本的加载时间比页面自己的内容更长。网络瀑布图显示第三方请求主导了时间线。图 8-1 显示了一个域分解图表的示例，其中来自第三方域的字节和请求占了很大比例。这种数据是一个警钟：如果不加以干预，第三方脚本可能会抢走你第一方内容的风头，让用户等待。

鉴于这些风险，网络性能专家将第三方脚本优化视为一项关键任务。在接下来的章节中，我将探讨如何分析第三方脚本的影响并优化其加载、执行和整体占用。




The actual website

Tracking scripts, tracking pixels, remarketing, etc.

Figure 8-1. WebPageTest's waterfall view can highlight the impact of heavy third-party script use

### Measuring Third-Party Scripts’ Performance Impact

Before you can optimize third-party scripts, you need to identify them and quantify their impact on your site’s performance. This involves using performance auditing tools and techniques to see what third-party code is present and how it affects loading metrics. You’re already familiar with tools like Chrome DevTools, Lighthouse, and WPT from Chapter 2. Here, I’ll focus on applying them specifically to third-party script analysis, highlighting what to look for and how to interpret the results.

### Inventory and Identification: Key Metrics and Indicators

The first step is to inventory all third-party scripts on a given page. This includes both scripts added directly into your HTML (like a `<script src="https://third-party.com/script.js">` tag) and those dynamically injected by other scripts or tag managers.

The key is to establish a baseline: know what third-party scripts are on the page, where they come from, and some measure of their cost in bytes, load time, execution time, and so on. When evaluating third-party scripts, metrics to pay attention to include:

- How many HTTP requests are coming from third-party domains? (Generally, more requests mean more overhead.)
- How much data is being transferred from third parties, in total KB/MB? This affects load on slow networks. If third parties dominate the bytes, that’s a red flag.
- How many milliseconds on the main thread (CPU) are spent executing third-party JavaScript? This directly affects TTI and responsiveness.
- Do third-party requests start after your main content, or are they interfering by starting too early? Ideally, critical content should

- Be particularly cautious with AI-generated code that integrates third-party services. As noted in Chapter 4, AI tools don’t inherently optimize for performance. If you’re using AI to generate integration code for third-party services, you must manually review it for performance best practices: check if the code uses async loading, uses proper error handling, and doesn’t block critical rendering paths.

When reviewing any third-party code (whether AI-generated or manually written), ensure it follows the optimization patterns discussed in later sections of this chapter.

### Lighthouse and Chrome DevTools

In Chrome DevTools, the Network panel is a great starting point: you can load your page and filter and group requests by domain. Any requests to domains that aren’t yours (or your CDN’s) are third-party, so they’re easy to spot.

Tools like PageSpeed Insights and Lighthouse (which is integrated into Chrome DevTools) offer automatic analyses that list third-party resources. For example, Lighthouse has an audit called “Reduce the impact of third-party code,” which enumerates third-party scripts, showing their sizes and main-thread execution times. It’s an easy way to get a quick list of all external script providers and how costly each one is. Other useful Lighthouse audits include:

- “Reduce JavaScript execution time” (previously “JavaScript boot-up time”)
- “Avoid enormous network payloads”
- “Eliminate render-blocking resources”
- “Reduce unused JavaScript”

Another way to identify third-party script execution in Chrome DevTools’ Performance panel is to record a performance trace while loading the page,

then group your profiling data by script origin. To do this, go to the “Bottom-Up” or “Call Tree” view, group by “Product.” Chrome will label groups like “Google Analytics” or “Facebook SDK” if it recognizes them. This tells you how much CPU time was spent executing script from each domain. Sorting by total time quickly shows which third-party scripts are consuming the most CPU during page load.

The Coverage tab in DevTools’ Sources panel shows how *much* of each script was executed. This can sometimes reveal that you loaded a big third-party library but that only a small portion of it actually ran.

You can also use DevTools throttling to mimic real-world conditions by turning on network and CPU throttling when measuring third-party impact. On a fast local machine with no throttling, a third-party script might seem to load innocuously in 100 ms. But on a typical midtier smartphone on slow 4G, that could be 500 ms or more plus slower execution. By testing under “Regular 4G” and a fourfold CPU slowdown (which approximates a low-end device), you’ll see what users really experience. Many third-party scripts (especially heavy analytics and visual widgets) have a disproportionate impact under constrained conditions, so it’s crucial to measure with throttling for an accurate picture.

The Performance panel recording can also reveal long tasks caused by third-party script execution. DevTools even supports a long task API in pages themselves: you can use PerformanceObserver to log long tasks in real time, which can include identifying the culprit frame or script. Some advanced RUM setups use this to track third-party slowdowns on real user devices.

### WebPageTest (WPT)

WPT is a powerful ally in understanding third-party impact. It can produce a handy waterfall chart showing the percentage of requests and bytes coming from each domain on your page. Figure 8-2 shows an example of such a chart. If you see that, say, 50% of the bytes are from *.doubleclick.net or *.facebook.com, you know that third-

Another valuable WPT feature is request blocking, which allows you to simulate the page load with certain domains blocked. A recommended workflow is this:

1. Run a test normally (no blocking) to get baseline load times.
2. Go into Advanced settings. In the “Block” field, list the hostnames of known third-party domains (like ad networks) to block.
3. Run a second test blocking those domains.
4. Compare the two results using WPT’s filmstrip and comparison features.

By doing this A/B test, you can quantify the impact of those third parties. For example, you might find that the page visually completes two seconds faster without the ad scripts. That delta is essentially the “performance tax” of those scripts. Andy Davies, a web performance expert, has demonstrated using WPT comparisons to measure third-party costs and found it very insightful for prioritizing what to optimize or remove.

Chrome DevTools lets you perform similar experiments: in the Network panel, you can right-click a request and choose “Block request domain.” This will prevent any request to that domain from loading, and you can then reload the page to see the effect. While this is a manual test (and won’t give you a filmstrip like WPT does), it’s a quick way to check during development: “What if I remove this tag? Does my load time improve dramatically?” Remember to disable caching in DevTools when doing this to simulate first-load conditions.

WPT also has a special SPOF simulation. Instead of outright blocking a domain, SPOF mode simulates what happens if that domain is slow or times out. This is useful because third-party problems often manifest as slow servers rather than complete failures. Using the SPOF tool, you can see, for instance, how your page behaves if the analytics server takes five seconds to respond. Does it delay the whole page? Does the page eventually load without it? This helps you assess risk and ensure you have proper timeouts and fallbacks.

By thoroughly auditing with the tools and methods covered in this section, you will gain a deep understanding of your site’s third-party landscape. Keep logs of these findings: they’ll inform the strategies you choose and help you track improvements as you begin optimizing.

### Principles of Third-Party Optimization

Before I dive into any specific techniques, it’s useful to establish some guiding principles and a mindset for optimizing third-party scripts. Unlike your own code, you can’t edit an analytics script to make it execute faster—it’s someone else’s code. So the kind of optimization you’ll do here revolves around how and when you load those scripts and how you manage their impact on the page.

When you’re dealing with a third-party script that hurts performance, you have two options, broadly speaking: you can get rid of it (by removing it or replacing it with a more efficient alternative), or you can optimize its loading and execution to minimize its interference with your page’s critical path. This includes deferring it, loading it asynchronously, conditionally loading it, and otherwise managing its execution timing and environment. A holistic strategy may involve a bit of both: maybe removing a few tags that aren’t needed and heavily optimizing the rest.

### Remove, Reduce, Replace

Now that you have a list of third-party inclusions and their costs, start with a critical look at each one. What does it do? Is it worth it? Is it truly needed? The most effective optimization is elimination. There’s no script that loads faster than the script you don’t load at all. So part of measuring impact is arming yourself with data to make the case: “This chat widget adds 300 KB and 0.5 s load, but only one in a thousand users clicks it. Perhaps we should load it differently or remove it.”

It’s easy for sites to accumulate third-party scripts over time (from various teams and experiments) and never remove them. Periodic audits often find



tags that no one in the organization owns or uses anymore. Eliminating those frees your site from unnecessary baggage. Consolidating and removing duplicates is also low-hanging fruit—you might, for example, find two analytics doing similar tracking because the marketing department added one and sales added another.

Guidance from web.dev suggests running A/B tests to balance each inclusion’s “perceived value versus its impact on key metrics.” You might experiment by removing a third-party script (or use a tool like DevTools or WPT to simulate removing it) to see if there’s any effect on user engagement or revenue. If not, maybe you can drop it permanently.

For scripts that provide some value, are there lighter alternatives? Perhaps you’re using a heavy third-party date-picker library for a certain form; could you use a native `<input type="date">` or a lighter-weight library instead? If an embed provides something fancy but not critical, maybe a simpler embed would do. Or maybe a one-time static integration could replace that live embed. For example, instead of loading a live Instagram feed through a script, maybe you could fetch and embed a few images at build time. (I’ll discuss this concept, called *static facades*, more in the next few chapters.)

As you’ve seen throughout this book, optimization is often about trade-offs. Weigh the value of each third-party script against its cost. A script that is crucial to revenue might justify a 200 ms delay, whereas one that’s marginal probably does not.

### Think in Performance Budgets

Adopting a performance budget mindset can be helpful. Set quantitative goals for third-party impact, such as “Third-party scripts should consume no more than one second of main-thread time on mobile” or “Total third-party bytes should total less than 500 KB.” Treat these as budgets. This approach turns optimization into a continuous effort: whenever a new script is proposed, you evaluate it against the budget. If adding it would exceed the budget, you must optimize elsewhere or reconsider the addition.

Many organizations include performance budgets in their CI/CD processes, for instance, by running the “Reduce third-party impact” Lighthouse audit in CI and failing builds if certain metrics regress beyond the budgeted threshold. This is effective at preventing new third-party bloat from slipping in unnoticed.

### Shifting Left: Involving Everyone

It’s crucial to understand that third-party optimization is as much about culture as technology. Often, the teams adding third-party tags (marketing, analytics, third-party vendors) are different from the performance engineers or core developers. So when deciding on what to remove and/or replace, involve your stakeholders. Marketing might be reluctant to remove a tracking pixel until you show that it slows the site without any demonstrable ROI. Here’s where data helps: present the performance impact (as you’ve measured it) alongside any business metrics. It could be worth running an A/B test measuring conversion or engagement with the script and without it. If removing the script has no negative impact on business metrics, that’s a win-win: you’ve sped up the site and removed complexity.

Fostering a culture where performance is a shared responsibility will ensure your optimizations persist. Chapter 11 will discuss this in detail.

### The Goal: Fast and Functional

Our goal is to have our cake and eat it too: preserve the useful functionality of third-party integrations while dramatically reducing their impact on performance. This often means loading them in smarter ways so that they don’t block or delay what the user cares about most: the primary content or interaction of your site.

In the next chapters, I’ll explore optimization patterns and techniques for third-party scripts, including:

- Explicitly controlling the loading sequence
- Asynchronous and deferred loading
- Lazy loading
- Click-to-load (import on interaction) patterns
- Conditional loading based on device or network conditions
- Scheduling loads during idle times or after initial load
- Offloading work to web workers or the server/edge
- Utilizing framework-specific optimizations
- Tuning tag managers

From there, I’ll turn to what it means to build a performance culture in your organization.

### Chapter 9. Loading Third-Party Scripts

Third-party scripts—from analytics platforms to advertising networks, social-media widgets to customer-support chatbots—are essential to modern websites, but they come with a cost. While Chapter 8 covered what third-party scripts are and why they matter, this chapter focuses on how to load them efficiently. The common thread throughout is sequencing: prioritizing what loads first, delaying what can wait, loading conditionally based on user context, and implementing click-to-load patterns that defer heavy resources until they’re actually needed.

Loading third-party scripts effectively requires balancing functionality with performance. Load them too early or carelessly, and they can block your critical content, slowing down the initial page render and frustrating users. Load them too late, and you might miss capturing important analytics data or delay features users expect. The techniques in this chapter—like using async and defer attributes and implementing lazy loading with facades—give you fine-grained control over when and how third-party content appears on your pages. By the end, you’ll understand how to sequence third-party loading to minimize their performance impact while maintaining the functionality your site needs.

### Sequencing and Prioritizing

One of the fundamental aspects of optimizing third-party scripts is controlling when they load and execute relative to your page’s critical content. AI-generated HTML often simply includes third-party <script> tags without consideration for when they load, which means the browser might load and run them at inconvenient times, potentially delaying other important tasks. You need to take manual control over sequencing to ensure that third-party code doesn’t interfere with the initial page render.

### Identify Critical Scripts

First, categorize your third-party scripts into critical and noncritical. *Critical* third-party scripts are those absolutely required for the page to function or render properly. These should be loaded early, albeit carefully. For example, if you use a third-party library to render part of your UI (say, a map component that uses an external JavaScript API), that might be critical to showing content above the fold.

Noncritical third-party scripts provide enhancements or tracking but are not needed for the page to work initially. This includes analytics, advertising, heatmaps, social widgets, and so on. The user experience is usually fine if these load a bit later or after the main interface is up. They should almost always be deferred or delayed until after the main content loads or becomes interactive.

A well-optimized site prioritizes the main content over third-party extras. As I’ve written for Web.dev, as a best practice, “the main content should load quickly and before any other supporting content.” For instance, on a news article page, the article text and images are critical first-party content; the social share buttons and ad frames are noncritical content that can load afterward.

### Positioning Script Tags

Where you place third-party script tags in your HTML matters a lot. By default, when the browser encounters a normal <script> tag without any special attributes, it will pause HTML parsing to download and execute that script. This means that if you put a third-party script in the <head> of your document, it can block everything that comes after from rendering until it’s done.

Here’s an example of what *not* to do:

### Nonblocking Scripts: async and defer

Using the `async` or `defer` attributes on script tags are the primary ways to load JavaScript without blocking the HTML parser. Let’s clarify these attributes:

### defer

With `defer`, the script is fetched in parallel while the document is parsing, but its execution is deferred until after the HTML parsing is complete. Deferred scripts also preserve execution order relative to each other: if you have two scripts with defer, they will execute in the order they appear in the document (after parsing).

### async

With `async`, the script is fetched in parallel and executed as soon as it’s downloaded and available, without waiting for HTML parsing to finish. Critically, async scripts do *not* guarantee execution order: two async scripts can finish and execute out of order if one downloads faster. When an async script executes, it can still pause the parser, but at least downloading it doesn’t block the parser initially. In modern browsers, async works a bit differently for module scripts, which will defer execution to maintain order among dependencies. For classic scripts, though, think of async as “download in the background, execute ASAP.”

For third-party scripts, I generally recommend using `defer` by default for noncritical scripts (any script that is not needed before the DOM is ready). This ensures it won’t interfere with HTML parsing and will run after the document is done, which is usually what you want for things like analytics or ads. In contrast, `async` is useful for scripts that you want to load in parallel and run as soon as possible but that aren’t dependent on other scripts. For example, if you have an early analytics snippet that you want to start recording data immediately, you could use `async` so that it fires




### 尽早使用资源提示建立连接

顺序安排不仅仅是脚本标签放在哪里；它还涉及为浏览器准备第三方加载。如果您知道某个第三方域名将被需要，可以在 HTML 中使用资源提示，例如 <link rel="dns-prefetch" href="..."> 和 <link rel="preconnect" href="...">。这些提示会提前建立到第三方服务器的网络连接，这样当您稍后发起脚本获取时，部分网络握手已经完成。

例如，您可能会添加到 head 中：

```
<link rel="preconnect" href="https://example-third-party.com">
```

这告诉浏览器尽早开始对 *example-third-party.com* 进行 DNS 查找、TCP 连接和 TLS 协商。到您实际从该域名加载脚本时，它可以更快地获取字节。较弱的提示 `dns-prefetch` 仅提前进行 DNS 查找；这对于旧浏览器或不太关键的域名很有用。

此技术是顺序优化的一部分，因为它关乎网络操作的排序：您本质上是将一些工作提前到时间线更早的位置（启动时的空闲时间），这样当实际脚本请求到来时速度会更快。

Andy Davies 报告了一个真实案例，使用 `preconnect` 用于图像 CDN 使中位加载时间改善了 400 ms，95 分位改善超过 1 s。虽然该示例针对的是图像，但同样的思路适用于第三方脚本：预连接您的分析域名可以为用户到该服务器的网络延迟节省几百毫秒。

在规划顺序时，请记住为您知道会调用的关键第三方源添加预连接。这样，即使在加载完整脚本之前，您也能获得提前开始的好处。

### 总结顺序最佳实践

总之，第三方脚本顺序的最佳实践包括：

- *优先加载第一方内容*

始终在非关键第三方脚本之前加载您网站的核心内容和关键资源。相应地构建 HTML。

- *将脚本置于末尾*

将第三方脚本标签放在 body 底部，或在内容之后动态加载，除非绝对有必要另行处理。

- *使用 async 和 defer*

将第三方脚本标记为 `async` 或 `defer`，以免阻塞解析器。大多数情况下使用 `defer`，尤其是在有多个脚本或脚本必须在 DOM 就绪后运行的情况下。如果顺序很重要，请依赖 `defer`（或通过回调管理加载）。避免依赖 `async` 的随机顺序。

- *尽早加载关键第三方脚本*

对于少数关键的第三方脚本，仍应异步加载，但在需要时尽早加载（在 head 中延迟）。问问它是否真的 *必须* 在 head 中加载。

- *如果可能则预连接*

使用 `<link rel="preconnect">` 为关键第三方域名减少连接设置时间。

- *一次一个（也许）*

不要一次性向页面注入十几个第三方加载，尤其是在早期。即使是异步脚本，如果同时开始下载太多，也会争夺带宽。尽可能通过优先级或稍后启动来错开它们。

接下来，我将探讨在适当时进一步延迟第三方加载的技术。

### 延迟加载第三方内容

正如您在第 4 章中学到的，*延迟加载* 是一种技术，用于将资源加载延迟到实际需要它的时刻。它通常用于仅在用户向下滚动时加载图像，但同样的概念也可以应用于第三方脚本和嵌入。通过延迟加载第三方内容，您减少了浏览器前期需要做的工作，从而实现更快的初始加载时间。那些从不交互或滚动到某些功能的用户的成本可能永远不会产生，从而节省数据和时间。

考虑一个典型场景：您在侧边栏或页面更下方有社交媒体小部件或视频嵌入，在初始加载时不可见（“折叠以下”）。在用户滚动到它们或交互之前，没有必要获取和渲染它们。立即加载它们会减慢每个用户的页面速度，即使他们可能永远不会滚动到小部件。延迟加载此类内容可优化初始加载，并减少未与该页面部分互动的用户的不必要下载。

延迟加载第三方脚本可以带来实质性的性能改进，包括：通过不提前加载重量级脚本来减少初始页面重量；加快首次渲染和交互时间，因为浏览器初始需要做的工作更少；以及降低未超出初始内容互动的用户的数据使用量。

例如，YouTube 的 iframe 嵌入以重量大而闻名。只需向折叠以下的 YouTube 嵌入添加 `loading="lazy"`，即可在初始页面加载时节省大约 500 KB。这非常巨大，尤其是在移动设备上。同样，如果您有三个总计 1 MB 脚本的社交嵌入，延迟加载它们意味着不滚动的用户将加载 1 MB 更少的数据，这对于使用有限数据套餐的用户很重要。

### iframe 的浏览器原生延迟加载

许多第三方嵌入，如 YouTube 视频和地图，都是在 iframe 中。最简单的方式是使用原生 loading="lazy" 属性在 <iframe> 标签上，就像您在 <img> 上添加 loading="lazy" 一样。例如：

```
<iframe src="https://www.youtube.com/embed/VIDEO_ID"

width="560" height="315"

loading="lazy"

title="YouTube video player"

frameborder="0" allowfullscreen></iframe>
```

所有现代浏览器都通过此属性支持 iframe 延迟加载。浏览器将推迟加载 iframe 的内容，直到它接近视口。确切的距离阈值因浏览器而异，但以屏幕高度为单位衡量。好的一点是，浏览器会自动将这一行代码作为提示处理。

在延迟加载 iframe 时，一个重要的考虑是为内容预留空间。如果您未指定其宽度和高度（或 CSS 尺寸），当 iframe 最终加载时，它可能会导致布局偏移，损害 CLS，因为浏览器最初未为其分配空间。始终为延迟加载的 iframe 设置明确的尺寸属性或 CSS。

浏览器原生延迟加载是完全被动的——您不需要 JavaScript 来处理它。这对于简单性和可靠性来说很棒。然而，有一些注意事项：

- 有些浏览器会保守加载以避免内容过晚弹出。如果 iframe 刚好在折叠以下，一个浏览器可能会急切加载，而另一个可能会等到用户滚动。
- 不支持延迟加载的旧浏览器会忽略此代码并立即按常规定加载 iframe。不会造成伤害；它们只是不会从此优化中受益。随着浏览器支持变得更加广泛，这已不再是问题。
- 如果您需要更精确的控制，也许在特定阈值或条件下加载，您可能需要基于脚本的延迟加载解决方案。

### 使用 IntersectionObserver 自定义延迟加载

为了获得更多控制，或延迟加载图像/iframe 之外的元素（如脚本或任何元素），IntersectionObserver API 是首选解决方案。它可以在元素进入或退出视口（或其定义的阈值）时通知您。

此技术让您可以完全控制何时加载脚本（在此情况下）。以下是通过 IntersectionObserver 延迟加载的典型模式：

- 1. 在 HTML 中放置一个空的容器 div 或轻量级占位符元素，第三方内容将放置在该位置。
- 2. 在脚本中设置 IntersectionObserver 来监视该占位符。
- 3. 当占位符进入视口时（即观察者回调触发），浏览器将注入第三方内容的 <script> 或 <iframe> 元素，然后停止观察该元素。

以下是当占位符至少 10% 可见时加载脚本的纯 JavaScript 示例：

```
const placeholder = document.getElementById('social-feed-placeholder');

if ('IntersectionObserver' in window) {

  let observer = new IntersectionObserver(entries => {
```





```
entries.forEach(entry => {
    if (entry.isIntersecting) {
        // Placeholder is now visible or near-visible, load the actual content
        loadSocialFeed();  // e.g., replace placeholder innerHTML
                          // with embed code or append script
        observer.unobserve(entry.target);
    }
});
}, { root: null, threshold: 0.1 });

observer.observe(placeholder);
} else {
    // Fallback for older browsers: just load immediately or after a timeout
    loadSocialFeed();
}
```

`loadSocialFeed()` 函数将包含创建 iframe 或 script 标签以加载实际第三方内容的逻辑。如果你有多个项目，比如视频嵌入的列表，它会观察它们全部，并在每个项目进入视口时加载。Facebook 的 SDK 在其嵌入代码上支持 `data-lazy="true"` 属性以启用内部懒加载。如果存在此类选项，请使用它：它本质上是告诉第三方脚本本身延迟工作。

由于编写 IntersectionObserver 逻辑可能很重复，lazysizes 库对其进行了抽象，在现代浏览器中底层使用 IntersectionObserver。你可以使用它通过某些类和数据属性自动懒加载图片和 iframe。它提供一致的阈值和行为，而原生加载在不同浏览器中可能会略有差异。

以下是之前的 iframe 示例，这次使用 `lazysizes`：

```
<script src="lazysizes.min.js" async></script>
<iframe data-src="https://www.youtube.com/embed/VIDEO_ID" width="560" height="315" class="lazyload" frameborder="0" allowfullscreen></iframe>
```

这里，你把真实 URL 放在 `data-src` 中，`lazysizes` 会在需要时将其替换为 `src`。类 `lazyload` 指示 `lazysizes` 跟踪它。这样可以避免你自己编写 IntersectionObserver 逻辑，并确保即使是非常旧的、没有 IntersectionObserver 的浏览器也能获得某种行为（`lazysizes` 会回退到滚动事件检查）。要调整这些阈值和条件，请使用 IntersectionObserver 中的选项或 `lazysizes` 中的数据属性。

### Trade-Offs

虽然懒加载很棒，但要注意其潜在的缺点。包括：

- User experience

如果懒加载实现不当，用户在滚动时可能会注意到内容出现较晚。理想情况下，它应该在用户到达时或稍早前加载。设置 IntersectionObserver 中的合理阈值有助于此。

- SEO

懒加载对 SEO 很重要内容（文本内容或某些图片）如果搜索引擎看不到它，可能会有问题。通常，对于纯粹的第三方内容且不是主要文本来说是可以的。Google Search 可以执行部分 JavaScript，但最好不要懒加载关键的文本内容。仅对非主要内容使用这些技术，以确保关键内容仍可被索引。

### Analytics impact

如果你使用基于滚动的懒加载，你的分析可能仍需要考虑懒加载组件实际出现或被查看的时间，如果这很重要的话。一些分析工具不会自动知道稍后加载的内容。

### Browser differences

如前所述，不同的有效连接类型可能会导致不同的行为。Chrome 的原生懒加载在快速连接上可能会比在慢速 3G 上更早加载。如果一致性是关键，请使用 IntersectionObserver（配合你自己的逻辑）或 `lazysizes` 来使行为在各种条件下保持一致。

### Example: Lazy Loading an Analytics Script

到目前为止，我谈论的是懒加载可见小部件。但在某些情况下，你甚至可以懒加载非视觉脚本，如分析。如果你网站的主要内容对性能非常敏感，你可能会选择懒加载 Google Analytics——意味着你不会在主内容显示后或甚至在用户交互前加载它（由于 Google Analytics 如果加载过晚可能会错过初始页面浏览时间，技术上必须权衡利弊）。

一些人使用的一种模式是以 `defer` 加载分析（以便在解析后加载），或甚至等待 `load` 事件（使用 `window.onload = function() { loadAnalytics() }`）。这是一种按事件的懒加载形式。它确保分析脚本不会干扰页面加载，尽管它可能会错过捕获一些早期数据，例如页面的加载时间。许多现代分析实际上可以延迟加载而不会有问题，但这是需要考虑的事情。

### Facades and Click-to-Load Patterns

即使有懒加载，一些第三方内容在加载后仍然可能很耗资源。Facade 将懒加载更进一步，通过提供一个轻量级的占位符来代替重量级的第三方嵌入，只有在用户交互时才加载真正的內容。这种方法也被称为“点击加载”或“交互时导入”。其思路是，除非用户明确请求，否则根本不加载第三方内容（通常是通过点击占位符）。

在此上下文中，*facade* 是一个静态或更简单的元素，模仿更丰富、交互式嵌入的外观。它为用户提供视觉提示或部分功能，但实际上不包含完整的第三方内容。由于它是静态的，因此更轻量，没有明显的性能开销。例如，你可以显示视频的静态图片预览（缩略图上叠加播放按钮）作为占位符。视频播放器脚本只有在用户点击时才加载。

在简单的懒加载中，内容在滚动触发前为空，然后加载实际内容。使用 facade 的懒加载可以进一步改善感知性能，因为用户至少能看到一些东西而不是空白。

使用 facade 方法，浏览器会显示一些轻量级的占位内容，看起来像或暗示真实内容，但只有在交互（点击或悬停）时才加载真实内容。这对于视频播放器、地图和聊天小部件等重量级交互式嵌入尤其有用，因为用户可能根本不会与之交互。Facade 迎合了那些不会交互的用户，为他们提供非交互式预览，速度更快，只有在真正交互时才启用完整功能。

### Implementing Click-to-Load

点击加载模式通常涉及三个阶段：

- Page load

这是 facade 渲染的时候。此时不会加载任何第三方脚本或 iframe，只有你自己的轻量级占位元素。

### Preconnect on hover

可选地，如果你想更流畅，当用户悬停或触摸 facade 以表示兴趣时，你可以发起对第三方域的预连接。这会预先建立网络连接，以便如果他们点击，实际加载会更快。这提高了感知速度。

### Load on-click (or interaction)

当用户点击 facade 时，动态加载第三方内容。这可能意味着插入一个 <script> 标签，创建带有真实 src 的 <iframe>，或以其他方式初始化嵌入。Facade 然后被真正的內容替换，用户现在可以与之交互。

例如，对于 YouTube 视频，在页面加载阶段，页面显示 <img src="thumbnail.jpg"> 并带有播放按钮覆盖层（CSS 或小图片）。悬停时，它会使用 <link rel="preconnect" href="https://www.youtube.com"> 建立与 YouTube 的预连接，甚至以隐藏方式开始加载 YouTube iframe。当用户点击时，浏览器会用实际视频替换 <img>：<iframe src="https://www.youtube.com/embed/VIDEOID? autoplay=1">。视频随后加载并（如果启用了自动播放）开始播放。

生成式 AI 驱动的客户支持聊天机器人，如 Intercom、Zendesk 或专业 AI 聊天提供商的产品，在实施点击加载模式时值得特别关注。这些工具因提供即时客户支持而变得越来越流行，但它们带来了显著的性能成本。AI 聊天机器人通常需要：

- 加载用于自然语言处理界面的大量 JavaScript 框架
- 建立用于实时通信的 WebSocket 或长轮询连接
- 加载额外的资源，如头像、输入指示器和建议界面
- 有时预加载模型数据或对话上下文

这很容易使初始页面加载增加 500 KB、1 MB 或更多，加上持续的网络活动。对于客户支持聊天机器人，使用 facade 模式特别有效，因为：

- Most visitors won’t need support

通常只有 1% 到 5% 的访客会真正使用支持聊天。为所有人加载完整的聊天小部件会浪费 95% 以上从不使用它的用户的资源。

- Support is user-initiated

与需要尽早加载以捕获页面浏览量的分析不同，用户在需要帮助时会明确寻求聊天。点击后的一点延迟是可以接受的，也是预期的。

- Alternative contact methods exist

你可以将轻量级的“需要帮助？”按钮作为 facade 显示，点击后打开聊天小部件。如果小部件加载失败，你可以提供备用电子邮件或联系表单链接。

这里是一个典型的实现方法：

```
// Show a lightweight chat button

const chatButton = document.getElementById('chat-button');

chatButton.addEventListener('click', async () => {
```




+   - 性能大幅提升，因为如果用户没有交互，你根本不会加载大型脚本。这可以节省数秒的加载时间。
+   - 用户会看到一个占位符（可能是预览图片或图标），这表明他们可以按需加载内容，比留空要好。
+   - 用户本质上是在选择更重的体验。
+   - 尤其是移动用户受益，因为除非他们想要，否则不会下载媒体密集型内容。

缺点包括：

+   - 用户必须执行一个操作（额外点击）才能获取内容，在某些场景下这可能并不符合预期。对于视频，用户习惯点击播放，但如果你用“加载 Instagram 动态”按钮替换了自动加载的 Instagram 动态 feed，有些人可能不会费心或意识到。
+   - 实现门面比直接嵌入要多花功夫。你必须设计占位符并编写脚本来在交互时处理加载，增加复杂性。
+   - 静态图片地图在点击并加载真实内容或跳转到外部站点之前不允许缩放。这通常是可以接受的，但要考虑是否会丢失或延迟任何核心功能。

如果第三方嵌入对页面至关重要（例如视频流站点页面上的主要视频），你可能不想要门面：毕竟用户就是冲着该内容来的。在这种情况下，更好的策略可能是尽可能优化该嵌入，如果折线上方本来就是缩略图，也许只做懒加载。门面在第三方内容为补充内容时效果最佳。

接下来，我将研究控制的另一个维度：根据环境定制第三方加载——具体来说是设备能力和网络

可以用来衡量设备能力：CPU 核心数为一到两个的设备可能比八核设备更弱。然而，核心数并不是性能的完美衡量标准——CPU 架构和其他因素也很重要。

### Client Hints

Chrome Lite 模式中用户可以启用一个名为“Save-Data”的 Client Hint 头部。如果开启，这是一个强烈的信号表明用户希望节省数据，因此你可以积极削减第三方脚本。Client Hints 可以在你请求时提供设备信息。

### Device Memory API

API navigator.deviceMemory 给出用户 RAM 的近似值。如果 deviceMemory < 2（意味着设备 RAM 少于 2 GB），你就知道应将其视为低端设备并跳过部分脚本。示例如下：

```
if ('deviceMemory' in navigator && navigator.deviceMemory < 2) {
    // Don't load heavy third-party
} else {
    // Load as normal
}
```

一个实际用例：假设你有一个复杂的聊天小部件，已知会拖慢低端手机。你可以在内存低于某个阈值的手机上有条件地不加载它，可能为这些用户显示一个简单的“联系我们”链接代替（渐进增强方法）。是的，少数用户可能会错过实时聊天，但他们在自己的设备上本来也不会有很好的体验，提供替代的联系方式比让他们使用缓慢的站点更好。

### Network-Aware Loading

浏览器的 Network Information API（navigator.connection）提供有关用户连接类型和有效带宽的信息。例如，navigator.connection.effectiveType 可能返回诸如 '5g'、'4g'、'3g'、'2g'、'slow-2g' 等值，基于吞吐量和 RTT 估计。而 navigator.connection.saveData 指示用户是否已在浏览器中明确启用了“数据节省”模式。Network Information API 并非在所有浏览器中都可用（Safari 不支持），因此应将其作为渐进增强使用。

你可以使用这些信息来做出决策。以下代码告诉浏览器，如果 effectiveType 为 '2g' 或 'slow-2g'（非常慢的连接），可能根本不要加载重量级的第三方脚本，或至少推迟到绝对需要时再加载。如果 saveData 为 true，则要非常保守并禁用所有非关键的第三方脚本，只加载绝对必要的脚本）：

```
if ('connection' in navigator) {
  const conn = navigator.connection;
  if (conn.saveData || (conn.effectiveType || '').includes('2g')) {
    // Save-Data is on or it's a 2G connection, avoid large third-party scripts
    window.__enableMinimalMode = true;
  }
}
if (!window.__enableMinimalMode) {
  loadThirdPartyScripts();
}
```

在此片段中，loadThirdPartyScripts() 函数可能会动态加载广告或大型分析脚本，但在非常慢的连接或数据节省用户的情况下可以跳过。

即使没有 Network Information API，你也可以近似一些信息，尽管这种间接方法并不理想。如果初始页面加载花费很长时间（你可以通过 TTFB 来衡量），这可能暗示网络缓慢。或者，你可以为用户提供手动切换，例如“基础模式”，但这会增加 UI 复杂性。

### Trade-Offs

与本文讨论的所有技术一样，条件加载也有其权衡。以下是其优点：

+   - 对于网络较慢或设备性能较弱的用户，体验更好，因为他们获得更轻、更快的页面。
+   - 对于带宽有限或按量计费计划的用户，可提供数据节省。
+   - Core Web Vitals 指标在不同用户群体中得到改善。
+   - 你仍可提供替代的联系方式或功能的简化版本，确保没有用户被完全排除在外。

缺点包括：

+   - 很难完美检测能力。一些中端手机可能仍然会挣扎；一些快速连接具有高延迟；但即使 effectiveType 显示为 '4g'，网络也可能拥塞。因此你的规则不会完美。目标是覆盖明显的极端情况（非常慢、非常低端），对其他情况保持适度。
+   - 测试可能很复杂，因为你本质上为不同用户创建了多个可能的代码路径。进行测试很重要

### Chapter 10. Scheduling and Optimizing Third-Party Scripts (and AI’s Role)

即使在决定加载哪些第三方脚本以及何时加载之后，仍有一个额外的优化层，即何时执行某些工作。浏览器有能力在空闲期间或主加载完成后安排任务，这有助于处理不紧急的第三方任务。这种方法可确保非关键的第三方代码在不会干扰响应性或渲染时运行。

### Scheduling Third-Party Scripts

在浏览器中安排非紧急工作的主要工具是 window.requestIdleCallback。

### The requestIdleCallback API

此 API 允许你注册一个函数，该函数应在浏览器事件循环空闲时运行（当它没有更紧急的工作如布局、绘制或用户输入处理时）。浏览器会尝试在空闲间隙执行你的回调。你还可以提供超时以确保即使空闲时间稀缺也能最终运行。

以下是如何使用 window.requestIdleCallback：

```
if ('requestIdleCallback' in window) {
  requestIdleCallback(() => {
    // Execute non-critical third-party init here
```

```
loadThirdPartyAnalytics();
}, { timeout: 3000 });
} else {
    // Fallback: if no support, run after a delay
    setTimeout(() => loadThirdPartyAnalytics(), 2000);
}
```

此片段安排 loadThirdPartyAnalytics() 在浏览器空闲时或最多三秒后运行。在快速设备上，它可能很快运行（页面处理完成后）；在繁忙设备上，它可能会等待一段时间，但肯定会在三秒标记前运行。这对于初始化分析或小部件等真正不需要立即执行的任务非常好。例如，也许你已推迟加载分析脚本文件，但即使如此，它也会执行并开始工作（例如发送页面数据）。你可以将该初始化包装在 requestIdleCallback 中，以确保它仅在主线程空闲时才开始发送数据。

注意：requestIdleCallback 并非在所有浏览器中都可用（Safari 在 18.2 版本中添加了支持，于 2024 年 12 月发布，但旧版本仍缺乏支持）。这就是为什么通常会回退到 setTimeout。回退可确保代码最终会运行，但会稍作延迟以避免阻塞关键任务。

### After Onload or First Interaction

更简单的方法是等待页面的“load”事件。window.onload 事件在所有资源（如图片和脚本）加载完成时触发。通过将第三方初始化推迟到此事件，你可以确保它不会与加载关键资源竞争。例如：


itself after a delay (though Google’s official snippet doesn’t do that).
There’s a balance, since delaying GTM might delay important tags (like analytics or ads firing). But if certain tags aren’t urgent, you could incorporate an idle strategy.

One idea is to put critical tags in one container that loads normally and heavy noncritical tags in another container loaded via a delayed mechanism. But GTM doesn’t directly support requestIdleCallback in its interface; this would be a custom integration.

### Using setTimeout as a Fallback or Simpler Option

If requestIdleCallback feels too fancy or isn’t supported widely, a crude but sometimes effective method is to use setTimeout with a few seconds’ delay:

```
setTimeout(() => {
  loadExtraThirdParties();
}, 5000);
```

This just waits five seconds after the page starts to load something. The assumption is that, by five seconds, the page is likely settled. (On fast connections, it may be done at two seconds; on slower ones, it may still be in progress, but hopefully at least the critical content has loaded.) It’s not perfect, but ensures the initial experience isn’t marred by those extras.

When scheduling for idle or after load, monitor to see whether those tasks ever interfere. If a user is on a slow device, the CPU is churning, maybe even after page load. Using requestIdleCallback provides a deadline to your callback about how much idle time the system has, which you can use for chunked processing. If idle time never happens (some pages are continuously busy, such as with an animation), consider a fallback strategy where these tasks run after a maximum time, to avoid starvation—meaning that some tasks never run because the browser never becomes idle (as shown with the timeout option).

The *Telegraph case study* did something conceptually similar. Those developers deferred all third-party scripts and found that not only did it not harm metrics, but it improved the time the first ad appeared by four seconds on average. That’s an example where deferring helped both performance and business metrics: the ads still got loaded, but just faster, since the page overall loaded faster.

By scheduling noncritical third-party work for idle periods, you ensure that critical interactions, like the user trying to scroll or click during initial load, aren’t forced to compete with those scripts. The user perceives the page as more responsive, and the third-party scripts still eventually run.

Now that I’ve covered strategies for when and whether to load third-party scripts, the next question is where they execute. Can you offload their work away from the main thread or even to other environments like web workers or servers?

### Offloading and Sandboxing Third-Party Scripts

One of the more advanced approaches to mitigating third-party scripts’ impact is to run them in a different context where they won’t interfere with your page’s main thread as much. *Sandboxing* refers to isolating third-party code in a restricted environment (like an iframe or web worker) to limit its access to your main application and prevent it from negatively affecting page performance or security. This can involve using web workers to execute scripts off the main thread, or even server-side or edge processing to handle work that otherwise would happen in the browser. Additionally, sandboxing third-party scripts in iframes can contain their effects.

### Web Workers and Partytown

Web workers allow JavaScript to run in a background thread separate from the main UI thread. Normally, third-party scripts interact with the DOM. That isn’t directly possible from a plain web worker, since workers don’t have access to the DOM API. However, an innovative library called Partytown (by Builder.io) addresses this by running third-party scripts in a worker and proxying any needed interactions back to the main thread when necessary.

Partytown’s philosophy is that the main thread should be reserved for your first-party code, and that all those heavy third-party scripts like analytics and trackers can be moved to a worker so they don’t block the main thread. It intercepts access to globals like window and delegates them.

To use Partytown, you self-host the Partytown library and then mark your third-party script includes with type="text/partytown" instead of "text/javascript". Partytown will load those in the worker environment. For example:

```
<!-- Partytown initialization in head -->
<script src="/-partytown/partytown.js" defer></script>

<!-- Third-party script configured for Partytown -->
<script type="text/partytown"
    src="https://third-party.com/analytics.js"
    data-partytown-config></script>
```

Under the hood, Partytown sets up proxies so that if the third-party script tries to do document.cookie or other main-thread actions, it will forward those calls in a controlled manner to the main thread. The heavy lifting, like crunching data and preparing requests, stays in the worker thread.

It’s a clever concept: third-party code often doesn’t need to alter your page synchronously; it mostly logs data or inserts some element eventually. (Ads might be an exception, where they add iframes, but even that can be coordinated.) If successful, this means that third-party scripts no longer jank the main thread. Your page stays responsive because the busy work is in a worker. It’s a sandbox, too, so any crashes or slowdowns are isolated.

Of course, there are trade-offs: Partytown is actively maintained and has matured since its initial release. Not all scripts will work perfectly with it, especially if they require immediate DOM access or assume certain synchronous behaviors. It also requires you to host the Partytown scripts and set up that integration, which isn’t trivial. There’s also a slight overhead in proxying calls between the worker and main thread. The library provides React components to integrate with frameworks, like a <Partytown/> component to include in Next.js heads, and specific components for GTM. Because it’s new, use it experimentally: test thoroughly and be prepared for occasional issues. It might not support every API a third-party script uses, though it covers cookies, localStorage, and so on via proxies.

Partytown is most appealing if you have a lot of third-party scripts that heavily tax the main thread, like multiple analytics or A/B testing libraries. If those can run in a worker, it frees up the main thread significantly. It’s one of those “next-gen” approaches that, if it becomes robust, could change how we include third-party code.

### Sandboxing in Iframes

A simpler, older method is to sandbox third-party scripts in an <iframe> so they don’t directly affect your page. For example, some sites put large third-party bundles in an invisible iframe (like an ad iframe). The advantage is that whatever happens in that iframe, such as long tasks or reflows, might not directly interfere with your main page’s scripting. If it hogs CPU, however, it still affects overall device performance.

Accelerated Mobile Pages (AMP), a framework by Google designed to create fast-loading mobile web pages, used this idea by loading all third-party JavaScript inside sandboxed iframes, freeing the main page from their direct influence. However, managing communication between the iframe and main page, if needed, can be complex. Also, an iframe still runs on the main thread—just with an isolated scope—so CPU usage isn’t offloaded; it’s just that blocking operations might not block the parent document’s parsing.

your server periodically, cache it, and output the content in HTML. That offloads the work to the server side and avoids the user’s browser pulling in X’s widgets script). Real-time interactivity, like buttons to repost, might not work in a static render, but often that’s acceptable.

### SERVICE WORKERS FOR CACHING

Using a service worker doesn’t offload execution, but it can offload repeated network costs. Patterns.dev suggests using service workers to intercept and cache frequently changing third-party scripts. For instance, if a third-party script isn’t well cached by the provider, you could cache it in the service worker so that subsequent page loads get it from cache instantly and can maybe even inject it early. This doesn’t reduce the first load cost but helps on repeat visits. You could also use service workers to delay or schedule third-party loads after certain events by intercepting requests and responding when convenient (though that can get complicated; it is easier to control via normal code, as we have discussed).

Server-side solutions often require more engineering effort and coordination with backend systems. Not every third-party can be moved server side either: for example, Google Analytics relies on the client side to measure things like page timings and user interactions. However, the cultural shift is interesting: for a long time, developers accepted that “third party” meant “runs in client.” Now tools like Partytown challenge that, showing we can reimpose control and decide where code runs.

Adopting these strategies might also require convincing stakeholders. For instance, to perform server-side experiments, product teams need to move logic out of marketing tools and into engineering’s domain—it’s often a collaboration between frontend and backend teams.

Next, I’ll discuss how modern frameworks and tools support some of these patterns out of the box, making it easier to implement best practices without reinventing the wheel each time.




``` 
gtag('js', new Date());

gtag('config', 'GA_ID');`}
</Script>

{/* Chat widget script, lazy load */}

<Script
    src="https://example.com/chat.js"
    strategy="lazyOnload"
    />
    </>
);
}
```

在此片段中，Google Analytics 被设置为 afterInteractive，因此在 Next.js 水合完成后会尽快加载。假设的聊天脚本被设置为 lazyOnload，因此它甚至要等到其他所有内容完成后再开始加载——类似于 window.onload 的时间。

在底层，Next.js 确保这些脚本以适当的方式和顺序注入（参见 Next.js Script 文档）。例如，所有 afterInteractive 脚本都表现得像延迟脚本（它们不会阻塞内容），并且通过此组件添加的任何脚本默认都是延迟的，除非 Next.js 被告知否则（这是一个安全的默认值）。使用此组件可以避免手动 DOM 操作或编写 effect hooks 来注入脚本。它还可以防止常见的错误，例如忘记 async/defer 或意外阻塞主线程。

此外，<Script> 有助于协调顺序。如果你将一个脚本标记为 beforeInteractive，另一个标记为 afterInteractive，Next.js 就知道哪个先执行。它还会自动执行一些操作。例如，如果多个脚本都设置为 afterInteractive，它们实际上都会使用 defer 加载以保持其顺序不变。并且如果你不小心

### 按站点类型优化

管理第三方脚本的策略通常需要根据网站架构和平台的特定特征和限制进行定制。不同类型的网站在处理第三方脚本时面临不同的挑战。本节讨论针对几种常见场景的定制策略：单页应用（SPA）和现代 JavaScript 应用、由内容管理系统（CMS）驱动的 WordPress 和其他内容站点，以及电子商务平台（如 Shopify）。每个都有独特的限制和优化机会。

### 在 SPA 与多页应用（MPA）中处理第三方脚本

使用 React、Angular 和 Vue 等框架构建的 SPA 通常提供丰富的客户端体验。它们可能依赖第三方 SDK 来提供身份验证（Auth0）、分析（Segment、Google Analytics）和媒体（视频播放器）等服务。

SPA 有一些挑战。例如，由于初始加载已经包含框架包，如果处理不当，在其上添加第三方脚本可能会使初始加载变差。由于导航是客户端进行的，正常情况下在每次页面加载时触发的第三方脚本可能需要在路由更改时手动处理。这可能意味着在路由更改时手动触发分析中的页面浏览，因为没有完整的页面重新加载。内存也可能成为问题：一个为不同部分和功能积累了许多脚本的 SPA，即使之后很少使用，也可能将它们保留在内存中。

SPA 的另一个陷阱是，如果第三方脚本运行过早，它可能会在框架附加事件监听器之前操作 DOM，导致这些更改丢失或被覆盖。这就是为什么 Next.js 的 beforeInteractive 被精心设计以与 SSR 集成（它实际上在服务器上将脚本注入 HTML 输出）。如果你自己实现，请注意：如果在框架水合之前注入脚本，请确保它们不会冲突。加载分析通常是可以的：它们除了添加自己的元素外不会过多地干扰 DOM，而框架会忽略这些元素，如果它们位于根元素之外（框架挂载的 DOM 元素，如 <div id="root">）或使用不冲突的 ID。

SPA——加载一次并通过 JavaScript 动态更新内容——与 MPA——每次导航时从服务器加载新页面——有略微不同的挑战。在 SPA 中，在没有完整页面重新加载的情况下在“页面”（视图）之间导航时，你可能需要管理为某些视图添加和删除第三方脚本。例如，营销嵌入可能仅在主页上需要。在 MPA 中，你只会在主页上包含嵌入的脚本标签。在 SPA 中，如果用户进入主页，你可能需要动态加载该脚本，并在离开时可能卸载它。卸载 JavaScript 很棘手，但你基本上可以移除事件监听器或元素，如果可能的话。

幸运的是，有一些基于框架的解决方案。在 SPA 中，Next.js 仍会根据策略加载每个页面的脚本，即使在客户端转换期间也是如此。但在客户端转换后，新页面的 beforeInteractive 脚本无法在水合之前追溯运行，而水合发生在应用启动时。Next.js 通过提前加载关键内容或通过设计将此类使用限制为初始加载来解决这个问题。

如果你使用 React Router 或类似工具，你可能会在页面组件中使用 effect。例如：

```
useEffect(() => {
  if (!window.thirdPartyLoaded) { /* load it */ }
  return () => { maybe cleanup }
}, [])
```

一些框架，如 Qwik，旨在暂停和恢复逻辑并以不同方式处理第三方。Qwik 实际上默认集成了 Partytown 用于第三方脚本。然而，在纯 vanilla SPA 中，你必须小心不要重复加载脚本。如果脚本需要在全局使用，请考虑在应用启动时一次性加载它（使用 async），以便它可用。

SPA 的一个良好实践是将第三方集成封装在服务或自定义 hook 中，如 `useChatWidget()`，以确保它加载并且必要的初始化只发生一次。例如，你的组件可以在需要时调用 `useChatWidget()`，它会在脚本尚未存在时插入脚本，并处理显示和隐藏小部件。

按路由进行代码拆分也很重要。使用动态导入仅为需要的路由加载代码（包括打包在应用中的第三方库）。例如，如果管理仪表板使用重量级的图表库，不要将其包含在主包中：拆分它并在用户导航到仪表板时加载它。同样，对于外部脚本：如果你只在特定路由上需要第三方小部件，就在进入该路由时加载它。在 React 中，你可能会在该页面组件中放置一个 `<Script>`（如果使用 Next.js）或 `useEffect` 来注入脚本。确保不要在用户离开并返回时重复加载它；你可能会检查一个全局标志或简单地保持它加载。

决定哪些第三方脚本应在全局加载（在应用启动时），哪些应延迟加载。例如，分析可能在启动时加载（使用 `defer`）以捕获所有页面。但反馈小部件可以等到用户触发它或到达特定页面。例如，假设一个 React 应用仅在结账页面使用 Stripe.js 进行支付。不要将 Stripe 的脚本放在 *index.html* 中，这会为每个用户加载它，即使他们从不购买，应用也可以在用户进入结账时动态加载它，使用 `@stripe/stripe-js` 中的 `loadStripe()` 函数（它内部处理注入脚本）。这种方法对于嵌入式地图也适用。

你也可以利用 service worker 进行缓存。在 SPA 中，后续导航在同一应用实例内，但 service worker 可以帮助缓存静态第三方请求，例如缓存你从第三方 API 获取的数据。

### 标签管理器性能调优

如果你使用类似 GTM 的工具，这里有一些提示：

- GTM 本身默认异步加载（片段使用 async 脚本标签）。但一旦加载，它会立即执行任何标签——其中一些可能是内部的第三方脚本。
- 你可以为标签配置触发器。出于性能考虑，你可能会将某些标签设置为在“Window Loaded”而不是立即触发。例如，分析标签可以设置为“Page View (Window Loaded)”而不是“Page View (DOM Ready)”，如果你想稍作延迟。你也可以只在特定页面上触发某些重量级标签，而不是全站。
- 如果你在 GTM 中有许多标签（比如超过 20 个带有重量级脚本的标签），请考虑根据页面类型或优先级将它们拆分为多个容器。但是，请注意，这会增加加载多个容器片段的开销。或者，你可以审核并删除未真正使用的标签。
- GTM 现在提供服务器端容器选项。这样，客户端会得到一个小型脚本，将数据发送到你的服务器容器，然后再分发到各端点。这需要基础设施（Google Cloud 设置），但可以大幅减少客户端负载。权衡是服务器端的成本和复杂性。

像 Next.js 这样的框架可以通过在 _app.js 中使用 `<Script strategy="afterInteractive">` 加载片段轻松集成 GTM，这是一个结合框架与标签管理器知识的示例。

### WordPress 和内容管理系统（CMS）平台

WordPress 网站（以及类似的 Joomla、Drupal 等）通常将很大一部分第三方脚本归因于插件和嵌入


WordPress 5.7+（2021 年 3 月发布）通过在 iframe 嵌入中添加 loading="lazy" 默认启用了 iframe 的懒加载，这一默认行为在当前版本中仍然保留。请确保此功能未被你的主题或插件禁用。

### 使用更新的官方小部件

一些第三方功能可以通过服务器端解决方案实现。例如，与其使用在页面加载后获取的 JavaScript 驱动的“相关文章”小部件，不如使用一个在服务器端计算相关文章并直接输出的插件，从而消除客户端的额外脚本调用需求。

### 限制插件

最简单的建议：插件越少，通常意味着第三方脚本越少。审计你的插件并移除任何没有提供显著价值的插件。有时多个插件会添加重叠的第三方标签；如果可能，请将其合并。

### 利用缓存/CDN

许多第三方脚本如果用户访问多个页面，可能会被用户浏览器缓存。通过使用 CDN 或服务工作者为你的静态资源提供缓存来鼓励缓存，并将第三方脚本的过期时间设置为遥远的未来（大多数已经通过其 CDN 这样做了）。

### 谨慎使用 GTM

如果你在 WordPress 上使用 GTM，请移除任何未使用的标签，并配置触发器使其不在不需要的页面上触发。

### 将第三方内容转换为第一方

与其嵌入 YouTube 视频播放器，不如考虑使用链接到 YouTube 的预览图片，或使用在点击时加载视频的灯箱。或者使用像 Embed Privacy 这样的插件为你实现门面，例如在用户同意或点击之前为 YouTube 提供占位符。

### SMALLER DEPENDENCY OPTIONS

一些可考虑的更小工具和 polyfill 包括：

#### *Head 管理库*

在单页上下文中管理 <head> 注入的工具，如 React 的 Helmet 和 Next.js 的 Head，允许你在导航时更新 <head>，包括添加和移除脚本标签。

#### *组件的动态导入*

对于不仅仅是脚本而是包的第三方库，在 React 等框架中使用动态 `import()` 可以拆分代码，以便你仅在需要时加载这些包。例如，如果页面使用第三方图表库，按需执行 `import('charting-library')` 将确保它不在初始包中。

#### *CDN 与自托管*

如果你使用具有打包功能的框架（如 Webpack），有时需要决定是打包第三方库还是通过 CDN 包含它。打包意味着更少的 HTTP 请求并保持在你的控制之下（如果它是模块化的，也许可以进行 tree-shaking）。使用 CDN 则可能允许并行加载和单独缓存。衡量哪种方式更适合你的情况。

### Ecommerce Sites

Shopify 不会给商家提供对加载时机的精细控制；如果第三方应用添加了脚本，它通常会按自己的意愿加载（通常在 head 中）。Shopify 通过商家一键安装的“应用”注入脚本，通常商家并不了解其对网站性能的潜在影响。然而，创建 Shopify 主题的开发者可以选择在底部添加脚本并使用属性，或使用 Shopify 的 script tag API 以 async 方式插入脚本。一些 Shopify 商家会诉诸自定义代码在加载后移动脚本，这并不理想。Magento 和 Drupal 工作方式类似：它们允许你在页面模板中添加脚本，因此精明的开发者可以将它们放置在主要内容之后并添加 async/defer。

电子商务网站也往往因大量营销标签而受苦：分析、跟踪像素、A/B 测试、评论小部件、推荐引擎、聊天等。业务团队经常添加这些来跟踪活动、联盟链接等。每个都可能是通过平台集成添加的第三方脚本，或在主题中添加代码片段。移除脚本可能会与组织的收入目标冲突，因此要谨慎。例如，A/B 测试工具可能很慢但能推动转化率的提升。

以下是电子商务网站的一些优化策略：

### 监控性能以符合预算

持续使用合成监控和 RUM 衡量第三方脚本的影响，并设置性能预算以帮助开发者做出决策（参见第 2 章）。

### 审计 ROI

定期审查所有第三方集成，以确定它们是否提供了足够的价值来证明其性能成本。例如，如果你有两个分析工具，考虑放弃一个。如果花哨的个性化脚本没有证明有益，请将其移除。

### 使用标签管理器

尽可能将尽可能多的脚本置于一个控制之下。如果营销坚持使用许多标签，将它们通过 GTM 或标签管理器至少可以集中优化位置，使用一次网络请求而不是多次，并使移除它们变得更容易。然而，如果使用不当，标签管理器会放大问题——例如，在每个页面上无论是否必要都加载数十个标签，采取“全部触发”的方式，其对性能的损害可能超过少数几个放置良好的脚本。定期审计你的标签，移除未使用的标签，并配置适当的触发器。尽可能考虑对电子商务使用服务器端 GTM。

### 按页面类型有条件加载

在结账页面或关键转化流程中，加载最少的第三方内容以避免任何中断。许多电子商务平台允许对结账页面和商店前端进行单独控制：例如，在 Shopify 中，你可以使用主题的条件逻辑（如 `{% if template == 'product' %}`）仅在产品页面加载产品评论小部件，或使用应用设置或主题的 *checkout.liquid* 文件中的自定义代码禁用结账页面的聊天小部件。

### 将广告和营销推迟到用户操作之后

类似用于捕获电子邮件的退出意图弹出脚本可能很重；你可以仅在用户花费 X 秒或查看 Y 个页面后才加载它，而不是在首次页面加载时加载。

### 优化核心第三方脚本

例如，产品推荐轮播有时提供预渲染部分内容的选择，这样脚本就不必做太多工作。

### 使用 Shopify 的原生功能

避免安装添加自己 JavaScript 的过多应用。例如，使用 Shopify 原生的客户评价系统可能比第三方评价小部件更精简。如果某个应用很简单，比如添加跟踪像素，有时你可以手动在主题中添加其代码并移除沉重的应用集成。

Shopify 的案例研究涉及这些想法：当公司移除或延迟非关键脚本时，转化率有所改善。速度可以直接与电子商务中的转化相关联。利用这些数据来论证性能：“是的，那个聊天小部件可能捕获一些线索，但如果它减慢每个页面，也许我们的整体销售额会下降。让我们以更智能的方式实现它。”

### AI 在第三方脚本优化中的作用

正如本章标题所暗示的，人工智能开始在帮助开发者管理和优化第三方脚本方面发挥有意义的作用。让我们探讨 AI 工具如何协助这一具有挑战性的网页性能领域。

现代 AI 工具，包括 LLM 和专门的性能分析工具，可以帮助识别有问题的第三方脚本并提出优化建议。

自动化脚本审计是一个重大创新：AI 工具可以分析你网站的加载行为并识别哪些第三方脚本导致了最多的性能影响。像 Chrome DevTools 和 Lighthouse 这样的工具已经提供了部分分析，而 AI 增强版本可以提供更具上下文的建议。例如，AI 助手可以分析你的 Lighthouse 报告并根据每个第三方脚本的目的和影响提出具体的加载策略。

在实施第三方集成时，AI 编程助手（如 GitHub Copilot、ChatGPT 或 Claude）可以帮助你：

- 建议适当的加载策略（async、defer、lazyOnload）
- 生成实现空闲回调或条件加载的包装代码
- 识别你不小心多次加载同一库的情况





- 推荐框架特定的解决方案，例如 Next.js 的 `<Script>` 组件

例如，如果你将供应商提供的脚本片段粘贴到 AI 助手里，你可以问：“我该如何在 React 应用中优化此脚本的性能？” AI 可以建议将其包装在带有适当清理的 `useEffect` 中，使用动态导入，或实现延迟加载。

AI 工具还可以帮助你快速创建常见第三方嵌入的 facade 实现。例如，你可以要求 AI 助手“创建一个 YouTube 嵌入 facade，显示缩略图并仅在点击时加载 iframe”，它将生成所需的 HTML、CSS 和 JavaScript，从而节省你的实现时间。

机器学习模型可以被训练来检测第三方脚本行为中的异常。例如，AI 系统可以监控你的 RUM 数据，并在第三方脚本突然开始导致性能问题时向你发出警报。例如，如果你的分析脚本通常执行时间为 50 ms，但突然跃升至 500 ms，AI 系统可以自动标记此异常。

AI 工具对于预测分析也很有帮助。通过分析第三方脚本如何影响跳出率和转化率等 UX 指标的模式，AI 模型可以帮助预测添加或删除特定脚本的 ROI 影响，从而使你更容易与利益相关者进行数据驱动的对话。

当第三方脚本过于沉重或存在问题时，AI 可以帮助生成替代方案，例如将客户端第三方功能转换为服务器端实现。例如，你可以要求 AI“将此客户端 A/B 测试脚本转换为在边缘函数上运行”，它可以生成适当的服务器端代码。

此外，对于某些第三方功能，AI 还可以帮助你构建更轻量的 first-party 替代方案。例如，与其加载沉重的反馈小部件，你可以使用 AI 生成一个简单、高性能的自定义解决方案，以满足你的特定需求。

虽然 AI 是优化的强大工具，但请牢记以下注意事项：

#### 需要验证

始终彻底测试 AI 生成的代码。AI 建议可能无法考虑你的特定边缘情况或安全要求。

#### 了解第三方政策

一些第三方服务对其脚本的加载方式有特定要求。AI 可能会建议违反服务条款的优化。

#### 上下文很重要

AI 工具没有关于你业务需求的完整上下文。一个看似缓慢的脚本可能对收入至关重要，因此仍需要人工判断。

#### 保持学习

将 AI 用作学习工具。不要只是复制粘贴解决方案——要理解 AI 推荐某种方法的原因，以便你做出明智的决策。

AI 在网页性能优化中的作用正在迅速发展。随着这些工具的改进，它们将变得越来越有助于管理第三方脚本这一复杂任务，但它们最好作为知识渊博的开发人员的助手，而不是取代人类专业知识。

在所有情况下，都很重要在组织内部培养性能文化，以便开发人员、营销人员和内容创作者了解添加“再一个脚本”的成本。我将在第 11 章深入探讨这一主题。
