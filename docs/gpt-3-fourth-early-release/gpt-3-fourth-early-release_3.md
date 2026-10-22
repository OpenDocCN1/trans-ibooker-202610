## 第 3 章. 使用 GPT-3 编程

### 早期发布读者的说明

使用 Early Release 电子书，你可以在官方发布这些标题之前很久就利用这些技术——作者在写作时提供原始未编辑的内容。

这将是最终书籍的第 3 章。请注意，GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果在本章中发现缺失的材料，请联系作者 sandra.a.kublik@gmail.com 和 shubhamsaboo111@gmail.com。

几乎所有 GPT-3 的 NLP 能力都是在 Python 编程语言中创建的。但为了符合 OpenAI 使公司使命实现广泛的 AI 可访问性，API 设计为语言无关，提供灵活性，使用用户选择的编程语言构建 GPT-3 驱动的应用程序。

在本节中，我们将讨论如何使用不同的编程语言使用 GPT-3，通过用不同的编程语言复制一个示例。只是提前说明：在本章中，我们假设你对所讨论的编程语言有基本了解。如果你没有，可以安全地跳过该部分。

### 使用 Python 使用 OpenAI API

Python 是数据科学和机器学习任务中最流行的语言。在 R 和 Stata 等传统数据科学编程语言中，Python 因其可扩展性和与数据库的良好集成而脱颖而出。它被广泛使用，并且拥有一个蓬勃发展的开发者社区来保持其生态系统更新。Python 易于学习，并附带了有用的数据科学库，如 numpy 和 pandas。

现在我们将看到如何使用一个名为 Chronological 的库使用 Python 使用 GPT-3（可从此 Github 仓库获得）。该库为用户提供简单直观的界面。每当你在 Python 中构建 GPT-3 驱动的应用程序时，它可以减轻每次从头编写整个代码的单调工作。其功能包括：

- 它异步调用 OpenAI API，允许你同时生成多个提示补全。
- 你可以轻松创建和修改训练提示；例如，修改不同示例使用的训练提示相当直接。
- 它允许你通过将一个提示的输出插入到另一个提示中来串联提示。

Chronology 已托管在 PyPI 上并支持 Python 3.6 及以上版本。要安装该库，可以运行以下命令：

```
(base) PS D:\GPT-3 Python> pip install chronological
```

通过 PyPI 安装 Python 库后，让我们来看一个文本摘要示例的实际操作，我们将看到如何调用 API，将训练提示作为请求发送，并将摘要补全作为输出获取。你可以在以下仓库找到所有代码。

在本示例中，我们将展示如何引导 GPT-3 将给定的文本文档摘要到一个二年级学生可以轻松理解的水平。我们将使用以下训练提示：

```
My second grader asked me what this passage means:
```

```
"""
Olive oil is a liquid fat obtained from olives (the fruit of Olea europaea; family Oleaceae)...
"""
I rephrased it for him, in plain language a second grader can understand:
"""
```

你可以在以下链接找到完整的提示。

要开始执行文本摘要任务，让我们首先导入以下依赖项：

```
#### Importing Dependencies
from chronological import read_prompt, cleaned_completion, main
```

导入依赖项后，让我们创建一个读取训练提示并为我们提供补全输出的函数。我们将使用以下配置用于 API 参数：

```
Maximum tokens=100, Execution Engine="Davinci",
Temperature=0.5, Top-p=1, Frequency Penalty = 0.2,
Stop Sequence = ["\n\n"]
```

```
#### Takes in the training prompt and returns the completed response
async def summarization_example():
    # Takes in a text file(summarize_for_a_2nd_grader) as the input prompt
    prompt_summarize = read_prompt('summarize_for_a_2nd_grader')
    # Calling the completion method along with the specific GPT-3 parameters
    completion_summarize = await
    cleaned_completion(prompt_summarize, max_tokens=100,
    engine="davinci", temperature=0.5, top_p=1, frequency_penalty=0.2,
    stop=["\n\n"])
    # Return the completion response
    return completion_summarize
```

注意
我们已将上述函数设为 async，这使我们能够进行并行函数调用。

一旦函数创建，我们可以简单地创建一个 async 工作流，使用库提供的 ‘main’ 函数调用该工作流并在控制台打印输出：

```
#### Designing the end-to-end async workflow, capable of running
multiple prompts in parallel
async def workflow():
    # Making async call to the summarization function
    text_summ_example = await summarization_example()
    # Printing the result in console
    print('--------------------------')
    print('Basic Example Response: {0}'.format(text_summ_example))
    print('--------------------------')
#### invoke Chronology by using the main function to run the async
workflow
main(workflow)
```

我们可以将其保存为名为 ‘text_summarization.py’ 的 Python 脚本并从终端运行以生成输出。你可以从根文件夹运行以下命令：

```
(base) PS D:\GPT-3 Python> python text_summarization.py
```

一旦我们执行了脚本，我们的控制台打印了提示的以下摘要：

```
--------------------------
Basic Example Response: Olive oil is a liquid fat that comes from olives. Olives grow on a tree called an olive tree. The olive tree is the most common tree in the Mediterranean. People use the oil to cook with, to put on their salads, and as a fuel for lamps.
--------------------------
```

有关如何使用 Python 编程与 GPT-3 交互的更多示例，请访问以下 github 仓库。如果你不熟悉 Python 并希望在不编写代码的情况下串联不同的提示，你可以使用构建在 chronology 库之上的无代码界面。UI 允许你使用拖放功能创建提示工作流，并在设计完成后导出 Python 代码。这是应用程序的链接：(https://chronology-ui.vercel.app/#) 以及维护它的 git 仓库：(https://github.com/OthersideAI/chronology-ui)。





### 使用 Go 调用 OpenAI API

开发者经常声称，就语法而言，Google 的 Go 语言是二十一世纪的 C 语言。

Go 是一种开源编程语言，使构建简单、可靠且高效的应用程序变得容易。创建 Go 的核心思想是将解释型、动态类型语言的编程简易性与静态类型、编译语言的效率和安全性相结合。简单来说，Go 结合了其他编程语言的最佳特性。

Go 是构建需要高安全性、高速度和高模块化项目的首选语言。这使其成为金融科技行业众多项目的有吸引力的选择。Go 的主要特性如下：

- 易用性与最先进的生产力相结合
- 高效性与静态类型并存
- 网络的高级性能以及对多核能力的充分利用。

如果你完全是 Go 语言的新手并想尝试一下，你可以按照以下文档开始入门：https://golang.org/doc/install

在完成安装并了解 Go 编程的基础知识后，你可以按照以下步骤使用 Go 使用 GPT-3。我们将使用以下框架使用 Go 编程创建 GPT-3 驱动的应用程序。

让我们开始创建一个我们可以用来跟踪和导入代码依赖项的 go 模块。为了了解如何创建 go 模块，我们建议你查看本教程 - https://golang.org/doc/tutorial/create-module。对于我们的用例，让我们使用以下命令创建并初始化“gogpt”模块：

```
D:\GPT-3 Go> go mod init gogpt
```

创建“gogpt”模块后，让我们将其指向以下 github 仓库 - github.com/sashabaranov/go-gpt3 以下载用于与 API 工作的必要依赖项和包。为此你可以使用以下命令：

```
D:\GPT-3 Go> go get github.com/sashabaranov/go-gpt3
go get: added github.com/sashabaranov/go-gpt3 v0.0.0-20210606183212-2be4a268a894
```

完成这些步骤后，我们现在将查看相同的文本摘要示例（如下文 3.1 节所述），其中我们将看到如何使用 Go 语言调用 API、将训练提示作为请求发送，并将汇总的完成内容作为输出获取。你可以在以下仓库找到所有代码。

在本示例中，我们将展示如何引导 GPT-3 将给定的文本文档汇总到 2 年级学生也能轻松理解的水平。我们将使用与 3.1 节中提到的相同训练提示，使用 Python 的 OpenAI API。

让我们首先导入必要的依赖项和包：

```
#### Calling the package main
package main
#### Importing Dependencies
import (
    "fmt"
    "io/ioutil"
    "context"
    gogpt "github.com/sashabaranov/go-gpt3"
)
```

Go 编程将源文件组织到称为包的系统目录中，这有助于在 Go 应用程序之间实现代码重用。在代码的第一行我们调用包“main”，并告诉 Go 编译器该包应编译为可执行程序而不是共享库。

### NOTE

在 Go 中，每当你构建可重用的代码片段时，你将开发一个作为共享库的包。但当你开发可执行程序时，你将使用包“main”使该包成为可执行程序。通过调用此包，包“main”中的 main 函数将被设置为我们可执行程序的入口点。

导入依赖项和包后，让我们创建一个 main 函数来托管读取训练提示并为我们提供完成输出的整个逻辑。我们将使用以下 API 参数配置：

```
Maximum tokens=100, Execution Engine="davinci",
Temperature=0.5, Top-p=1, Frequency Penalty = 0.2,
Stop Sequence = ["\n\n"]
```

```
func main() {
    c := gogpt.NewClient("OPENAI-API-KEY")
    ctx := context.Background()
    prompt, err :=
    ioutil.ReadFile("prompts/summarize_for_a_2nd_grader.txt")
    req := gogpt.CompletionRequest{
        MaxTokens: 100,
        Temperature: 0.5,
        TopP: 1.0,
        Stop: []string{"\n\n"},
        FrequencyPenalty: 0.2,
        Prompt: string(prompt),
    }
    resp, err := c.CreateCompletion(ctx, "davinci", req)
    if err != nil {
        return
    }
    fmt.Println("----------------------")
    fmt.Println(resp.Choices[0].Text)
    fmt.Println("----------------------")
}
```

在代码中我们按步骤执行以下任务：

- 1. 我们首先通过提供 API 令牌来设置新的 API 客户端，然后将其置于后台运行。
- 2. 在下一步中，我们从 prompts 文件夹以文本文件的形式读取以下提示“”。
- 3. 我们然后通过提供训练提示并指定 API 参数值（如 temperature、top-p、stop sequence 等）来创建完成请求。
- 4. 创建完成请求后，我们通过提供 API 客户端、完成请求和执行引擎来调用 create completion 函数。
- 5. 该函数将生成完成形式的响应，我们在最后将其打印到控制台。

我们然后可以将代码文件保存为‘text_summarization.go’并从终端运行它以生成输出。你可以使用以下命令从根文件夹运行该文件：

```
(base) PS D:\GPT-3 Go> go run text_summarization.go
```

一旦我们执行了该文件，我们的控制台打印了以下输出：

```
-------------------
Olive oil is a liquid fat that comes from olives. Olives grow on a tree called an olive tree. The olive tree is the most common tree in the Mediterranean. People use the oil to cook with, to put on their salads, and as a fuel for lamps.
-------------------
```

有关如何使用 Go 编程与 GPT-3 交互的更多示例，请访问以下 github 仓库。

### 使用 Java 调用 OpenAI API

Java 是开发传统软件系统最古老、最流行的编程语言之一。全球三分之二的软件行业仍将 Java 作为开发新软件解决方案的核心编程语言。Java 既是一种编程语言，也是一个平台，它附带 Java 运行时环境，帮助程序员高效地开发和运行应用程序。

Java 由 Sun Microsystems（现为 Oracle 的子公司）于 1995 年开发，截至今天，超过 30 亿台设备运行在 Java 上。它是一种通用、基于类的、面向对象的编程语言，旨在减少实现依赖性。它遵循其前身 C、C++ 使用的类似语法，这使得开发人员很容易切换到 Java。

我们现在将查看相同的文本摘要示例（如下文 3.1 节所述），其中我们将看到如何使用 Java 编程语言调用 API、将训练提示作为请求发送，并将汇总的完成内容作为输出获取。

在本示例中，我们将展示如何引导 GPT-3 将给定的文本文档汇总到 2 年级学生也能轻松理解的水平。我们将使用与 3.1 节中提到的相同训练提示，使用 Python 的 OpenAI API。

要进行逐步的代码演练，你可以克隆以下 github 仓库并进入 Programming_with_GPT-3 文件夹并探索 GPT-3_Java 部分。首先，我们导入所有相关依赖项：

```
package example;
// Importing Dependencies
import java.util.*;
import java.io.*;
import com.theokanning.openai.OpenAiService;
import com.theokanning.openai.completion.CompletionRequest;
import com.theokanning.openai.engine.Engine;
```

导入依赖项后，我们创建一个名为“OpenAiApiExample”的类，我们的所有代码都将是该类的一部分。在该类下首先我们使用 API 令牌创建 OpenAiService 的对象：

```
class OpenAiApiExample {
    public static void main(String... args) throws FileNotFoundException {

        String token = "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";
        OpenAiService service = new OpenAiService(token);
```





The connection to OpenAI API is now established in the form of a “service” object. We will now go ahead and read the training prompt from the prompts folder:

```
// Reading the training prompt from the prompts folder
File file = new File("D:\\GPT-3 Book\\Programming with GPT-3\\GPT-3 Java\\example\\src\\main\\java\\example\\prompts\\summarize_for_a_2nd_grader.txt");

Scanner sc = new Scanner(file);

// we just need to use \\Z as delimiter
sc.useDelimiter("\\Z");
// pp is the string consisting of the training prompt
String pp = sc.next();
```

Once you have the training prompt in place, you can create a completion request with the following configuration for the API parameters:

```
Maximum tokens=100, Execution Engine="Davinci",
Temperature=0.5, Top-p=1, Frequency Penalty = 0.2,
Stop Sequence = ["\n\n"]

// Creating a list of strings to used as stop sequence
List<String> li = new ArrayList<String>();
li.add("\n\n''");
// Creating a completion request with the API parameters
CompletionRequest completionRequest =
CompletionRequest.builder().prompt(pp).maxTokens(100).temperature(0.5).topP(1.0).frequencyPenalty(0.2).stop(li).echo(true).build();
// Using the service object to fetch the completion response
service.createCompletion("davinci",completionRequest).getChoices().forEach(System.out::println);
```

We can then save the code file as ‘text_summarization.java’ and run it from the terminal to generate the output. You can use the following command to run the file from your root folder:

```
(base) PS D:\GPT-3 Java> ./gradlew example:run
```

Once we executed the file, our console printed the following output:

```
--------------------
Olive oil is a liquid fat that comes from olives. Olives grow on a tree called an olive tree. The olive tree is the most common tree in the Mediterranean. People use the oil to cook with, to put on their
salads, and as a fuel for lamps.
---------------------
```

For more examples of how you can use Java programming to interact with GPT-3, visit the following github repository.

### The GPT-3 Sandbox

By now, you should be familiar with the basic components required to build a GPT-3 powered application. In this section we will walk you through the GPT-3 Sandbox, an open-source tool that will help you to turn your ideas into reality with just a few lines of python code.

The goal of our sandbox is to empower the readers of this book to create cool web applications agnostic of their technical backgrounds. The sandbox is designed to be low-code to make it user-friendly for people with little to no technical background.

It is built on top of the Streamlit framework enabling users to quickly create & deploy GPT-3 based web applications and showcase their ideas to a wider audience.

As part of the book resources, we have also created a video series with a step-by-step walkthrough on creating and deploying your GPT-3 application. Please follow the video series along with this tutorial to better understand the sandbox ecosystem.

In this tutorial we will show you how to use the GPT-3 sandbox and customize it for your specific application. For this tutorial we will be using VSCode as the IDE, but feel free to use any IDE of your choice.

Prerequisites

- 1. Install VSCode / Any other IDE
- 2. Python 3.7 and above

To use the GPT-3 sandbox, clone the code from the following github repository to your favorite development environment (we will use vscode for this example). You can do it by simply opening a new terminal in your IDE, and using the following command:

```
git clone https://github.com/Shubhamsaboo/kairos_gpt3
```

After cloning the repository just make sure that you are using Python version 3.7+ installed by running the following command:

```
python --version
```

The code structure in your IDE should now look as follows:

![](img/495f94a597dee9cc5b6b1e9654aa4339_84_0.png)

Figure 3-1. Sandbox File Directory Structure

Everything you need to create and deploy a web application is already present in the code. You just need to tweak a few files to customize the functioning of the sandbox for your specific use case.

- 1. After cloning the git repository, the next step is to create a python virtual environment by following the instructions mentioned in this document.

After creating & activating the virtual environment, now its time to install the required dependencies.

At this point we have created and activated a python virtual environment named “env”. Now from here you go to the email_generation folder. To make sure you are in the right space, your path should look like this:

```
(env) kairos_gpt3\GPT-3 Sandbox\email_generation>
```

And from there run the following command:

```
(env) kairos_gpt3\GPT-3 Sandbox\email_generation> pip install
-r requirements.txt
```

- 2. After installing the dependencies you can start customizing the sandbox code. The first file that you need to look at is training_data.py.

In that file you can replace the default prompt with the training prompt specific to your use-case. To play and experiment with different training prompts for your application, you can use the GPT-3 playground as we discussed in chapter 2. Refer to the following video for a visual walkthrough of customizing the sandbox.

- 3. Once you are done with replacing the training prompt based on your use-case you can then tweak the API parameters (Maximum tokens, Execution Engine, Temperature, Top-p, Frequency Penalty, Stop Sequence) as per your requirements in the training_service.py file.

- 4. That’s it! Your GPT-3 based web application is now ready and can be run locally using the following command

```
(env) kairos_gpt3\GPT-3 Sandbox\email_generation> streamlit run gpt_app.py
```

5. Now after checking the application sanity locally, its time for you to deploy the application to the internet using Streamlit sharing (refer to the following video for the deployment walkthrough) to showcase it to a wider audience.

## NOTE

This application follows a simple workflow, where the training prompt receives single input from the UI and comes up with the response. If your application requires complex workflow, where the training prompt takes in multiple inputs to come up with the output. You can go ahead and customize the UI elements by going through the following scripts (app1.py, app2.py, gpt_app.py).

The front-end interface is customizable based on your usecase and creating complex workflows totally depends on your comfort with Python and Streamlit. To understand in detail how you can customize a streamlit application, please refer to the following docs.

Following are the snapshots from the example application that is hosted here using streamlit sharing;

#### Figure 3-2. Dashboard

#### Figure 3-3. GPT-3 Sandbox

In the next chapters, we will explore different areas involving applications of GPT-3 and leverage this sandbox to create easily deployable web applications that can be shared with the GPT-3 community and the broader audience.

### Going Live with GPT-3-Powered Applications

By now you have successfully created your gpt-3 powered application and it’s time to put that into production. Just like every coin has two sides, there are a number of harmful ways in which GPT-3 can be used as discussed in section 1.5.1 (Risks associated with GPT-3). In order to safeguard the use of API against those malicious practices, OpenAI created a certain set of guidelines and procedures that you have to follow before going live with your application.

Currently, OpenAI allows you to serve the API out to five people without taking pre-approval, but as you grow to more than five people you need to apply for the production review. It is highly recommended to go through the Use Case Guidelines before even applying for the pre-launch review.

When you are ready to “go live” with your application, you can submit the Pre-Launch Review Request which will prompt the OpenAI team to look into your use case in detail and raise any potential red flags which may violate the API usage Safety Guidelines set up by OpenAI. When you apply for the pre-launch review, the openAI team examines how you have integrated the API into your application and assesses possible risks and the possibility of misuse.

After submitting the pre-launch review proposal, your request is either approved or denied, with appropriate feedback highlighting the potential red flags. If your request is successful, OpenAI will grant you a maximum spend limit, and increase that limit over time as you build a track record with your application. This gives you the freedom to build and deploy your application, while they monitor the potential impact of your application on the platform. As your application grows and builds up the user base, you can submit a Quota Increase Request for OpenAI to accommodate your application needs.




