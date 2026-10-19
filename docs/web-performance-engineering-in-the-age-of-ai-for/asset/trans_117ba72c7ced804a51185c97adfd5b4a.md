
scheduling
  – resources, 请求优先级和调度
  – streaming AI interfaces, 调度和负载下的响应性
  – tasks, Chrome 主线程任务调度器和任务优先级-Chrome 主线程任务调度器和任务优先级
  – third-party scripts
    – after onload, 加载后或首次交互
    – breaking up tasks, 拆分长任务-拆分长任务
    – on first interaction, 加载后或首次交互-加载后或首次交互
    – idle until urgent pattern, 空闲直至紧急模式
    – requestIdleCallback API, requestIdleCallback API-requestIdleCallback API
    – setTimeout API, 使用 setTimeout 作为回退或更简单选项-使用 setTimeout 作为回退或更简单选项
Script component (Next.js), Next.js 中的 Script 组件-Next.js 中的 Script 组件
script tags, positioning, 脚本标签定位-脚本标签定位
scripts and event loop, 脚本与事件循环
scrolls, infinite, 原地增长的列表和信息流
selective hydration, 服务端渲染和水合
sequencing third-party scripts, 排序和优先级-总结排序最佳实践

server-sent events (SSE), 传输层选择

server-side architecture

– streaming AI interfaces, 网络和服务器管道考虑-谨慎分块

– third-party scripts, 在服务器端处理第三方逻辑-在服务器端处理第三方逻辑

server-side rendering (SSR), 服务端渲染和水合-服务端渲染和水合

service workers, 缓存和服务工作线程，在服务器端处理第三方逻辑

setTimeout API, 使用 setTimeout 作为回退或更简单选项-使用 setTimeout 作为回退或更简单选项

Shopify Carpe case study, Shopify 商家商店：Carpe-结果

Shopify case study, Shopify：商家商店 AI 优化-结果

Shopify Sunday Citizen case study, Shopify 商家商店：Sunday Citizen

SI (speed index), 其他重要指标

single point of failure (SPOF) problem, 为什么第三方脚本影响性能，WebPageTest (WPT)

single-page applications (SPAs), 在单页应用与多页应用中处理第三方脚本-在单页应用与多页应用中处理第三方脚本

slow loading, 臃肿的包和缓慢加载-臃肿的包和缓慢加载

slow start, 渲染管道

SPAs (single-page applications), 在单页应用与多页应用中处理第三方脚本-在单页应用与多页应用中处理第三方脚本

speculative loading, 推测加载和预渲染

speed (in user experience)

+   - first impressions and, 缓慢的代价：第一印象与用户满意度-评估 UX 性能：关键指标
+   - perceived, 感知速度模式
+   - psychology of waiting, 等待的心理学
+   - response-time thresholds, 为什么性能即用户体验，缓慢的代价：第一印象与用户满意度-缓慢的代价：第一印象与用户满意度

speed index (SI), 其他重要指标

SpeedCurve, WebPageTest：真实测试与高级分析

SPOF (single point of failure) problem, 为什么第三方脚本影响性能，WebPageTest (WPT)

SSE (server-sent events), 传输层选择

SSR (server-side rendering), 服务端渲染和水合-服务端渲染和水合

stall rate, 指标

stall rates, monitoring, 警报停滞模式

stream trace views, 构建流追踪视图

streaming AI interfaces

+   - accessibility, 可访问性
+   - antipatterns, 应避免的反模式
+   - checklist for production, 生产实践清单-生产实践清单
+   - CWVs and, 流式世界中的 CWV-流式世界中的 CWV
+   - design patterns, 应优先的设计模式
+   - energy usage, 能耗与成本意识
+   - internationalization, 国际化
+   - media content, 流式内容与媒体-渐进到达的图像
+   - network considerations, 网络和服务器管道考虑-谨慎分块
+   - observability and debugging, 可观测性与调试流式系统-警报停滞模式
+   - privacy, 隐私
+   - responsiveness under load, 调度和负载下的响应性
+   - safety and errors, 安全
+   - text, 流式文本-感知速度模式
+     - metrics, 指标
+     - network considerations, 传输层选择-传输层选择
+     - perceived speed, 感知速度模式
+     - performance budgets, 性能预算
+     - rendering, 渲染策略-渲染策略

+   - UI components, 流式 UI 组件-需关注的故障模式
+     - failure modes, 需关注的故障模式
+     - metrics, 流式 UI 指标
+     - rendering, 流式组件渲染策略，流式组件渲染策略
+     - user experience and, 流式优先 AI 界面与用户体验：新的性能范式-流式优先 AI 界面与用户体验：新的性能范式

streaming compilation, 后台编译、解析和 GC

style calculation, 渲染管道

Sunday Citizen case study, Shopify 商家商店：Sunday Citizen

suspense reveal latency, 流式 UI 指标

# T

tag managers, 标签管理器性能调优

tasks

+   - breaking up, 拆分长任务-拆分长任务
+   - prioritizing, Chrome 主线程任务调度器和任务优先级-Chrome 主线程任务调度器和任务优先级
+   - scheduling, Chrome 主线程任务调度器和任务优先级-Chrome 主线程任务调度器和任务优先级

TBT (total blocking time), 其他重要指标

team education in performance culture, 教育和赋能团队-教育和赋能团队

Telegraph Media Group case study, 电讯报媒体集团-结果

testing
  – AI-generated code, 对质量和性能负责
  – optimizations, 真实设备测试：终极验证
text, streaming via AI, 流式文本-感知速度模式
  – metrics, 指标
  – network considerations, 传输层选择-传输层选择
  – perceived speed, 感知速度模式
  – performance budgets, 性能预算
  – rendering, 渲染策略-渲染策略
text-to-speech (TTS), 音频和视频生成或文本转语音
thermal throttling, 设备性能：CPU、内存和移动差距
third-party scripts
  – defined, 第三方脚本简介
  – loading
    – conditional loading, 条件加载-权衡
    – facades, 伪装和点击加载模式-权衡
    – lazy loading, 延迟加载第三方内容-示例：延迟加载分析脚本
    – sequencing and prioritizing, 排序和优先级-总结排序最佳实践
  – metrics

+   - AI-based scripts, AI 基于脚本的特殊考虑
+   - inventory of scripts, 库存和识别：关键指标和指标
+   - with Lighthouse and DevTools, Lighthouse 和 Chrome DevTools-Lighthouse 和 Chrome DevTools
+   - with WebPageTest, WebPageTest (WPT)-WebPageTest (WPT)
+   - optimizing
+     - AI and, AI 在第三方脚本优化中的作用-AI 在第三方脚本优化中的作用
+     - ecommerce sites, 电商网站-电商网站
+     - framework-specific optimization, 框架特定优化：Next.js 及以后-在其他框架中优化
+     - principles of, 第三方优化原则-目标：快速且功能完整
+     - single-page versus multipage applications, 在单页应用与多页应用中处理第三方脚本-在单页应用与多页应用中处理第三方脚本
+     - tag managers, 标签管理器性能调优
+     - WordPress and CMS platforms, WordPress 和内容管理系统平台-WordPress 和内容管理系统平台
+     - performance issues, 为什么第三方脚本影响性能-为什么第三方脚本影响性能

