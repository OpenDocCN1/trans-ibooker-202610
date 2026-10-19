### Part III. Optimizing JavaScript

[OceanofPDF.com](https://oceanofpdf.com)

#### Chapter 7. The Cost of JavaScript

> *It has never been easier to deliver pages quickly, but we are not collectively hitting the mark.*
> 
> — Alex Russell

JavaScript is a double-edged sword: its performance can determine whether your users are delighted or frustrated. While it enables rich interactive experiences and is essential for creating the dynamic web experiences discussed in Part II, its costs are paid in CPU cycles, network bandwidth, and precious seconds of user time. Part III of this book explores what slows down JavaScript web applications and how to deliver great user experiences by mitigating those costs.

This chapter takes a deep, pragmatic look at JavaScript performance in the browser, focusing on real-world constraints and modern solutions. You’ve seen in Chapters 5 and 6 how new techniques like code splitting, hydration, and scheduling can improve perceived performance. Here, I’ll examine how hardware and browser internals affect JavaScript execution and how JavaScript engines themselves have evolved to run code faster. My goal is to give you a high-level framework for thinking about JavaScript performance and a set of concrete techniques to apply.

#### Understanding the Constraints

To understand the cost of JavaScript, you must first understand *why* JavaScript can be so costly on some devices and networks. This chapter explores how CPUs, networks, and browser architectures shape its performance, often in surprising ways.

#### Device Performance: CPUs, Memory, and the Mobile Gap

As Alex Russell warns, if you build assuming ideal conditions, you risk excluding the many users on suboptimal devices or connections. Not all users have the latest iPhone or a high-end desktop. In fact, as discussed in Chapter 1, a huge portion of the world accesses the web on budget Android phones or older devices. The disparity in computing power is stark, and it directly impacts JavaScript execution.

JavaScript is CPU-bound during parsing and execution. A script that runs in one second on a high-end device might take three, four, or even six seconds on a low-end one. One 2019 study found that in loading a typical site like Reddit’s web app on a midrange phone, script execution took three to four times longer than on a flagship phone. It took more than six times longer on a low-end budget Android phone. In other words, a page that is “fine” on your MacBook Pro might be nearly unusable on an inexpensive Android handset.

Why such a big gap? Late-model high-end CPUs have multiple cores, higher clock speeds, bigger caches, and more advanced microarchitectures. Low-end and older devices often have fewer and slower cores and may throttle performance to save battery or prevent overheating (this is called thermal throttling). So not only do cheap devices have slower baseline speeds, but their sustained performance can degrade over time. JavaScript that “pegs the main thread,” or fully occupies a CPU, on these devices can easily cause noticeable lag or jank. It’s not just raw speed either; memory and garbage collection (GC) can also be constraints. Low-end devices have less RAM, which can lead browsers to collect garbage more frequently or to struggle with large script files in memory.

Russell’s research into this performance inequality gap highlights that, to serve users at the 75th percentile of devices, developers and engineers need to rein in our resource usage significantly. The assumption that Moore’s law means “devices will keep getting faster” simply hasn’t panned out for the majority of users. In Russell’s blunt words, “It was all bullsh!t.” The reality is, device performance improvements have slowed, and the lower end of the

milliseconds even before any data flows. If your page is making dozens of requests (for scripts, JSON, and so on), those latencies add up.

For instance, on a 4G network with good signal, the *setup latency* for a single request might be around 100 ms; on a 3G network, it could be 600 ms or more. And that’s *per request*. If your site fetches many small JavaScript chunks sequentially, users could be waiting through many round-trip delays. This is why performance best practices often advise bundling resources to reduce requests—though, as you saw in Chapter 6, bundling has its own trade-offs.

Bandwidth is the other piece. After the initial latency, how fast can the bytes of the JavaScript file travel? Modern networks like 4G do provide higher bandwidth, but user conditions vary wildly. A user who has “full bars” of LTE but is in a congested network cell might find that a connection with a usual speed of tens of MB per second (Mbps) yields only 1 Mbps. Or they might get a strong connection that suddenly drops or fluctuates. Unlike wired broadband, mobile bandwidth can change second to second. Moreover, users on limited data plans or in “Save Data” mode explicitly *prefer* sites to be lightweight. If your site ships megabytes of JavaScript, it could exhaust some users’ monthly data, literally costing them money. That’s why developers should consider payload size as part of the cost of JavaScript.

Browser networking optimizations (like HTTP/2 multiplexing and caching) help mitigate some of these issues. HTTP/2, for example, allows multiple files to be downloaded in parallel over one connection, so splitting a bundle into pieces doesn’t incur as much extra latency overhead as it would under HTTP/1.1. But there are limits to what protocol improvements can do. The bottom line is that smaller scripts and fewer of them mean faster loads, especially on slow networks.

One practical guideline from performance experts is to keep initial JavaScript payloads under certain size budgets (for example, <200 KB compressed for mobile initial load remains a reasonable guideline as discussed in 2018). And as a Google study of news sites found, complexity

kills performance: some slow pages made 500+ network requests, leading to load times of 10 to 30 s. Much of that overhead comes from third-party scripts and large JavaScript bundles.

Remember that “the best network request is one that you don’t make.” Every script you choose to load should be justified by its benefit to the user.

Networks—particularly mobile networks—are a fundamental limit. Even with a powerful chip, a device can’t run code it hasn’t downloaded. And if that download is stuck behind a slow 3G connection or 300 ms latency per request, the user will be waiting. Thus, the cost of JavaScript begins even before execution: large scripts cost time on the wire.

#### Browser Architecture: The Main Thread, Parsing, and Execution

Finally, we have to understand how browsers handle JavaScript under the hood. As you learned in Chapter 5, the main thread is a busy place: it’s not only running JavaScript, but it’s also doing layout, style calculations, painting pixels to the screen, and handling user-input events. When your JavaScript finally arrives over the network, the browser must parse it from text into bytecode or an abstract syntax tree (AST), compile or interpret it, and execute it—typically on the main thread. If JavaScript takes too long on the main thread, it blocks everything else, delaying other work.

Parsing JavaScript is surprisingly expensive. It’s CPU-intensive and must happen before the code can run. According to the team that runs V8 (Chrome’s engine, also used in Node.js), raw JavaScript parsing speed has improved significantly, but it still can consume a lot of time on a slow CPU. If you send huge scripts, you force the browser to do a lot of parsing work up front.

Modern JavaScript engines (as discussed later in this chapter) do many clever things to reduce parse and compile overhead on the main thread. For instance, V8 can perform streaming parsing (parsing the script incrementally as it downloads) and can offload some compilation work to a background thread. These optimizations have cut down main-thread


parse/compile time dramatically. V8 has reduced main-thread parse/compile work by about 40% on average for real websites compared to earlier versions by doing work off the main thread. Some sites saw even bigger improvements (Pinterest saw around 62% less main-thread compile work, YouTube around 81%). This means the browser tries hard to make loading JavaScript less likely to block the main thread. But there are limits; certain phases (like initial parsing and handling syntax errors) still happen on the main thread, and of course code is always executed on the main thread.

Once parsed, the JavaScript engine either interprets the code or just-in-time (JIT) compiles it and begins running it. If your code immediately does a lot of work (like a big loop or complex calculations on load), that’s going to tie up the main thread, delaying user interactivity. Chrome’s parsing and compiling might be happening off-thread, but the moment your code calls function initApp() { ...heavy work... }, that heavy work is on the main thread, potentially causing what users perceive as slow loading or input lag.

Recent measurements show that script execution (not just download) can account for a significant portion of total load time—30% or more on JavaScript-heavy single-page apps. In other words, even after the bytes are delivered and parsed, running the code is a major cost. A complex web app might run tens of thousands of JavaScript operations during startup, and all of that competes with rendering and input handling.

Browsers typically execute JavaScript in a single thread (the main thread, unless you explicitly use web workers), effectively making it single-threaded (in the context of the page) and cooperative. Only one JavaScript task runs at a time. If a task doesn’t yield, nothing else can run. The browser can’t, for example, scroll the page or process a button click while your script is crunching numbers. This is why I emphasize breaking up long tasks and yielding control (covered in Chapters 5 and 6).

Like devices, not all browsers are equal. Each major browser has its own JavaScript engine and performance characteristics. Most have similar main-thread constraints, but some lower-end browsers (or older versions) lack certain optimizations. Also, if the user has many tabs open or the device is under load, your page has to contend for CPU, so its performance could be affected. You can’t control those external factors, but you *can* at least avoid being the thing that maxes out the CPU.

### The JavaScript Engine

Modern JavaScript engines are marvels of optimization, squeezing every millisecond out of our code.

While we developers optimize our code loading and scheduling, the browser’s JavaScript engine works tirelessly in the background to run that code faster. Over the past decade, JavaScript engines like Chrome’s V8, Firefox’s SpiderMonkey, WebKit’s JavaScriptCore, and others have made astonishing progress. They have introduced JIT compilation, multitiered execution strategies, concurrent garbage collectors, and myriad micro-optimizations.

This chapter explores how JavaScript engines have evolved to mitigate JavaScript’s performance costs. Understanding these internals will help you appreciate what the runtime does—and what it *cannot do*. Engines have reduced many bottlenecks (like parsing and function call overhead), but they can’t magically speed up a bloated 5 MB bundle or eliminate the logic cost of an app. Once you understand the engine’s strengths and weaknesses, you can write code that works with the engine, not against it.

### From Interpretation to JIT Compilation: Engine Evolution

In the early days, JavaScript was purely an interpreted language. The engine would parse the source code and execute it directly, via a tree walker or similar mechanism. That was fine in the 1990s, when scripts were tiny. As JavaScript use grew, though, the engines became more sophisticated.

The big leap came in the mid-2000s with JIT compilation. Modern engines use a *tiered JIT* architecture. The basic idea of JIT is to compile JavaScript to native machine code *at runtime*, so it runs faster than interpretation. But doing this optimally is tricky, because JavaScript is a dynamic language.

Let’s use V8 (Chrome’s engine) as an example. V8 has at least three execution tiers:

#### *Ignition (interpreter)*

When code is first loaded, V8’s Ignition interpreter turns JavaScript source into bytecode and executes it immediately. Interpretation has low startup costs: it starts running quickly, which is good for initial load. But interpreted code runs slower than compiled code.

#### *Baseline JIT (Sparkplug)*

V8 added an internal baseline compiler called Sparkplug. This kicks in after the interpreter, taking the bytecode and compiling it to machine code without heavy optimizations. The goal is to get to faster-than-interpreter code quickly but without spending a lot of time optimizing. Sparkplug-generated code isn’t as fast as fully optimized code, but it’s a step up and can be produced quickly.

#### *Optimizing JIT (TurboFan)*

As the code runs, V8 collects profiling information, like the types of variables and which branches are taken. When it identifies *hot functions* (code that runs frequently), it invokes TurboFan, the optimizing compiler. TurboFan uses the type feedback to generate highly optimized machine code for that function. This code can inline functions, unbox objects, and do all sorts of optimizations under the hood, all tailored to the types of variables it observes. The result is often an order-of-magnitude speedup for hot code.

This tiered approach means that code starts running quickly, with no big pause to compile everything fully up front, and gets faster as it runs. There is a warm-up period: a function might run a bit slower at first, while it is interpreted or baseline compiled, and then speed up after a few iterations, once it’s been optimized. Surma, a Chrome DevRel engineer, explains that under ideal conditions, both WebAssembly and JavaScript end up as machine code and can reach similar peak performance, but JavaScript has this dynamic compilation path to get there. The engine trades initial performance for eventual performance: first make it work, then make it fast.

However, JITs also can deoptimize. If the assumptions used to optimize the code break—say, a variable that was always a number suddenly becomes a string—the optimized code might no longer be valid. V8 then deoptimizes and falls back to baseline or interpreter. It might attempt to reoptimize the code later, if conditions stabilize. All this happens under the hood, without developers realizing it.

Other engines, like SpiderMonkey and JavaScriptCore, have similarly tiered systems. While the specifics differ, conceptually, they all aim to balance startup and peak performance.

This optimization pipeline means that your JavaScript often runs much faster than you might expect. It also means that certain patterns can be friendly or hostile to JIT. For example, if you write functions that consistently receive the same types and shapes of objects, JITs can optimize them well. If your function sometimes gets a number, sometimes a string, sometimes an object, the JIT will have a harder time optimizing, because it has to handle all cases. Thus, it’s a good idea to avoid “shape-shifting” your variables: try to use a consistent structure. That’s not to say you need TypeScript (the JIT will figure types out at runtime); it’s just that at runtime, you shouldn’t mix and match too radically in hot code paths.

If your code runs only once (like initialization code), it might never get optimized by the JIT because it’s not “hot” enough. In these cases, interpreter performance matters. In contrast, a tight loop that runs thousands of times will get optimized. This is why microbenchmarking a small loop might not show benefit if it doesn’t run enough to trigger the optimizer. It also suggests an interesting paradox: if you break your code into too many small functions, each of which is called only a few times, they might not get


optimized individually. Sometimes inlining can help (whether it’s done manually or by the engine) because it makes one function hotter.

Modern JavaScript engines are also increasingly incorporating machine learning to improve their optimization decisions. For example, engines can use profiling data and ML models to predict which functions are likely to become hot and should be optimized earlier or which branches are likely to be taken. While still an emerging area, AI-assisted JIT compilation represents the next frontier in JavaScript performance optimization, potentially making engines even smarter about when and how to optimize code.

### Background Compilation, Parsing, and GC

I’ve touched on moving work off the main thread, but let’s look deeper. Parsing and compiling JavaScript on the main thread directly competes with the critical rendering path. To mitigate that, engines have introduced background threads to handle these tasks. V8, for example, can parse and compile scripts on a worker thread as they are being downloaded (this is called streaming compilation). By the time the script is fully downloaded, a good chunk of it might already be parsed or even compiled—without blocking the main thread. The V8 team has reported that these efforts “reduc[ed] the amount of time spent compiling on the main thread by between 5% to 20% on typical websites.”

This translates to pages loading faster because it frees the main thread to do other stuff (like process HTML/CSS or handle interactions) while the heavy JavaScript prep work is done in parallel. Why not parse on one core while executing on another? Background compilation isn’t trivial—thread safety and coordination are major considerations—but the payoff is big on multicore devices (which is basically all devices today).

Another improvement has been lazy parsing. Engines don’t always fully parse every function on initial load. Many employ a lazy strategy: parse just enough to know where functions start and end, but only fully parse the function’s code when and if it’s actually invoked. This way, if you have a large library but only use a portion of it, the unused parts might never be fully parsed (saving time). For example, V8 has had lazy parsing for years. Not parsing inner functions until they’re called can reduce parsing overhead significantly if many functions are never used. Of course, if eventually they are all used, the cost comes anyway—it’s just deferred.

Memory management has also become more efficient. JavaScript has automatic GC, and early on, this sometimes caused noticeable pauses. Modern engines have highly optimized garbage collectors that often do a lot of work incrementally or on background threads. GC matters to performance because a GC pause is a pause in JavaScript execution. Engines try to make GC pauses so short that you don’t notice them: ideally, under one or two ms, in incremental slices. Efficient memory management also means the engine can allocate and free objects quickly, which is important for object-heavy code. However, if you allocate enormous numbers of objects very quickly or hold onto a lot of memory, this may cause long GC pauses occasionally. Memory leaks in your code, like forgetting to drop references, can also hurt performance, since the engine may have to scan a larger heap.

Engine improvements also target *power efficiency*. On mobile, doing the same work faster means the CPU can idle sooner, saving battery and reducing thermal strain (less chance of hitting that thermal throttle point). So optimizing JavaScript isn’t just about raw speed but about using fewer CPU cycles overall, which is good for energy.

### The Limits of Engine Magic

For all the engine’s smarts, it still has limitations. If a site is slow because it’s executing huge scripts, the engine can only optimize so much. No matter how good the JIT, on a low-end phone, executing 5 MB of minified (compressed and optimized) code that initializes a complex app will take a noticeable time. Engines also can’t eliminate logic that’s fundamentally unnecessary to the user; they won’t, for instance, decide, “this framework is doing extra work, so I’ll skip those calls.” It’s a developer’s job to avoid sending unnecessary code. Engines just make each individual operation faster.

Another limitation is that certain JavaScript behaviors are inherently hard to optimize. For instance, with `eval` or dynamic code generation (function constructors), the engine has to treat the resulting code as new and unknown, preventing some optimizations. Similarly, if you modify the prototypes of built-in objects (known as *monkey-patching*), the engine’s assumptions about, say, `Array.prototype.map` might become invalid, which will affect its ability to optimize calls to that object. Engines generally optimize for the idiomatic common cases—the well-trodden path. So if you avoid too much dynamic trickery and use normal prototypes and standard patterns for classes or closures, you’ll maximize optimization. If you do weird stuff like using a single object to store lots of unrelated properties dynamically, the engine might fall back to slower, more general paths.

There are also trade-offs between engines’ startup performance and their long-term performance trade-off. For faster startup on repeat visits, Chrome and other browsers use a technique called *bytecode caching*: the engine can cache the compiled bytecode of scripts in a database so that the next page load can skip parsing that script. However, this cache can be invalidated if the script changes. It’s especially helpful on mobile, where parsing large scripts repeatedly is expensive. As a developer, you don’t manage this, but you should know that it exists.

Finally, consider multithreading and parallel JavaScript. Apart from web workers, JavaScript in a single page remains single-threaded, and we don’t yet have a way to utilize multiple cores for computation in JavaScript. If you have a compute-heavy task, using a web worker that runs on another thread is a way to leverage another core. But writing multithreaded code is complex. The typical approach is to offload distinct tasks to workers and communicate via messages, so the main thread (and thus the UI) stays responsive. Transferring data between threads does incur overhead, but there are techniques to optimize that.

Today’s engines execute JavaScript an order of magnitude faster than they did 10 years ago. Yet many sites still feel slow—often because developers give the engines even more work than we did before. Our job is to not overwhelm the engine in the first place, and the engine’s job is to run our code as efficiently as possible. Now let’s look beyond the browser to optimizing JavaScript on the backend.

### Backend JavaScript Performance

The browser is where JavaScript’s cost is most visible to users, but JavaScript is also used on the backend: in Node.js, Deno, serverless functions, and more. This brief chapter touches on considerations for JavaScript performance on the server side. Although backend JavaScript performance isn’t directly visible to end users in the way the frontend is, it ultimately affects them via API latency or page generation time. For example, if you’re using SSR on the backend with Node, a slow server will delay the initial HTML. So achieving good backend performance is part of delivering good UX. It’s all part of the critical path, until the user sees and can interact with your content.

The backend environment is different—there’s no UI to render, and the hardware is typically more consistent—but a sluggish server can harm user experience indirectly by causing slow responses. Interestingly, many of the frontend techniques discussed in this book apply conceptually to the backend, too. In a backend context, splitting code might mean not requiring modules until you need them (lazy require) to save memory and avoid cold-start latency (discussed later on). The backend, like the browser, can cache computations or responses in memory or a caching layer to avoid redoing work. And load shedding on the backend works somewhat like a performance budget does on the frontend: you can decide not to do certain optional work under heavy loads to preserve core performance.

One difference is that, on the backend, you usually don’t need to worry about bundle size in the network sense, since the code isn’t delivered to a client. But you do need to worry about dependency bloat, which can affect


内存使用和启动时间。很容易引入许多 NPM 包，这可能会减慢 require 时间或消耗大量 RAM，从而间接影响性能——尤其是在内存受限的环境中，例如内存有限的前端即服务容器。

### Node.js 和事件循环

Node.js 运行在 Chrome 的 V8 引擎上，因此第 6 章中讨论的所有引擎优化也同样惠及 Node。解析和 JIT 的工作方式类似。一个关键方面是，Node 在 JavaScript 执行方面是单线程的，除非使用其 Worker Threads 或集群。这意味着，就像在浏览器中一样，当你的 Node 服务器正在执行繁重的 JavaScript 计算时，它将无法在同一线程上处理其他传入请求。Node 在输入/输出（I/O）并发方面表现出色（得益于其事件循环和异步非阻塞 I/O 模型），但 CPU 并发需要更多努力。

因此，一个原则是：不要阻塞 Node.js 事件循环。如果你的服务器必须进行昂贵的计算（例如图像处理或大数据 crunching），请考虑将其卸载到单独的进程或线程，甚至是不同的服务。对于 Web 服务器，通常花费的时间是在等待数据库或网络 I/O。Node 通过在等待或回调期间不阻塞来高效处理这一点，但偶尔人们会误用 Node，在内联中执行 CPU 密集型任务。

例如，解析一个大型 JSON 文件或在纯 JavaScript 中压缩数据可能会冻结 Node 服务器。权衡在于吞吐量与简单性。如果你这样的任务很少发生，或者服务器主要一次只为一个请求执行此操作，你可能可以应付。但通常，最好流式传输并分块处理此类工作，或使用原生模块（如 C++ 插件或 WASM），它们有时可以更快地完成工作并定期释放控制。

Node 提供了 worker threads，允许你在同一进程内的并行线程中运行 JavaScript。如果你有一个多核服务器和一个 CPU 密集型任务，将其移至 worker thread 可以确保主线程

（事件循环）继续处理其他请求。这就像在浏览器中使用 web workers。

### 后端特定优化

在后端，性能通常关乎吞吐量（每秒请求数）和延迟（响应时间）。如果你的 Node 服务器提供一个从各种来源组装数据的 API 端点，你如何优化它？你可以并行化外部调用，同时发出多个 awaits 以减少总体延迟。这些编排中的 JavaScript 开销通常与网络和数据库时间相比微不足道。

然而，如果你的服务器每个请求都做很多 JavaScript 处理，它需要高效，否则会降低吞吐量。例如，为每个请求执行复杂逻辑的 Express 中间件在规模化时可能成为瓶颈。分析你的 Node 应用（使用 Node profiler 或 clinic.js 等工具）可以帮助你找到热点。

缓解重 CPU 使用的一种方法是扩展。Node 进程可以集群化，每个 CPU 内核一个进程。例如，四核机器上的四个进程组成的 Node 集群可以并行处理四个 JavaScript 执行，因为每个都是单线程的。许多基于 Node 的服务器（如 PM2 或内置的 cluster 模块）就是这样做的，以利用所有 CPU 内核。权衡是内存使用量更高，因为每个进程都有自己的 V8 实例。但对于后端服务来说，为了最大化硬件使用，这通常是值得的。

Node 中的内存泄漏，例如在全局中保留请求数据，会随着时间的推移降低性能。进程可能会更频繁地进行垃圾回收，甚至耗尽内存，导致变慢或崩溃。在浏览器中，内存泄漏只影响一个页面；在服务器上，它会影响所有用户的正常运行时间和性能。这就是为什么监控内存使用很重要。你可以连接 Node 的 --inspect 和 Chrome DevTools 等工具在需要时进行堆快照。

Node 中的 GC 是“stop-the-world”式的：换句话说，它会暂停 JavaScript 执行以进行清理。在服务器上，50 毫秒的 GC 暂停

阻塞操作，优化热点代码路径，利用并发，并进行测量。Node.js 已经成熟到可以驱动大规模系统，部分成功归功于工程师将性能最佳实践应用于 Node，就像他们对任何其他服务器平台一样。整体性能策略同时关注客户端和服务器。你的前端优化可能会被缓慢的后端抵消，反之亦然——因此在优化你的 Web 应用时，请考虑在需要的地方分析并改进堆栈的两端。

### 结论

JavaScript 的成本是每个用户都要支付的，无论是时间还是设备资源，作为工程师和开发人员，我们的工作是代表他们明智地花费这些字节和周期。

回顾本章的几个关键主题：

- *了解你的约束*

并非所有用户都有高端设备或快速网络。CPU 的单线程性能每年并没有显著提升，而且廉价设备与昂贵设备之间的差距巨大。网络延迟是一个根本限制，要求我们最小化往返次数和负载大小。浏览器在单个主线程上执行 JavaScript，使得及时、模块化的工作至关重要。牢记这些约束，可以将我们的性能工作建立在真实的用户环境中。

- *加载更少，做更少，尽可能晚地做*

这一座右铭支撑着许多技术。代码拆分和懒加载有助于你前期加载更少的代码。调度、空闲工作和高效算法有助于你的代码以更少的工作获得相同的结果。推迟非关键任务和资源意味着尽可能晚地做事，但不能更晚——你仍需要在时间内满足用户的需求。通过注意 JavaScript 运行的*时间*和*多少*，你可以创建更多
