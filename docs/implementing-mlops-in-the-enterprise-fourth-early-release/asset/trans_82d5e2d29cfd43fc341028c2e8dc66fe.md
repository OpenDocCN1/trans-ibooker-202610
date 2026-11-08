
# 使用 AWS Comprehend 和 Translate 进行 NLP 实战

## 使用 AWS Comprehend 进行情感分析

现在，让我们动手实践一些 NLP。首先从 [UCI 机器学习知识库](https://archive.ics.uci.edu/ml/datasets/Sentiment+Labelled+Sentences) 下载 [情感标注句子](https://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip) 数据集。下载后，打开 `yelp_labelled.txt` 文件。接下来，导航到 [AWS Comprehend 界面](https://aws.amazon.com/comprehend/)。要查看 AWS Comprehend 的实际效果，请将 `yelp_labelled.txt` 文件中的一行粘贴到 AWS Comprehend 控制台中，如 [图 8-5](#ch08.xhtml#Figure-9.8-comprehend-sentiment) 所示。该平台允许您快速原型设计和可视化 NLP 任务，例如情感分析。从传统的笔记本或 `Python SDK` 方法转向更直接、用户友好的界面，体现了 AWS 致力于在 MLOps 工作流程中提供实用且易于访问的 NLP 工具的承诺。

该图提供了 AWS Comprehend 控制台界面的图形表示。此基于 Web 的用户界面允许您在无需编程或命令行交互的情况下与 AWS Comprehend 交互。您可以直接将文本数据输入控制台，服务会分析这些数据以识别关键情感指标。

![](assets/9.8-comprehend-sentiment.png)

本示例中，一句来自 Yelp 标注数据集的句子被输入到控制台中。控制台的输出显示了来自 AWS Comprehend 的情感分析结果。它展示了检测到的主要情感（积极、消极、中性或混合），以及与每种情感相关的置信度分数。这些分数反映了模型为每个情感类别分配的概率。这种可视化是 AWS Comprehend 如何让您快速原型设计和测试情感分析任务的绝佳示例，使其成为 MLOps 的宝贵工具。它展示了从数据输入到有洞察力的结果的轻松过渡，所有这些都在用户友好且直观的界面中完成。

### 注意

情感分析，有时称为 *观点挖掘*，是自然语言处理（NLP）的一个方面，关注预测词语的情感基调。这种推断评估文本数据中表达的态度、观点和情感。该技术常见于客户之声（VoC）分析、品牌监控和社交媒体监控等领域。它是理解公众情绪的有力工具，与我们围绕时间序列分析和预测所探索的概念密切相关。当与 AWS Comprehend 等机器学习服务或 Hugging Face 等工具配对时，情感分析可以提供有关趋势的宝贵见解，帮助组织做出明智的决策。正如我们使用 `AWS CLI` 进行时间序列预测一样，AWS Comprehend 也可通过类似方式用于情感分析，凸显了这些平台的多功能性。

我们之前利用 `AWS CLI` 进行时间序列预测。另一项服务 AWS Comprehend 可以使用 `CLI` 或 `SDK` 执行情感分析，进一步证明了这些预训练模型服务在 MLOps 上下文中的适应性和实用性。

## 使用 Python SDK 进行情感分析

现在，让我们进入动手实践部分。必须了解，尝试使用 `AWS Console` 和 `Python SDK` 可提供宝贵的反馈循环。这种迭代过程使您更好地了解 AWS 服务的内部运作，从而更轻松地将其集成到 MLOps 工作流程中。以下是一个很好的示例：

```
import boto3

# initialize comprehend client
comprehend = boto3.client(service_name='comprehend', region_name='your_aws_region')

# input text for sentiment analysis
text = 'Crust is not good.'

# detect the sentiment of the input text
response = comprehend.detect_sentiment(Text=text, LanguageCode='en')

# print the sentiment score
print(response['SentimentScore'])
```

## 使用 AWS Translate 进行文本翻译

让我们再次使用这种方法开始翻译一些文本，首先使用控制台，如 [图 8-6](#ch08.xhtml#Figure-9.9-translate) 所示。该确切的文本字符串从英语翻译成葡萄牙葡萄牙语。

![](assets/9.9-translate.png)

接下来，让我们构建几个 Python 函数来帮助我们翻译文本——所有源代码都位于此 [仓库](https://github.com/nogibjj/assimilate-aws) 中。首先，让我们创建一个列出可用语言的函数，以构建一个为最终用户提供多种选项的命令行工具：

```
def list_languages():
    """List available languages"""

    client = boto3.client("translate")
    result = client.list_languages()
    return result["Languages"]
```

以下函数翻译文本：

```
def translate_text(text, source, target):
    """Translate text from source to target language"""

    client = boto3.client("translate")
    result = client.translate_text(
        Text=text, SourceLanguageCode=source, TargetLanguageCode=target
    )
    return (
        result["TranslatedText"],
        result["SourceLanguageCode"],
        result["TargetLanguageCode"],
    )
```

接下来，使用两个子命令构建一个 [click 命令行工具](https://github.com/nogibjj/assimilate-aws/blob/main/translator_cli.py)，其中每个子命令映射到这些函数：

```
#!/usr/bin/env python

from awstools.translatelib import translate_text, list_languages
import click
from random import choices


# build out click group
@click.group()
def cli():
    """A simple command line interface for AWS Translate"""


# build out click command to list languages
@cli.command("languages")
def cli_list_languages():
    """List available languages"""

    colors = ["red", "green", "blue", "yellow", "magenta", "cyan", "white"]
    languages = list_languages()
    for language in languages:
        # randomly select a color
        color = choices(colors)
        # print the language name in the randomly selected color
        result = f"{language['LanguageName']}, {language['LanguageCode']}"
        click.secho(result, fg=color[0])


@cli.command("translate")
@click.argument("text")
@click.option("--source", default="en", help="Source language")
@click.option("--target", default="es", help="Target language")
def translate(text, source, target):
    """Translate text from source to target language

    Example:
    ./translator_cli.py translate "Hello World" --source en --target es

    """

    text, source, target = translate_text(text, source, target)
    # use colored text to highlight the source and target languages
    click.secho("Source: {}".format(source), fg="blue")
    click.secho("Target: {}".format(target), fg="yellow")
    click.secho(text, fg="white")


# run the cli
if __name__ == "__main__":
    # pylint: disable=no-value-for-parameter
    cli()
```

语言翻译是 NLP 的一个重要组成部分，也是 MLOps 专业人员经常遇到的另一项任务。在多语言数据环境中，了解和翻译跨语言的数据对于全球数据驱动决策至关重要。AWS、Google Cloud 和其他云服务为此提供了解决方案，但工具的选择很大程度上取决于您的具体需求和项目的背景。

[图 8-7](#ch08.xhtml#Figure-9.10-colored-languages) 中的彩色输出展示了我们所选工具支持的语言的多样性。这种可视化是通过随机化交替行来产生彩虹颜色效果的简单技巧创建的（请注意，印刷版读者需要在终端上自行运行才能看到颜色。

![](assets/9.10-colored-languages.png)

接下来，我们使用自定义构建的 Python 工具演示命令行翻译示例。以下命令将短语“Crust is not good”从英语（`en`）翻译成葡萄牙语（`pt-PT`）。

```
python translator_cli.py translate --source en --target pt-PT "Crust is not good"
```
