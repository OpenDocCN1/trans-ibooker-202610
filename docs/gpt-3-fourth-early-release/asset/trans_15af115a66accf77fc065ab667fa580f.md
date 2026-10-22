

## NOTE

对于刚开始使用 GPT-3 且对 token 消耗难以理解的初学者来说。许多用户输入过长的提示文本，这会导致积分过度使用，进而产生计划外的费用。为避免这种情况，在最初的几天里，请尝试使用 API 控制面板观察所消耗的 token 数量，并查看提示和完成的长度如何影响 token 使用情况。这可以帮助你防止积分失控使用，并将一切保持在预算范围内。

## GPT-3 在传统 NLP 任务上的表现

GPT-3 是 NLP 领域高度先进且复杂的继任者，使用核心 NLP 方法和深度神经网络构建并训练。对于任何基于 AI 的建模方法，模型性能的评估方式如下：首先，你在训练数据上为特定任务训练模型（如分类、问答、文本生成等）；然后使用测试数据（未见过的数据）验证模型性能。

类似地，存在一套标准的 NLP 基准用于评估 NLP 模型的性能并得出相对模型排名或比较。这种比较，或 *相对排名*，使你能够挑选和选择最适合特定 NLP 任务（业务问题）的模型。

![](img/495f94a597dee9cc5b6b1e9654aa4339_58_0.png)

图 2-11. 传统 NLP 任务

在本节中，我们将讨论 GPT-3 在一些标准 NLP 任务上的表现，并将其与类似模型在各自 NLP 任务上的表现进行比较。

## 文本分类

文本分类是将文本归类到有组织的组中的过程。通过使用 NLP，文本分类可以自动分析文本，然后根据其上下文分配一组预定义的标签或类别。

文本分类涉及分析作为输入提供的文本并为其分配标签、分数或其他表征文本的属性。文本分类的一些常见示例是情感分析、实体识别以及异常或离群点检测，我们将在本节后面部分讨论这些内容。

你可以使用多种方法让 GPT-3 对文本进行分类，从零样本分类（你不向模型提供任何示例）到单样本和少样本分类（你向模型展示一些示例）。

## 零样本分类

现代人工智能长期以来一直致力于开发能够对从未见过的数据执行预测功能的模型。这个重要的研究领域称为零样本学习。类似地，*零样本分类* 是一种分类任务，其中模型在对文本片段进行分类时无需事先训练和微调。GPT-3 目前对未见数据的结果要么优于为特定目的微调的最先进 AI 模型，要么与之相当。为了使用 GPT-3 执行零样本分类，我们必须为其提供兼容的提示。在第 2 章中，我们将更详细地讨论提示工程。

以下是零样本分类的一个示例，其目标是执行事实核查分析，以确定推文中包含的信息是否正确或不正确。图 2-12 显示了基于零样本示例的信息正确性分类的相当令人印象深刻的结果。

![](img/495f94a597dee9cc5b6b1e9654aa4339_59_0.png)

图 2-12. 零样本分类示例

以下是我们的提示：

```
Analyse the tweet in terms of information correctness.
Tweet: "More than 50% of global scientists don’t believe in climate change."
Analysis:
```

输出结果为：

```
The tweet is incorrect.
```

## 单样本/少样本分类

文本分类的另一种方法是使用单样本或少样本训练示例对 AI 模型进行微调，也称为单样本或少样本文本分类。当你提供如何对文本进行分类的示例时，模型可以根据你提供的样本学习有关对象类别的信息。这是零样本分类的超集，允许你通过向模型提供一些示例来对文本进行分类。这对于需要一定程度上下文设置的下游用例尤其有用。

让我们来看一个少样本分类的以下示例。我们要求模型执行推文情感分析分类，并提供三个推文示例来说明每种可能的标签：积极、中性和消极。正如以下屏幕截图所示，配备了基于少数示例的如此详细上下文的模型，能够非常轻松地执行下一条推文的情感分析。

## NOTE

当你从书中重现提示示例或创建自己的提示时，请确保在提示中保持足够的行间距。段落后额外的一行可能会导致非常不同的结果，因此你需要尝试一下，看看什么对你最有效。

以下是我们的提示：

```
Analyse the tweet in terms of its sentiment. Depending on the sentiment, classify it as positive, neutral, or negative.
Tweet: "I'm seriously worried that super intelligent AI will be disappointed in humans."
Sentiment analysis(positive, neutral, negative): negative
Tweet: "I cannot wait for super intelligent AI to emerge and deepen our understanding of the Universe."
```

Sentiment analysis(positive, neutral, negative): positive
Tweet: "I think it is neither super likely nor super unlikely that the super intelligent AI will emerge one day."
Sentiment analysis(positive, neutral, negative): neutral
Tweet: "Super intelligent AI is going to be the most exciting discovery in human history."
Sentiment analysis(positive, neutral, negative):

# And the output:

positive

![](img/495f94a597dee9cc5b6b1e9654aa4339_61_0.png)

## 批量分类

在了解了 GPT-3 的少样本分类之后，让我们深入研究批量分类，它允许你在一个 API 调用中批量分类输入样本，而不是每次 API 调用只分类一个示例。它适用于你希望一次性对多个示例进行分类的应用，就像我们检查过的推文情感分析任务一样，但是一次分析几条推文。

与少样本分类一样，你希望为模型提供足够的上下文以实现所需结果，但采用批量配置格式。在这里，我们使用批量配置格式中的各种示例定义推文情感分类的不同类别。然后我们要求模型分析下一批推文。

以下是我们的提示：

```
Analyse tweets in terms of their sentiment. Depending on their sentiment, classify them as positive, neutral, or negative.
Tweet: "I'm seriously worried that super intelligent AI will be disappointed in humans."
Sentiment analysis(positive, neutral, negative): negative
###
Tweet: "I cannot wait for super intelligent AI to emerge and deepen our understanding of the Universe."
Sentiment analysis(positive, neutral, negative): positive
###
Tweet: "I think it is neither super likely nor super unlikely that the super intelligent AI will emerge one day."
Sentiment analysis(positive, neutral, negative): neutral
###
Tweet: "Super intelligent AI is going to be the most exciting discovery in human history."
Sentiment analysis(positive, neutral, negative): positive
###
Tweet:
1. "I'm seriously worried that super intelligent AI will be disappointed in humans."
2. "I cannot wait for super intelligent AI to emerge and deepen our understanding of the Universe."
3. "I think it is neither super likely nor super unlikely that the super intelligent AI will emerge one day."
4. "Super intelligent AI is going to be the most exciting discovery in human history."
5. "This is the latest report on the state of the AI"
Tweet sentiment:
1. negative
2. positive
3. neutral
4. positive
5. neutral
Tweet:
1. "I can't stand bad techno music"
2. "This is a tweet"
3. "I can't wait for going to the Moon!!!"
4. "AI is super cute ♡"
5. "Got VERY ANGRY now!!! 7줄Tweet sentiment:
1.
```
