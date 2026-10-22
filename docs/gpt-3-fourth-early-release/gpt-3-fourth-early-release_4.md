## 第4章 GPT-3 作为下一代初创企业的助推器

## 致抢先发布版读者的说明

使用抢先发布版电子书，您可以在这些标题的正式发布之前很久就获得书籍的最早形式——作者在写作时未经编辑的原始内容，以便您能够利用这些技术。

这是最终书籍的第4章。请注意，GitHub 仓库将在稍后激活。

如果您对如何改进本书的内容和/或示例有任何建议，或者如果您注意到本章中缺少材料，请联系作者 sandra.a.kublik@gmail.com 和 shubhamsaboo111@gmail.com。

在 GPT-3 发布之前，大多数人对人工智能的互动仅限于某些特定任务，例如让 Alexa 播放您喜欢的歌曲，或使用 Google 翻译用不同语言交流。研究人员已成功开发出能够执行枯燥任务的人工智能，但到目前为止，人工智能在执行抽象任务方面的创造潜力仍无法与人类相匹配，而这些任务没有明确、定义良好的指令。

随着大型语言模型时代即将到来，我们正在见证一次重大的范式转变。大型语言模型向我们展示了，通过增加模型规模，它们可以执行与人类相似的创造性和复杂任务。现在最大的问题是：人工智能是否能够执行创造性活动？

人工智能的创造潜力一直是一个令人兴奋的研究领域，尽管大多隐藏在 Google 和 Facebook 等公司严格的研发墙之后。GPT-3 改变了我们与人工智能互动的方式，并赋予人们构建在发布前似乎遥不可及的下一代应用的能力。IBM 研究院多媒体与视觉部门经理 John Smith 指出，“人工智能随机想出一些新颖的东西很容易。但要想出新颖、意外且有用的东西就非常困难。” 而 Somatic 首席执行官 Jason Toy 则问道，“我们能否将人类认为美丽和创造性的东西带入算法中？”

### 模型即服务

在本章中，我们将向您展示 GPT-3 如何通过为有创造力的企业家提供合适的技术来推动下一波初创企业。我们还将探讨人工智能研究如何在多个领域走向商业化。我们还将与支持这些计划的风投之一交谈，以了解蓬勃发展的 GPT-3 经济的财务方面。

OpenAI API 的创建故事与本章中许多初创企业和公司的故事相似。我们采访了 OpenAI 的产品与合作伙伴关系副总裁 Peter Welinder。他告诉我们的故事是关于大胆实验、快速迭代以及利用智能设计实现规模经济的故事，即以尽可能低的成本大规模交付强大的模型。

Welinder 用三个关键点总结了 OpenAI 的使命：“开发 AGI（通用人工智能），确保其安全，然后最后将其部署到世界，以最大化对全人类的利益。” 因此，该公司专注于开发可应用于越来越广泛需求的人工智能。

为了尽可能快而安全地实现 AGI，OpenAI 决定押注的技术之一就是大型语言模型，特别是 GPT-3。Welinder 说起尝试 GPT-3 时说：“那是我们第一次觉得，‘实际上，这似乎相当有用，它在学术基准的许多任务上都取得了最先进的结果等等。’”

对可能性感到兴奋，Welinder 和四位同事争论如何最好地使用该算法：构建翻译引擎？写作助手？客户服务应用？然后他们想到了。Welinder 说：“为什么不直接将这项技术作为 API 提供，让任何开发者在其之上构建自己的业务呢？”

API 方法与 OpenAI 的目标和使命一致，通过最大化技术的采用和影响力，赋予社区成员发明 OpenAI 团队无法预测的应用的能力。这也将产品开发留给了全球熟练的开发者，使 OpenAI 团队能够专注于他们真正擅长的事情：开发稳健、开创性的模型。

到目前为止，研究人员专注于设计可扩展、高效的训练系统，以从 GPU 中挤出最大效率。但几乎没有关注实际在真实数据上运行这些模型并为实际应用获得成果。因此，OpenAI 团队决定在核心 API 体验上加倍投入，专注于快速推理和低延迟等方面。

在计划推出 API 测试版的六个月前，他们根据 Welinder 的说法，将延迟降低到大约十倍，并将吞吐量提高了数百倍：“我们投入了大量工程，真正让这些模型发挥作用，确保其 GPU 尽可能高效，以非常低的延迟调用它们，并使其可扩展。” 通过 API 使用模型而不是需要自己的 GPU，使普通开发者以经济有效且易于访问的方式尝试用例和尝试新事物变得可行。非常低的延迟也很重要，以便于迭代。Welinder 说：“你不想输入内容然后等待几分钟才能得到响应，而这在 API 最早的日子里就是这种情况。而现在你可以看到模型实时输出内容。”

OpenAI 相信模型会越来越大，使得开发者难以部署它们；团队希望消除这一障碍。Welinder 说：“这对你来说成本太高了，因为你需要如此多的 GPU 和 CPU 来尝试一个用例。对你来说，自己部署这个模型在经济上是没有意义的。” 相反，他们决定通过 API 与开发者共享模型。Welinder 补充道：“成千上万的开发者都在使用相同的模型，这就是实现规模经济的方式。” “这会降低每个人访问这些模型的成本，并进一步扩大分发范围，让更多人可以尝试这些模型。”

以私人测试版发布 OpenAI API 带来了不少惊喜。他们之前的主打模型 GPT-2 几乎没有带来现实世界的用例，因此团队希望 GPT-3 会更有用。它确实如此，而且非常快。

另一个惊喜，Welinder 说，“我们平台上很多人都不是程序员。他们是作者，各类创作者，他们是设计师和产品经理等等。” GPT-3 在某种程度上改变了开发者的含义：突然间，原来构建人工智能应用不需要懂得编程。你只需要擅长用提示描述你希望人工智能做什么即可，如第 2 章所述。

Welinder 和他的团队发现，“经常那些真正擅长的人没有任何机器学习背景”——而那些有背景的人必须重新学习他们对很多问题的思考方式才能使用 GPT-3。许多用户在没有代码的情况下构建了基于 GPT-3 的应用。OpenAI 团队在没有真正有意的情况下降低了创建应用的门槛：迈向民主化人工智能的第一步。Welinder 说：“核心策略是让 API 尽可能多的人可以使用：‘这对我们的使命至关重要，确保使用我们技术的门槛很低。这就是我们构建这个 API 的原因。’” GPT-3 的另一个意外用例是编码。模型编码潜力的早期迹象促使 OpenAI 加倍投入为编码用例设计。他们的努力带来了 Codex，于 2021 年年中发布。¹

随着令人惊叹的多样化用例，API 还催生了一个全新的初创企业生态系统：Welinder 说：“在推出 API 几个月后，就有几家公司完全建立在 OpenAI API 之上。其中许多公司现在已以相当高的估值获得了风险投资。”

OpenAI 的核心原则之一是与客户紧密合作。Welinder 说，“每当我们有新的产品功能时，我们会尝试找到我们知道会觉得这些功能有用的客户，并建立直接的沟通渠道，让他们提前使用。” 例如，他们在 API 中更广泛发布该功能之前，与几位客户合作对搜索功能的微调。

OpenAI 主要关注确保人工智能的安全和负责任使用。除了许多积极的结果之外，他们还看到随着人工智能变得更容易被公众获取，滥用潜力正在增长。他们选择以私人测试版推出 API 的主要原因之一是了解人们将如何使用模型并检查其滥用潜力。他们尽可能检查尽可能多的不良模型行为实例，利用所学知识来指导研究和模型训练。

Welinder 从 API 驱动的项目广度和创造力中获得灵感。“未来十年将因人们在此技术之上构建的所有事物而变得令人兴奋。我认为通过共同努力，我们可以创建一些真正良好的护栏，以确保将要构建的这些技术、这些应用对我们的社会是真正积极的。”

### 新的初创生态系统：案例研究

OpenAI 发布 API 后不久，初创企业格局就充斥着使用它来解决问题公司的。这些企业家是尖端 NLP 产品领域的先驱，他们的旅程极具启发性，特别是对于任何计划基于 OpenAI API 的未来商业应用的人。本章其余部分通过对一些表现最佳初创企业领导者的采访，描绘了这一充满活力的格局



使用 GPT-3 作为其产品架构的核心，关于他们迄今为止在创意艺术、数据分析、聊天机器人、文案撰写和开发者工具等领域学到的知识。

### GPT-3 的创意应用：Fable Studio

GPT-3 最令人兴奋的能力之一是讲故事。你可以给模型一个主题，让它在零样本设置下写一个故事。

这种可能性让作家们拓展想象力并创作出非凡的作品。例如，由 Jennifer Tang 导演、与 Chinonyerem Odimba 和 Nina Segal 共同开发的戏剧 AI，描绘了人类与计算机思维之间独特的合作，得益于 GPT-3。而作者 K. Allado McDowell 在撰写他的书籍 PHARMAKO-AI 时将 GPT-3 视为合著者，McDowell 表示该书“为一个面临多重危机的世界重新构想了控制论，对我们如何在 21 世纪看待自己、自然和技术具有深远影响。”

我们与 Fable Studio 的联合创始人兼首席执行官 Edward Saatchi 以及 Fable Studio 的首席技术官 Frank Carey 坐下来，了解他们使用 GPT-3 创造一种全新互动故事类型的历程。Fable 将 Neil Gaiman 和 Dave McKean 的儿童读物《墙里的狼》改编成一部获艾美奖的 VR 电影体验。影片的主角 Lucy 可以通过 GPT-3 生成的对话与人们进行自然交流。Lucy 于 2021 年在 Sundance 电影节作为嘉宾亮相并推出了她的电影《Dracula: Blood Gazpacho》。²

Saatchi 和 Carey 注意到观众与 Lucy 建立了情感联系。这促使他们专注于利用 AI 创造虚拟生命体，以及由此产生的一种新的叙事和娱乐类别，将 AI 与讲故事编织在一起。正如 Awan 所说，“我们将拥有全新的电影类型和整体类型：我们将拥有互动、整合的体验。”

Carey 解释说，观众通常认为 AI 扮演角色的角色，就像演员一样：一个 AI 对应一个角色。相反，Fable 的 AI 是一个讲故事者，其曲目中有各种各样的角色。Carey 相信可以开发出一个像最好的人类作家一样熟练和富有创造力的 AI 讲故事者。

虽然 Lucy 的对话大多通过文字和视频聊天进行，但 Fable 也在 3D 模拟世界中尝试使用 GPT-3，以获得沉浸式 VR 体验。团队使用 AI 生成音频和手势并同步唇部动作。他们使用 GPT-3 为角色与观众互动的内容生成大量内容。其中一些内容可以预先编写，但其中很大一部分必须即时创建。Lucy 的合作者在她在 Sundance 的亮相期间以及电影制作期间都大量使用了 GPT-3。正如 Carey 所说，Lucy 的 Twitch 亮相一样，“超过 80% 的内容都是使用 GPT-3 生成的。”

这与团队早期的仅限文本的实验形成了鲜明对比，后者编写程度更高，遵循更线性的叙事。Fable Studio 团队通常不会在直播中使用 GPT-3 来处理观众不可预测的反应；他们对此的技巧早于 GPT-3。他们有时会将 GPT-3 用作写作伙伴或观众的替代品，以考虑他们的潜在反应。

Carey 解释说，GPT-3 对人类作者来说也是一个有用的工具：“对于即兴内容，我们使用 GPT-3 来进行测试，所以你可以把 GPT 当成人，而你扮演角色。与 GPT-3 来回互动可以帮助你想出，比如说，在这种情况下会有人问什么？后续会是什么？”这有助于作家涵盖尽可能多的对话结果。“有时候它是一个写作伙伴，有时候它可以填补正在发生的事情周围的空白，”Saatchi 说。“所以我们可能会想：这个角色这周会发生什么。下周会发生什么？而 GPT-3 [正在]填补其中的一些空白。”

Fable 团队在 2021 年 Sundance 电影节上进行了一项实验，充分利用了 GPT-3，在实验中 Lucy 与电影节参与者现场合作创作她自己的短片，同时 Fable Studio 和参与者正在策划她产生的想法，将其与参与者进行讨论，并将观众的想法反馈给 GPT-3。

用 GPT-3 为一个一致的角色提供动力是一个特殊的挑战。GPT-3 对于将角色重定向到参与者的用例非常有用，例如治疗会话，以及对于“关于他们有非常大的知识基础的角色，比如名人或像耶稣、圣诞老人或德古拉这样典型的角色。但显然，这在已经写过的任何信息范围内达到上限，”Saatchi 解释说，并指出任何与 GPT-3 驱动的角色广泛互动的人都会很快达到其极限。“它试图对你提出的故事给出一个好的答案。但如果你在提示中讲述一个奇幻的故事，它也会给出奇幻的答案，对吧？所以它不是一个说真话的人。我会说它本质上是一个讲故事的人；它只是试图在语言中寻找模式。”Carey 说，很多人没有意识到 GPT-3 的底线任务是讲故事，而不是“真相”：“它试图对你提出的故事给出一个好的答案。但如果你在提示中讲述一个奇幻的故事，它也会给出奇幻的答案。”

“仅仅使用 GPT-3 生成一堆随机场景是一回事，但确保它符合该角色的声音是另一回事，”Carey 补充道。“所以我们使用了一些技术来创建这些提示，以便为 GPT3 很好地定义角色。”他承认团队在确保 GPT-3 理解角色的声音并保持在其可能响应的范围内方面付出了额外努力。他们还必须避免让参与者影响角色，因为 GPT-3 可以捕捉到微妙的信号。Carey 解释说，如果 Lucy 与成年人互动，“它只会顺着这种感觉走，但 [如果] Lucy 是八岁，它可能会从参与者那里捕捉到更多成年人的感觉并反馈给他们。但我们实际上希望 [Lucy] 用八岁儿童般的声音说话”。

说服 OpenAI 允许他们使用 GPT-3 创建虚拟生命体需要一些谨慎。“我们非常有兴趣让我们的角色作为角色与人们交谈，”Carey 说。“你可以想象这可能是他们的问题领域之一，对吧？[它]绝对有可能被某人冒充人类而被恶意使用。”Fable Studio 和 Open AI 团队花了一些时间来弄清创建逼真角色和冒充人类之间的区别，然后 OpenAI 才批准了 Fable 的用例。

OpenAI 还有另一个要求：在任何虚拟生命体在观众面前假装“真实”的叙事实验中，Fable Studio 的 Fable 团队必须在过程中保持人类在环中。Carey 称，根据 Carey 的说法，让 GPT-3 与任何面向数千人的体验一起工作是具有挑战性的。话虽如此，他仍然认为大型语言模型将会大有裨益，“即使只是用于预先编写内容，或者在更宽容的领域，如果‘实时’使用且没有限制。”

Carey 相信 GPT-3 的创作最好作为协作工具，由懂得讲故事艺术并希望获得更好结果的人使用，而不是期望它完成所有工作。

就价格而言，他认为讲故事用例面临的挑战是，每次向 GPT-3 发出 API 请求以使其与不断展开的故事保持一致时，都必须“提供所有细节并生成一些内容来补充它。所以仅仅生成几行，你就会被收取整个令牌集的费用。这可能是一个挑战。”

Fable Studio 如何解决定价问题？他们在很大程度上避免了这个问题，因为主要尝试预生成，在这种情况下，“你预先生成一堆选项，然后可以使用搜索来找到正确的选项来回应，”Carey 说。

他们还找到了一种降低 API 用户数量的方法：与其让大量观众通过 AI 与 Lucy 互动，“我们有点转向了一种模式，即 Lucy 实际上是在 Twitch 直播中进行一对一的对话。”观众通过 Twitch 观看而不是进行 API 调用，这缓解了带宽问题，限制了 Lucy 在任何给定时间与之互动的人数，并扩大了受众范围。




Saatchi 提及了一个传闻，即 GPT-4 正在探索虚拟空间的空间理解，他认为这比仅限语言的聊天机器人更有潜力。他建议探索这一用例的人专注于在虚拟世界中创建角色。他指出，Replika 这家创建了虚拟 AI 朋友角色的公司，现在正在探索扩展到元宇宙³，在那里虚拟生命将拥有自己的公寓，可以相互见面和互动，最终也能与人类用户互动。“重点是打造一个感觉鲜活的角色，GPT-3 只是众多工具之一。 potentially 赋予虚拟生命对其所导航空间的真正理解，可能会为这些角色解锁学习。”

接下来会怎样？Carey 认为 GPT-3 的未来迭代在构建元宇宙方面有其位置，元宇宙是一个平行数字现实，人类可以在其中像现实世界一样自由地互动和执行活动。他设想它可以生成想法，并由人类在环中对其进行筛选。

Saatchi 认为，淡化语言作为唯一交互模式，有潜力创造与 AI 更加有趣和复杂的互动。“我确实认为 3D 空间给了我们让 AI 获得空间理解的机会，”他继续说道。Saatchi 设想的元宇宙赋予 AI 行走、探索的能力，并给人类机会成为环的一部分，帮助训练虚拟生命。他总结我们需要激进新闻”彻底改变一切，Daniel Erickson 说。Erickson 是 Viable 的创始人兼 CEO，也是 OpenAI API 的早期采用者。他解释道，“我实际上花了一个月的时间在实验上， literally 只是把我们的数据拿出来，放到 playground 里，弄清楚不同的提示等等。最终，我得出了结论，这可以驱动一个非常强大的问答系统。”

Erickson 及其同事开始使用 OpenAI API 与他们正在处理的大型数据集进行交互并从中生成洞察。他们最初使用另一种 NLP 模型，结果平平，但当他们开始使用 GPT-3 时，团队看到了“整体至少提升了 10%。当我们谈论从 80% 提升到 90% 时，对我们来说这是一个巨大的提升”。

在这一成功的基础上，他们将 GPT-3 与其他模型和系统结合，创建了一个问答功能，允许用户用纯英语提问并获得答案。Viable 将问题转换为复杂的查询，可以从数据库中提取所有相关反馈。然后它将数据通过另一系列的总结和分析模型运行，以生成精炼的答案。

此外，Viable 的系统每周为客户提供“一份 12 段的总结……概述了他们的主要投诉、主要赞美、主要请求和主要问题。”正如你从客户反馈专家那里所期望的那样，Viable 在软件生成的每个答案旁边都提供了点赞和点踩按钮。他们利用此反馈进行再训练。

人类也是流程的一部分：Viable 拥有一个标注团队，其成员负责构建训练数据集，既用于内部模型，也用于 GPT-3 微调。他们使用当前迭代的微调模型生成输出，然后由人类评估其质量。如果输出不合理或不准确，他们会重写它。一旦他们对输出列表感到满意，就会将该列表反馈到下一轮训练数据集中。

Erickson 指出，API 是一个巨大的优势，因为它将托管、调试、扩展和优化都留给了 OpenAI：“我宁愿为几乎所有不是我们技术核心的东西买单，而不是自己构建。即使它是我们技术的核心，使用 GPT-3 对我们来说仍然有意义。”因此，他们的理想解决方案是使用 GPT-3 处理流程中的所有元素。但由于成本原因，他们必须优化使用：“我们有公司向我们提供数十万个数据点，每个数据点从五到一千个词不等。”如果对所有内容都使用 GPT-3，成本会很高。

相反，Viable 主要使用内部模型来结构化数据，这些模型是在 BERT 和 ALBERT 之上开发的，并使用 GPT-3 输出进行训练。这些模型现在在主题提取、情感和情绪分析以及许多其他任务上达到了或超过了 GPT-3 的能力。Viable 还切换到基于使用量的定价模式，该模式建立在 OpenAI 的 API 定价之上。

Erickson 坚持认为，GPT-3 在两个方面让 Viable 领先于竞争对手：准确性和可用性。我们已经提到了 Viable 令人印象深刻的 10% 准确度提升。但可用性呢？Viable 的大多数竞争对手构建的工具专门为专业数据分析师设计。Viable 认为这受众太窄：“我们不想构建只有分析师才能使用的软件，因为我们觉得这限制了价值。我们想做的是帮助团队利用定性数据做出更好的决策。”

相反，Viable 的软件本身就是“分析师”。用户可以更快地迭代，得益于一个反馈循环，允许他们用自然语言询问有关数据的问题并获得快速准确的响应。

Erickson 分享了 Viable 的一些下一步计划：他们很快将引入定量数据并 crunch 产品分析。最终，Erickson 希望赋予用户执行完整的客户洞察分析的能力，并提出诸如“有多少客户在使用功能 X？”和“使用功能 X 的客户中，他们认为它应该如何改进？”等问题。

最终，Erickson 总结道，“我们销售的是生成的洞察。因此，我们让这些洞察越深入、越强大，以及越快交付这些洞察，我们创造的价值就越大。”

### GPT-3 的聊天机器人应用：Quickchat

GPT-3 在语言交互方面非常熟练，是增强现有聊天机器人体验的显而易见选择。虽然许多应用用 AI 聊天机器人角色娱乐用户，例如 PhilosopherAI 和 TalkToKanye，但有两家公司专门在其业务应用中利用这一能力：Quickchat 和 Replika。Quickchat 以其 AI 聊天机器人角色 Emerson AI 而闻名，可通过 Telegram 和 Quickchat 移动应用程序访问。Emerson AI 拥有广泛的通用世界知识，包括在 GPT-3 训练时间之后获取的更近期信息；支持多种语言；能够进行连贯的对话，并且很有趣。

Quickchat 的联合创始人 Piotr Grudzień 和 Dominik Posmyk 从一开始就对 GPT-3 感到兴奋，并对如何将其用于新产品充满想法。在他们早期使用 OpenAI API 的实验中，他们不断回到“机器与人之间演进的界面”这一概念。Grudzień 解释说，由于人类与计算机之间的交互不断演进，自然语言将是合乎逻辑的下一步：毕竟，人类更喜欢通过对话进行交流。



GPT-3，他们得出结论，似乎有潜力实现人与计算机之间类人化的聊天体验。

Grudzień 表示，两位创始人之前都没有构建过任何传统的聊天机器人应用。以“初学者的心态”[content]
GPT-3，他们得出结论，似乎有潜力实现人与计算机之间类人化的聊天体验。

Grudzień 表示，两位创始人之前都没有构建过任何传统的聊天机器人应用。以“初学者的心态”来处理任务，帮助他们保持思维新鲜并对解决问题持开放态度。与其他聊天机器人公司不同，他们并没有以成为最好的客户支持或营销工具为雄心开始。他们开始时想的是：“我如何让人类以一种令人惊叹的方式与机器交谈，并且是他们尝试过的最好的东西？”他们想要打造的聊天机器人不仅仅是完成收集客户数据和提供答案等简单功能，还能回答未编排的客户问题或进行愉快的闲聊。“与其说：‘我不知道’，”Grudzień 补充道，它可以“退回到对话 API 并继续对话。”

Posmyk 补充道，“我们的使命是让人们借助人工智能获得赋能，而不是取代他们。我们相信，在未来十年，人工智能将加速教育、法律、医疗等关键行业的数字化，并在工作和日常生活中提高我们的生产力。”为了展示这一看似遥远的使命，他们创建了 Emerson AI，一个由 GPT-3 驱动的智能通用聊天机器人应用。

尽管 Emerson AI 拥有不断增长的用户社区，但其真正目的是展示 GPT-3 驱动聊天机器人的能力，并鼓励用户与 Quickchat 合作，为其公司实施此类人格化。Quickchat 的产品是一款通用对话式 AI，可以谈论任何主题。客户，主要是成熟公司，可以通过添加特定于其产品的信息（或他们想要的任何主题）来自定义聊天机器人。Quickchat 见证了多样化的应用，例如自动化典型的 FAQ 客户支持问题解决，以及实施 AI 人格来帮助用户搜索内部公司知识库。

与典型的聊天机器人服务提供商不同，Quickchat 不构建任何对话树或僵化场景，也不必教聊天机器人以特定方式回答问题。相反，Grudzień 解释说，客户遵循一个简单的流程：“你复制粘贴包含你希望 AI 使用的所有信息的文本，然后点击重新训练按钮，这需要几秒钟来吸收知识，就这样。”现在在你的数据上训练完成，聊天机器人就可以进行测试对话了。

被问及开源模型与 OpenAI API 之间的权衡时，Grudzień 分享说，“OpenAI API 很棒且易于使用，因为你不必担心基础设施、延迟或模型训练。只需调用 API 并获得答案即可。非常可靠。”然而，他认为你为质量付出的价格相当高。作为对比，开源模型似乎是很好的免费替代方案。实际上，“你确实需要支付云计算的成本。它需要 GPU，并且需要设置 GPU 才能让这些模型快速运行，然后进行你自己的微调”——Grudzień 承认，这不是一个简单的过程。

像 Viable 的 Ericksen 一样，Grudzień 和 Posmyk 努力确保每次 API 调用都能提供价值。但他们也希望，随着越来越多的竞争模型发布，OpenAI 的 API 定价会“下降或稳定在某个水平，只是因为竞争的压力。”

那么 Quickchat 学到了什么？首先，光靠炒作不足以建立盈利的业务。像 GPT-3 这样的大型媒体轰动，可以带来最初的一波兴奋的爱好者，“但随后人们会感到厌倦并等待下一个大事件。唯一能够存活的产品是那些真正解决人们关心的问题的产品，”Grudzień 说。“没有人会仅仅因为它是 GPT-3 就使用你的产品。它需要提供一些价值，无论是有用还是有趣，或解决某些问题。GPT-3 不会为你做到这一点。所以你需要把它仅仅当作又一个工具。”

另一个关键教训是开发可靠的性能指标。“每当你构建机器学习产品时，评估总是很棘手，”Grudzień 说。在他看来，由于 GPT-3 强大且运行在难以量化的自然语言领域，评估其输出质量是复杂而繁琐的。突破可能令人兴奋，他说，“但用户很可能会根据你的最差表现来评判你，最多根据你的平均表现来评判。”因此，Quickchat 优化用户满意度。对他们来说，设计一个指标来捕捉与满意用户和高留存率相关的变量至关重要，这两者直接转化为更高的收入。

另一个挑战，或许令人惊讶的是，GPT 的创造力。“即使你将温度设得很低，无论你给出什么提示，它仍然会使用那个微小的提示，然后基于它所拥有的庞大知识生成一些东西，”Grudzień 解释道。这使得生成诗歌、营销文案或幻想故事等创意文本变得很容易。但大多数聊天机器人是为解决客户问题而设计的。“它需要具有可预测、重复的性能，同时仍然具有对话性，并在一定程度上具有创造性，但不要过度。”

大型语言模型有时会输出“奇怪的”、“空洞的”或只是“不怎么样”的文本，因此人类确实需要介入。“如果你开始衡量它是否满足了某些条件或完成了任务，那么就会发现它确实很有创造力，但在 10 次尝试中只有 6 次成功——这对于涉及付费客户的真实业务来说几乎等于零。”因此，对于成功的业务应用，你需要大量内部系统和模型来约束这种创造力并增强可靠性。“为了为我们的客户创建这个工具，使其 99% 的时间都能正常工作，我们开发了许多防御机制，”Grudzień 说。

如今，Quickchat 专注于与客户深入合作，确保他们的 API 性能能够在他们的使用场景中取得成功。Grudzień 最兴奋的是看到客户构建的东西：“我们真的、真的很想看到我们的聊天引擎在成千上万种不同的方式中被用于不同的产品。”

### GPT-3 的营销应用：Copysmith

GPT-3 能消除写作障碍吗？Kilcher 认为可以：“如果你有写作障碍，你只需询问模型，它就会从这样的模型中为你提供一千个想法，仅仅作为创意辅助工具。”让我们看一个这样的工具：Copysmith。

GPT-3 最受欢迎的应用之一是即时生成创意内容。Copysmith 是市场上领先的内容创作工具之一。联合创始人兼 CTO Anna Wang 表示，“Copysmith 使用户能够通过强大的 AI 在网络上的任何地方比以往快一百倍地创建和部署内容。”它使用 GPT-3 用于电子商务和营销的文案写作，以闪电般的速度生成、协作和发布优质内容。联合创始人兼 CTO Wang 和 CEO Shegun Otulana 分享了两位姐妹如何将她们的小型、 struggling 的电子商务商店转变为成功的科技公司——以及 GPT-3 在其中发挥的关键作用。

2019 年 6 月，Anna Wang 和她的姐姐 Jasmine Wang 联合创立了一家基于 Shopify 的精品店。但她们缺乏营销经验，“生意彻底崩溃了”，Anna Wang 说。当姐妹俩在 2020 年了解到 OpenAI API 时，Wang 说，“我们开始探索它用于创作诗歌、尝试模仿书籍和电影中的角色等创意追求。有一天我们意识到，如果我们在尝试建立电子商务商店时拥有这个工具，我们就能够写出更好的行动号召、产品描述，并提升我们的营销水平，让它起步。”

受此启发，她们于 2020 年 10 月推出了 Copysmith，得到了热烈反响。Anna Wang 的话说，“一切就从这里开始。我们开始与用户交流，并根据反馈迭代产品。”GPT-3，她指出，允许你在没有任何先验知识的情况下快速迭代，而其他开源模型，如 BERT 和 RoBERTa，则需要对每个下游任务进行大量微调。“它在可以执行的任务方面极其灵活，”她补充道，“而且是目前最强大的模型。”更重要的是，GPT-3 “对开发人员和用户都非常友好，其简单的文本输入文本输出界面允许你使用简单的 API 执行各种任务。”其另一个优势是 API 调用的简单性，与托管专有模型所付出的努力相比。





就基于 GPT-3 构建产品的挑战而言，Otulana 说：“你通常受制于 OpenAI 的限制。因此，要克服这一点，你必须为 API 注入自己的创业[content]

就基于 GPT-3 构建产品的挑战而言，Otulana 说，“你通常受制于 OpenAI 的限制。因此，要克服这一点，你必须为 API 注入自己的创业[content]

就基于 GPT-3 构建产品的挑战而言，Otulana 说，“你通常受制于 OpenAI 的限制。因此，要克服这一点，你必须为 API 注入自己的创业触感，以创造出脱颖而出的东西。另一个限制是控制力略有丧失，你的进展本质上受限于 OpenAI 的进展。”

Anna Wang 对想要使用 GPT-3 的准产品设计师有两个建议。首先，她说，“确保你解决的是一个真正的问题……想想你的用户，因为 GPT-3 最容易让人陷入的思维误区就是在安全指南的限制内构建事物，而不允许自己发挥创造力。”

其次，Wang 建议，“密切关注你向模型输入的内容。注意标点、语法和提示的措辞。我保证你会获得更佳的模型输出体验。”

### GPT-3 的编码应用：速记

随着 GPT-3 及其衍生模型 Codex 在与编程和自然语言交互方面的能力不断提升，新的潜在用例层出不穷。

Bram Adams 是 OpenAI 社区大使，以其对 GPT-3 和 Codex 算法的创造性实验而闻名，他在 2021 年底推出了其中一个项目：Stenography，该项目利用 GPT-3 和 Codex 来自动化编写代码文档这一令人厌烦的任务。Stenography 瞬间取得成功，在热门产品发布门户 ProductHunt 上以当日的第 1 名产品发布。

Adams 在此之前尝试了 API 的多个潜在用例，最终将想法缩小到成为他新业务的那个项目。“我认为这些实验很多都是我在无意识地测试像 GPT-3 这样的语言模型能处理什么。” Adams 的探索始于这个想法：“如果我能让计算机做任何事，我会做什么？”他开始探索，“在 OpenAI API 的边缘处试探，看看它能走多远。”他制作了一个生成 Instagram 诗歌的机器人；

尝试了一个自播客式的日记项目，用户可以与数字化的自己对话；基于用户偏好在 Spotify 上开展了一个音乐播放列表构建项目；并为满足好奇心创建了更多项目。多亏了这种好奇心，“我很早就对理解 GPT-3 的不同引擎变得非常擅长。”

那么为什么是 Stenography？“我从外部世界得到了相当好的信号，认为这对很多人会非常有帮助。” Adams 喜欢优雅编写的代码，但大多数 GitHub 用户只是下载已发布的代码并使用它：“没有人会真正欣赏你代码库中投入的美感。”他还注意到，GitHub 上那些文档不佳的优秀程序往往得不到应有的关注：“readme 文件是每个人首先看到的东西。他们会立即向下滚动查看。” Stenography 是尝试思考文档如何演进以对开发者来说不那么烦人：“这很难，因为对于文档来说，你必须为自己所做的事辩护。所以你会说，‘我用这个库来做这件事。然后我决定使用这个东西，然后我添加了这个函数来做这件事。’”

Adams 将文档视为人们与团队中的其他人、未来的自己，或只是偶然发现该项目的人进行沟通的方式。它的目标是让项目对他人可理解。”我对 GPT-3 是否能创建可理解的注释的想法很感兴趣。”他尝试了 GPT-3 和 Codex 两个模型，都对两者的解释水平印象深刻。他接下来问的问题是，“我如何让这对开发者来说真正简单且愉快？”

那么 Stenography 如何工作，其组件如何利用 OpenAI API？在高层次上，Adams 说，有两个主要流程，解析和解释，每个都需要不同的策略。“对于解析过程，我花了很多时间理解代码的复杂性，因为你代码中的并非所有行都值得记录。”某些代码可能目的明显，没有操作价值，或不再有用。

此外，“大”代码块，超过 800 行，对模型来说一次理解太棘手。“你必须将该逻辑分解为许多不同的步骤，才能准确地说出这个东西做什么。一旦我理解了这一点，我开始思考，‘我如何利用解析来找到足够复杂但又不过于复杂的块？’”由于每个人写代码的方式不同，这就变成了一项试图附加到抽象语法树并利用现有最佳资源的工作。这成为了解析层的核心架构挑战。

至于解释层，“那更多是让 GPT-3 和 Codex 说出你想要它们说的话的功能，”Adams 解释道。做法是找到创造性的方法来理解代码的受众，并让 GPT-3 与之对话。这一层“可以尝试解决任何问题，但它可能无法像计算器那样以百分之百的准确率解决。如果你输入二加二等于四，有时会得到五，但你不需要为乘法、除法和减法编写所有函数。那些是免费附带的。”这就是概率系统的权衡：有时它们有效，有时无效，但它们总是返回答案。Adams 建议保持足够的灵活性，以便在必要时调整策略。

Adams 强调在开始使用 OpenAI API 之前真正理解问题的重要性。“在我的办公时间里，人们会来，他们会有这些巨大的问题。他们会说，‘我如何从零开始用提示构建火箭？’而我会说，‘好吧，火箭有很多组件。GPT-3 不是万能药。它是一台非常强大的机器，但只有当你知道自己用它做什么时才如此。’”他将 GPT-3 比作 JavaScript、Python 和 C 等编程语言：“它们很有吸引力，但只有当你理解递归和 for 循环以及 while 循环，以及哪些工具可以帮助你解决特定问题时才如此”对 Adams 来说，这意味着提出大量“技术元问题”，例如“是什么东西通过 AI 文档得到帮助？”以及“文档到底是什么？”处理这些问题是他面临的最大挑战。

“我认为很多人只是立刻冲向 Davinci 来解决他们的问题。但如果你能在更小的引擎上解决某些问题，比如 Ada、Babbage 或 Curie，你实际上会比如果你只是用 Davinci 把整个 AI 扔上去更深入地了解问题。”他声称。

谈到使用 OpenAI API 构建和扩展产品时，他建议使用“小型引擎或较低温度，因为你无法预测最终的提示会是什么样（或是否会随着时间演变），你想做什么，以及你的终端用户是谁，但使用更小的引擎和更低的温度，你会更快找到真正困难问题的答案。”他告诉我们。

另一个挑战是从他自己的独立实验转向考虑用户可能面临的所有不同条件和工作方式。现在他正在努力“找到所有不同的边缘情况”，以便更好地理解 API 设计层需要多快、需要多频繁地响应特定请求，以及它如何与不同语言交互。

Stenography 的下一步是什么？现在 Adams 已经构建了他非常满意的产品，他计划在 2022 年专注于销售并与用户群交流。“Stenography 不再是关于构建，而是真正完善产品并让它呈现在人们面前。”

### 投资者对 GPT-3 初创生态系统的看法

为了了解支持基于 GPT-3 的公司的投资者的观点，我们采访了 Wing VC 的 Jake Flomenberg，Wing VC 是一家知名全球风险投资公司，也是多家 GPT-3 驱动初创公司的领投方，包括 Copy.AI 和 Simplified。

正如任何市场观察者可能想象的那样，风险投资家正在关注 GPT-3 等新兴 AI 技术。Flomenberg 总结了其吸引力：GPT-3“是我们以前从未见过的任何其他 NLP 模型。它是一个



substantial step in the direction of building more generalized AI.” The untapped potential is enormous, he argues, and the business world still “underestimates and therefore underutilizes the capabilities of LLMs.”

But how should potential investors evaluate something so new and different? “We value startups with a deep understanding of the problem, the domain, and the technology” as well as a good fit between product and market, Flomenberg says. “The nuance in assessing something built on GPT-3 is asking, what’s the secret sauce? What is it that the company has built a technologically deep knowledge on? Is the company solving a real problem using GPT-3, or just leveraging the hype to get their product out in the market? Why now? Why is this team the best fit to execute this idea? Is this idea defensible in the real world?” If a startup can’t defend its existence, that’s a huge red flag for investors.

Investors also keep a close eye on OpenAI and its API, since GPT-3-based businesses rely completely on its capabilities. Flomenberg cites OpenAI’s due-diligence review process as a major factor in this trust-based relationship: “The startups that pass the production review and are a subject of interest by OpenAI automatically become hot for investment.”

Investors usually dig into the background and expertise of founders while making investment decisions. GPT-3 is unusual, though, in that it allows people from any background, not just programmers, to build cutting-edge NLP products. Flomenberg stresses the importance of the market here: “Generally with a deep tech startup, we look for founders with a great understanding of technical and AI domain. But with GPT-3-based startups, we are more focused on whether the market resonates with the founders’ vision and whether they’re able to identify and address the needs of the end-users”. He cites Copy.AI as “a classic example of a product-led-growth model built on top of GPT-3. They found an extraordinary resonance with their users and developed a deep understanding of the technology, bringing depth and value to the table.” Successful startups, he says, “keep the AI inside,” focusing more on solving users’ problems and meeting their needs by using the right tool for the job.

### Conclusion

It’s mind-blowing to see these use cases, and many more, built on top of GPT-3 so quickly and with such success. By late 2021, when this chapter was written, several startups in the OpenAI community had already raised hefty rounds of funding and were looking at rapid expansion plans. This market tide seems to have awakened the appetites of bigger businesses as well. More and more enterprises are starting to consider implementing experimental GPT-3 projects within their organizations. In Chapter 5, we will look at this market segment consisting of large-scale products like GitHub Copilot and particularly the new Microsoft Azure OpenAI Service, which is designed to meet the needs of large-scale organizations.

- 1 For a brief explanation, see this blog post by OpenAI; for a deeper dive, see the development team’s research paper.
- 2 You can watch Dracula on Vimeo; a Fable Studios blog post also offers a behind-the-scenes overview.
- 3 Metaverse, in this context, refers to a futuristic concept of a network of 3D virtual worlds filled with virtual avatars that focus on social connection. The broader concept is independent of Mark Zuckerberg’s vision for a metaverse as a platform for Meta.
