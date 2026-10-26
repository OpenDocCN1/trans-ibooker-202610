## CHAPTER 7

## Insights from the Masters

Sometimes it’s important to seek inspiration from others, whether for motivation or understanding. Seeking knowledge from a master in a field can provide valuable insights and knowledge into a given topic.

In this chapter, I’ve interviewed some of these masters who’ve helped bring the games you love to market.

Let’s take a look. First up, we have Michael W., the creator of Mirror Networking, better known in the community as vis2k. Then, we have Curt, a senior solutions architect who’s been involved in games like Apex Legends and Fall Guys.

## Michael “vis2k” W. – Mirror Networking

Michael W. is the creator and founder of Mirror Networking. He and the contributors of Mirror Networking are why this book and many games are even possible in the first place. A special thank you to Michael for taking the time for this interview.

## Tell Me a Bit About Yourself and Where You Come From

I was born in Germany and switched schools when I was 15-ish, away from my friends.

I didn’t have the best time, so I started playing one of the early Asian grinder MMOs.

Super fascinated by that virtual world. I met so many people. Wanted to do my own MMO ever since.

So, I started reverse-engineering MMOs and developed bots to learn and have some income. I got into OpenGL and Delphi, C++, etc. Made my first engine, always with MMO as the goal.

Little town and environment, couple of monsters, server actually ran stable for months at a time. 60ish CCU, not much yet. Realized productivity was not ideal in C++. 60k LOC for a simple MMO scene already.




我学习计算机科学是为了了解更多。

我在Unity大约4.0版时开始开发。不用担心渲染器、动画、物理、多平台等等，这很不错。我一直坚持，做了些小游戏。

与此同时，我为MMO服务器尝试了Java、Python、Clojure、Erlang以及大概十几种其他语言。我学到了很多，尤其是Erlang。不过，情况似乎仍然不理想。

2015年8月的暑假，我看到了UNET测试版。我试了一下；它虽然满是漏洞，但服务器和客户端在同一个项目中、SyncVars、RPC等等，让我目瞪口呆。我知道这就是问题的解决方案。这显而易见。

我第一件试的东西是他们的移动球演示。我立刻开始了uMMORPG。

我做了血条、怪物、NPC等等。UNET的漏洞让人沮丧，但我还是在2015年12月发布了第一个版本，希望能赚到够买披萨的钱，同时偶尔开发。

事实证明，很多人都想制作MMO，这最终让我能够全职投入其中。

UNET仍然充满漏洞。我几乎恳求了团队中所有人去修复——没有结果。

所以，我最终分叉了它，自己修复其中一些问题，当时我们还必须在Unity安装文件夹中替换Unity.Networking.dll。Weaver一开始甚至没有开源。

这就是Mirror的起源。

现在是2021年了，已经过去一段时间——修复了数千个漏洞，改进了无数。目标是今年完成路线图，然后也许去度个假，最后开始做我的MMO。

如今已经有些人用Mirror运行着小型的MMO。看起来这件事终于变得可能了。

我仍然想重制我15岁时玩过的那款MMO。也许现在太晚了，人们不再喜欢刷刷刷的游戏。但这没关系。

我只是为自己做。

至少网络层现在是开源的。
这非常重要。
总有一天，我们会有开源引擎的。

## 你还有什么其他建议想对读者说吗？

- 健康饮食，远离毒品，去健身房锻炼。
- 每周编程80小时以上很辛苦。
- 锻炼身体，让它能跟上你的大脑。

## 你想感谢哪些人吗？

- Unity。我爱这个引擎。我真的爱。
- UNET团队的Sean Riley和Alexey Abramychеv，他们让这一切成为可能。
- Paul Pacheco，感谢他对Mirror的贡献以及对测试的推动。
- 所有贡献者。
- 我要感谢所有在Mirror Discord上伸出援手的人。
- 以及Unity资源商店团队，帮我实现了梦想的工作。

## Curt – 高级解决方案架构师

Curt是一位高级解决方案架构师，也是一位真正的行业老兵，已有9年经验。

## 请介绍一下你自己以及你的背景

我在2000年代初通过管理反恐精英服务器进入了游戏服务领域。
凭此，我申请了游戏行业的工作，最初是管理CS局域网锦标赛的服务器。

后来我逐步承担更重要的角色，专注于支持在线游戏和管理企业级游戏与编排系统的集成。

我的技能逐渐扩展到匹配器以及开发者需求中的相关服务。

现在，我借助过往经验，帮助构建这些后端服务的端到端解决方案。

## 在你和你的团队或客户发行多人游戏时，遇到的最大网络相关挑战是什么？

网络代码。当我和开发者合作时，第一个挑战永远是网络代码。每款游戏都是不同的，因此不存在完美的开箱即用方案，除非你是在做竞技场射击游戏。

接下来的挑战是后端服务，需要回答这类问题：

- 你会用什么专用服务器？Unity无头服务器？
- 你会用什么网络代码/传输层：Photon Bolt、Unity MLAPI，还是自定义？
- 游戏服务器最多能支持多少玩家？30还是3000？
- 你如何从动态容量中分配游戏服务器，还是完全不扩缩容？
- 分配由什么来负责？匹配器？另一个服务？你如何将正在使用的服务与可用的服务和游戏服务器进行协调？你如何管理这些服务？还有很重要的一点，谁来支持它们？

想清楚你的目标网络容忍度（RTT）——50/100/200毫秒等，然后判断网络代码能否支持这样的延迟。

决定你希望在全球支持多少个集群。

你如何将玩家引导到区域服务；他们能否手动更改区域？

从后端服务的角度来看，玩家流程（循环：游戏、游玩、重复）是什么样的？

安全性如何？

## 在预算中，你应把多少比例花在云基础设施上？

这取决于重要性；游戏是否仅限在线？

## 你偏好哪家云基础设施提供商来托管数据库？

MySQL用Azure，其他数据库用GCP或AWS。

## 你偏好哪家云基础设施提供商来托管服务器？

Unity Multiplay

## 对于想学习多人游戏开发的人，你能给出的最关键建议是什么？

那些未知的问题才是最重要的。

尽可能让自己站在玩家的角度去发现这些问题。例如，网络连接不好的用户会把问题归咎于你的服务器差劲。

最重要的是确保你的游戏好玩。
