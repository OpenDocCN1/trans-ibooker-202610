# 11
## TESTING YOUR CODE

![](img/8f15563c16d2a98c4d3115150679b092_352_0.png)

当你编写函数或类时，你也可以为该代码编写测试。测试证明你的代码在响应其设计接收的所有类型输入时按预期工作。当你编写测试时，你可以确信随着越来越多的人开始使用你的程序，你的代码将能正确工作。你还能够在添加新代码时进行测试，以确保更改不会破坏程序的现有行为。每个程序员都会犯错，因此每个程序员都必须经常测试他们的代码，以在用户遇到问题之前捕获问题。

在本章中，你将学习使用 `pytest` 测试你的代码。`pytest` 库是一组工具，可帮助你快速简单地编写第一个测试，同时随着项目复杂度的增长支持你的测试。Python 默认不包含 `pytest`，因此你将学习如何安装外部库。知道如何安装外部库将使你能够使用大量设计良好的代码。这些库将极大地扩展你能够工作的项目类型。

你将学习构建一系列测试并检查每组输入是否产生你想要的输出。你将看到通过测试的样子和失败测试的样子，并学习失败的测试如何帮助你改进代码。你将学习如何测试函数和类，并开始了解为项目编写多少测试。

## Installing pytest with pip

虽然 Python 在标准库中包含大量功能，但 Python 开发人员也高度依赖第三方包。*第三方包* 是在核心 Python 语言之外开发的库。一些流行的第三方库最终被采纳到标准库中，并从那时起被包含在大多数 Python 安装中。这通常发生在一旦初始错误被解决后不太可能发生很大变化的库上。这类库可以与整体语言同步演进。

然而，许多包被排除在标准库之外，以便它们可以独立于语言本身按自己的时间表开发。这些包往往比如果它们与 Python 的开发计划绑定时更新得更频繁。这对 `pytest` 和我们在本书后半部分使用的许多库都是如此。你不应该盲目信任每个第三方包，但也不应因大量重要功能是通过此类包实现的而感到却步。

## Updating pip

Python 包含一个名为 pip 的工具，用于安装第三方包。由于 pip 有助于从外部资源安装包，因此它经常更新以解决潜在的安全问题。因此，我们将首先更新 pip。

打开一个新的终端窗口并发出以下命令：

```
$ python -m pip install --upgrade pip
① Requirement already satisfied: pip in /.../python3.11/site-packages (22.0.4)
--snip--
② Successfully installed pip-22.1.2
```

此命令的第一部分 python -m pip 告诉 Python 运行模块 pip。第二部分 install --upgrade 告诉 pip 更新已安装的包。最后一部分 pip 指定应更新的第三方包。输出显示我当前的 pip 版本 22.0.4 ① 已被本次写作时的最新版本 22.1.2 ② 替换。

你可以使用此命令更新系统上安装的任何第三方包：

```
$ python -m pip install --upgrade package_name
```

> **NOTE**
> 
> *如果您使用的是 Linux，pip 可能未包含在您的 Python 安装中。如果尝试升级 pip 时出现错误，请参阅附录 A 中的说明。*

### Installing pytest

现在 pip 已更新，我们可以安装 pytest：

```
```bash
$ python -m pip install --user pytest
Collecting pytest
  --snip--
Successfully installed attrs-21.4.0 iniconfig-1.1.1 ...pytest-7.x.x
```
```

我们仍然使用核心命令 `pip install`，这次没有 `--upgrade` 标志。相反，我们使用 `--user` 标志，这告诉 Python 仅为当前用户安装此包。输出显示 pytest 的最新版本已成功安装，以及 pytest 所依赖的许多其他包。

你可以使用此命令安装许多第三方包：

```
```bash
$ python -m pip install --user package_name
```
```

> **NOTE**
> 
> *如果运行此命令时遇到任何困难，请尝试在不使用 `--user` 标志的情况下运行相同的命令。*

## Testing a Function




为代码的关键行为编写测试，并且只有当项目开始被广泛使用时，才去追求完全的测试覆盖率。

## 通过测试

使用 pytest 编写你的第一个单元测试是相当简单的。我们将编写一个测试函数。测试函数会调用我们正在测试的函数，并对返回值做出断言。如果断言正确，测试就会通过；如果断言不正确，测试就会失败。

下面是函数 `get_formatted_name()` 的第一个测试：

```python
# test_name_function.py
from name_function import get_formatted_name

def test_first_last_name():
    """Do names like 'Janis Joplin' work?"""
    formatted_name = get_formatted_name('janis', 'joplin')
    assert formatted_name == 'Janis Joplin'
```

在运行测试之前，让我们仔细看看这个函数。测试文件的名称很重要；它必须以 `test_` 开头。当我们要求 pytest 运行我们编写的测试时，它会查找任何以 `test_` 开头的文件，并运行该文件中找到的所有测试。

在测试文件中，我们首先导入要测试的函数：`get_formatted_name()`。然后我们定义一个测试函数：在这里是 `test_first_last_name()` ❶。这个函数名比我们平时使用的要长，这是有充分理由的。首先，测试函数需要以单词 *test* 开头，后跟下划线。任何以 `test_` 开头的函数都会被 pytest *发现*，并作为测试过程的一部分运行。

此外，测试名称应该比典型的函数名更长、更具描述性。你永远不会自己调用这个函数；pytest 会找到它并为你运行。测试函数名应该足够长，这样如果你在测试报告中看到函数名，就能很好地了解正在测试的是哪种行为。

接下来，我们调用正在测试的函数 ❷。这里我们用参数 `'janis'` 和 `'joplin'` 调用 `get_formatted_name()`，就像我们运行 `names.py` 时使用的那样。我们将该函数的返回值赋给 `formatted_name`。

最后，我们做出一个断言 ❸。*断言*是对某个条件的声明。这里我们声明 `formatted_name` 的值应为 `'Janis Joplin'`。

### 运行测试

如果你直接运行 *test_name_function.py* 文件，你不会得到任何输出，因为我们从未调用测试函数。相反，我们将让 pytest 为我们运行测试文件。

为此，打开一个终端窗口，并导航到包含测试文件的文件夹。如果你使用 VS Code，可以打开包含测试文件的文件夹，并使用编辑器窗口中嵌入的终端。在终端窗口中，输入命令 **pytest**。你应该会看到：

```console
$ pytest
============================= test session starts ==============================
platform darwin -- Python 3.x.x, pytest-7.x.x, pluggy-1.x.x
rootdir: /.../python_work/chapter_11
collected 1 item

test_name_function.py .                                                  [100%]
```

```console
========================= 1 passed in 0.00s =========================
==========
```

让我们尝试理解这个输出。首先，我们会看到一些关于运行测试的系统信息 ❶。我是在 macOS 系统上测试的，所以你可能在这里看到一些不同的输出。最重要的是，我们可以看出运行测试时使用的是哪个版本的 Python、pytest 和其他包。

接下来，我们看到测试运行的目录 ❷：在我的例子中是 _python_work/chapter_11_。我们可以看到 pytest 发现了一个要运行的测试 ❸，并且可以看到正在运行的测试文件 ❹。文件名后面的单个点告诉我们一个测试通过了，100% 则清楚地表明所有测试都已运行。一个大型项目可能有成百上千个测试，这些点和完成百分比指示器有助于监控测试运行的总体进度。

最后一行告诉我们一个测试通过了，运行该测试所花时间不到 0.01 秒。

这个输出表明，除非我们修改函数，否则 `get_formatted_name()` 对于只包含名和姓的名字将始终有效。当我们修改 `get_formatted_name()` 后，可以再次运行此测试。如果测试通过，我们就知道该函数对于像 Janis Joplin 这样的名字仍然有效。

> **注意**
>
> *如果你不确定如何在终端中导航到正确的位置，请参阅第 11 页的“从终端运行 Python 程序”。另外，如果你看到一条消息说找不到 pytest 命令，请改用命令 `python -m pytest`。*

## 失败测试

失败的测试是什么样的？让我们修改 `get_formatted_name()`，使其能够处理中间名，但这种方式会破坏该函数对只有名和姓的名字（如 Janis Joplin）的功能。

下面是 `get_formatted_name()` 的一个新版本，它需要一个中间名参数：

**name_function.py**

```python
def get_formatted_name(first, middle, last):
    """Generate a neatly formatted full name."""
    full_name = f"{first} {middle} {last}"
    return full_name.title()
```

这个版本应该适用于有中间名的人，但当我们测试它时，会发现我们已经破坏了它对只有名和姓的人的功能。

这一次，运行 `pytest` 会得到以下输出：

```console
$ pytest
============================= test session starts =============================
===========================
--snip--
❶ test_name_function.py F
   [100%]
❷ ============================== FAILURES ==============================
===========================
❸ ________________________ test_first_last_name _________________________

    def test_first_last_name():
        """Do names like 'Janis Joplin' work?"""
❹ >       formatted_name = get_formatted_name('janis', 'jopli
n')
❺ E       TypeError: get_formatted_name() missing 1 required p
ositional
          argument: 'last'

test_name_function.py:5: TypeError
```

```console
======================== short test summary info =========================
FAILED test_name_function.py::test_first_last_name - TypeError:
    get_formatted_name() missing 1 required positional argument: 'last'
==================== 1 failed in 0.04s ==========================
```

这里有很多信息，因为测试失败时你可能需要了解很多内容。输出中第一件值得注意的事是一个单独的 F ①，它告诉我们一个测试失败了。然后我们看到一个聚焦于 FAILURES ② 的部分，因为失败的测试通常是测试运行中最需要关注的内容。接下来，我们看到 `test_first_last_name()` 是失败的测试函数 ③。一个尖括号 ④ 指向导致测试失败的代码行。下一行的 E ⑤ 显示了导致失败的实际错误：由于缺少必需的固定参数 `last` 而导致的 TypeError。最重要的信息在最后以更短的摘要形式重复出现，所以当你运行许多测试时，可以快速了解哪些测试失败以及失败原因。

## 应对失败测试

当测试失败时你该怎么办？假设你检查了正确的条件，那么测试通过意味着函数行为正确，测试失败则意味着你编写的新代码存在错误。因此，当测试失败时，不要修改测试。如果你修改了测试，你的测试可能会通过，但任何像测试那样调用你函数的代码都会突然停止工作。相反，应该修复导致测试失败的代码。检查你刚刚对函数所做的更改，并弄清楚这些更改是如何破坏期望行为的。

在这种情况下，`get_formatted_name()` 以前只需要两个参数：名和姓。现在它需要名、中间名和姓。添加这个必需的中间名参数破坏了 `get_formatted_name()` 的原始行为。这里最好的选择是让中间名成为可选项。一旦我们这样做，我们针对 Janis Joplin 这样的名字的测试应该会再次通过，而且我们也应该能够接受中间名。让我们修改 `get_formatted_name()`，使中间名可选，然后再次运行测试用例。如果测试通过，我们将继续确保该函数能正确处理中间名。

为了使中间名可选，我们将参数 `middle` 移到函数定义的参数列表末尾，并给它一个空默认值。我们还添加了一个 `if` 测试，根据是否提供了中间名来正确构建全名：

***name_function.py***

```python
def get_formatted_name(first, last, middle=''):
    """Generate a neatly formatted full name."""
    if middle:
        full_name = f"{first} {middle} {last}"
    else:
        full_name = f"{first} {last}"
    return full_name.title()
```

在这个新版本的 `get_formatted_name()` 中，中间名是可选的。如果向函数传递了中间名，全名将包含名、中间名和姓。否则，全名将只包含名和姓。现在，该函数应该对这两种名字都有效。为了找出该函数是否仍然对 Janis Joplin 这样的名字有效，让我们再次运行测试：

```console
$ pytest
==================== test session starts ====================
==========
```





```
```
--snip--
test_name_function.py .
[100%]
============================= 1 passed in 0.00s ==============================
------------------------------
```
```

测试现在通过了。这很理想；这意味着该函数再次适用于像 Janis Joplin 这样的名字，而无需我们手动测试该函数。修复我们的函数更容易，因为失败的测试帮助我们确定新代码如何破坏了现有行为。

### 添加新测试

现在我们知道 `get_formatted_name()` 再次适用于简单的名字，让我们为包含中间名的人编写第二个测试。我们通过在文件 `test_name_function.py` 中添加另一个测试函数来实现：

**test_name_function.py**

```
```
from name_function import get_formatted_name

def test_first_last_name():
    --snip--

def test_first_last_middle_name():
    """Do names like 'Wolfgang Amadeus Mozart' work?"""
    formatted_name = get_formatted_name(
        'wolfgang', 'mozart', 'amadeus')
    assert formatted_name == 'Wolfgang Amadeus Mozart'
```
```

我们将这个新函数命名为 `test_first_last_middle_name()`。函数名必须以 `test_` 开头，这样当我们运行 **pytest** 时函数会自动运行。我们命名该函数以明确我们正在测试 `get_formatted_name()` 的哪种行为。因此，如果测试失败，我们会立即知道哪些类型的名字受影响。

# TRY IT YOURSELF

**11-1. City, Country:** 编写一个接受两个参数的函数：城市名和国家名。函数应返回一个形如 *City, Country* 的单个字符串，例如 `Santiago, Chile`。将该函数存储在名为 `city_functions.py` 的模块中，并将此文件保存在一个新文件夹中，以便 `pytest` 不会尝试运行我们已编写的测试。

创建一个名为 `test_cities.py` 的文件来测试你刚刚编写的函数。编写一个名为 `test_city_country()` 的函数，以验证使用诸如 'santiago' 和 'chile' 这样的值调用你的函数是否会产生正确的字符串。运行测试，并确保 `test_city_country()` 通过。

**11-2. Population:** 修改你的函数，使其需要第三个参数 `population`。它现在应返回一个形如 *City, Country – population xxx* 的单个字符串，例如 `Santiago, Chile – population 5000000`。再次运行测试，并确保这次 `test_city_country()` 失败。

修改函数，使 `population` 参数为可选。运行测试，并确保 `test_city_country()` 再次通过。

编写第二个测试 `test_city_country_population()`，验证你可以使用值 'santiago'、'chile' 和 'population=5000000' 调用你的函数。再次运行测试，并确保这个新测试通过。

# Testing a Class

在本章的第一部分，你为单个函数编写了测试。现在你将为类编写测试。你将在自己的许多程序中使用类，因此能够证明你的类工作正确非常有帮助。如果你在处理的类上有通过的测试，你就可以确信对该类所做的改进不会意外破坏其当前行为。

## A Variety of Assertions

到目前为止，你只看到一种断言：断言一个字符串具有特定值。在编写测试时，你可以提出任何可以用条件语句表达的主张。如果条件按预期为 True，你对程序该部分行为的假设将被证实；你可以确信不存在错误。如果你假设为 True 的条件实际上为 False，测试将失败，你就会知道存在需要解决的问题。表 11-1 显示了可在初始测试中包含的最有用的断言类型。

**Table 11-1: _Commonly Used Assertion Statements in Tests_**

| Assertion | Claim |
|---|---|
| `assert a == b` | Assert that two values are equal. |
| `assert a != b` | Assert that two values are not equal. |
| `assert a` | Assert that a evaluates to True. |
| `assert not a` | Assert that a evaluates to False. |
| `assert element in list` | Assert that an element is in a list. |
| `assert element not in list` | Assert that an element is not in a list. |

这些只是几个例子；任何可以用条件语句表达的内容都可包含在测试中。

## A Class to Test

测试类与测试函数类似，因为其中很大一部分工作涉及测试类中方法的行为。然而有一些区别，所以让我们编写一个要测试的类。考虑一个帮助管理匿名调查的类：

**survey.py**

```python
class AnonymousSurvey:
    """Collect anonymous answers to a survey question."""

    def __init__(self, question):
        """Store a question, and prepare to store responses."""
        self.question = question
```

```python
self.responses = []

def show_question(self):
    """Show the survey question."""
    print(self.question)

def store_response(self, new_response):
    """Store a single response to the survey."""
    self.responses.append(new_response)

def show_results(self):
    """Show all the responses that have been given."""
    print("Survey results:")
    for response in self.responses:
        print(f"- {response}")
```
```

该类以你提供的调查问题开始 ❶ 并包含一个用于存储响应的空列表。该类具有打印调查问题的 ❷ 方法、将新响应添加到响应列表的 ❸ 方法，以及打印列表中存储的所有响应的 ❹ 方法。要从此类创建实例，你只需提供一个问题。一旦你拥有代表特定调查的实例，你就可以使用 `show_question()` 显示调查问题，使用 `store_response()` 存储响应，并使用 `show_results()` 显示结果。

为了证明 `AnonymousSurvey` 类有效，让我们编写一个使用该类的程序：

# **language_survey.py**

```python
from survey import AnonymousSurvey

# Define a question, and make a survey.
question = "What language did you first learn to speak?"
language_survey = AnonymousSurvey(question)

# Show the question, and store responses to the question.
language_survey.show_question()
print("Enter 'q' at any time to quit.\n")
while True:
    response = input("Language: ")
```

```
if response == 'q':
    break
language_survey.store_response(response)

# Show the survey results.
print("\nThank you to everyone who participated in the survey!")
language_survey.show_results()
```

该程序定义了一个问题（"What language did you first learn to speak?"）并使用该问题创建了一个 `AnonymousSurvey` 对象。程序调用 `show_question()` 来显示问题，然后提示输入响应。每个响应在接收时被存储。当所有响应都已输入（用户输入 q 退出）时，`show_results()` 打印调查结果：

```
What language did you first learn to speak?
Enter 'q' at any time to quit.

Language: English
Language: Spanish
Language: English
Language: Mandarin
Language: q

Thank you to everyone who participated in the survey!
Survey results:
- English
- Spanish
- English
- Mandarin
```

该类适用于简单的匿名调查，但假设我们想要改进 `AnonymousSurvey` 及其所在的模块 `survey`。我们可以允许每个用户输入多个响应，我们可以编写一个方法仅列出唯一响应并报告每个响应出现的次数，或者我们甚至可以编写另一个类来管理非匿名调查。

实现此类更改可能会影响类 `AnonymousSurvey` 的当前行为。例如，在尝试允许每个用户输入多个响应时，我们可能会意外地改变单次响应的处理方式。为了确保在开发此模块时不会破坏现有行为，我们可以为该类编写测试。

# Testing the AnonymousSurvey Class

让我们编写一个测试来验证 `AnonymousSurvey` 行为的一个方面。我们将编写一个测试来验证对调查问题的单个响应是否被正确存储：

test_survey.py

```
from survey import AnonymousSurvey

def test_store_single_response():
    """Test that a single response is stored properly."""
    question = "What language did you first learn to speak?"
    language_survey = AnonymousSurvey(question)
    language_survey.store_response('English')
    assert 'English' in language_survey.responses
```

我们首先导入要测试的类 `AnonymousSurvey`。第一个测试函数将验证当我们存储对调查问题的响应时，该响应会出现在调查的响应列表中。给此函数一个好的描述性名称是 `test_store_single_response()` ①。如果此测试失败，我们将从测试摘要中的函数名得知在存储对调查的单个响应时存在问题。

要测试类的行为，我们需要创建类的一个实例。我们创建一个名为 `language_survey` 的实例 ②，问题为 "What language did you first learn to speak?" 我们使用 `store_response()` 方法存储单个响应 English。然后我们验证该


response was stored correctly by asserting that English is in the list `language_survey.responses` ③.

默认情况下，不带参数运行 **pytest** 命令将运行 `pytest` 在当前目录中发现的所有测试。若要只关注一个文件中的测试，请传入要运行的测试文件的名称。这里我们只运行我们为 `AnonymousSurvey` 编写的一个测试：

```
$ pytest test_survey.py
======================== test session starts =========================
--snip--
test_survey.py .
[100%]
========================= 1 passed in 0.01s ===========================
```

这是一个不错的开始，但调查只有在能生成多个响应时才有用。我们来验证三个响应能否被正确存储。为此，我们向 `TestAnonymousSurvey` 添加另一个方法：

```
from survey import AnonymousSurvey

def test_store_single_response():
    --snip--

def test_store_three_responses():
    """Test that three individual responses are stored properly."""
    question = "What language did you first learn to speak?"
    language_survey = AnonymousSurvey(question)
    responses = ['English', 'Spanish', 'Mandarin']
    for response in responses:
        language_survey.store_response(response)

    for response in responses:
        assert response in language_survey.responses
```

我们调用新函数 `test_store_three_responses()`。我们像在 `test_store_single_response()` 中那样创建一个调查对象。我们定义一个包含三个不同响应的列表 ❶，然后对每个响应调用 `store_response()`。一旦响应被存储，我们编写另一个循环，断言每个响应现在都在 `language_survey.responses` 中 ❷。

当我们再次运行测试文件时，两个测试（针对单个响应和三个响应）都通过了：

```
```text
$ pytest test_survey.py
========================= test session starts =========================
=========
--snip--
test_survey.py ..
[100%]
========================= 2 passed in 0.01s =========================
=========
```
```

这完美地工作。然而，这些测试有些重复，因此我们将使用 pytest 的另一个特性来让它们更高效。

## 使用 Fixtures

在 *test_survey.py* 中，我们在每个测试函数中都创建了一个新的 `AnonymousSurvey` 实例。在我们使用的这个简短示例中这没问题，但在一个包含数十或数百个测试的真实项目中，这会成为问题。

在测试中，*fixture* 有助于搭建测试环境。通常，这意味着创建一个可供多个测试使用的资源。我们通过编写带有装饰器 `@pytest.fixture` 的函数来在 pytest 中创建 *fixture*。*装饰器* 是放在函数定义之前的一条指令；Python 会在函数运行前将其应用到函数上，以改变函数代码的行为。如果这听起来很复杂，别担心；你可以在学习自己编写装饰器之前，就开始使用第三方包中的装饰器。

让我们使用一个 fixture 来创建单个调查实例，该实例可用于 *test_survey.py* 中的两个测试函数：

```
import pytest
from survey import AnonymousSurvey

@pytest.fixture
def language_survey():
    """A survey that will be available to all test functions."""
    question = "What language did you first learn to speak?"
    language_survey = AnonymousSurvey(question)
    return language_survey

def test_store_single_response(language_survey):
    """Test that a single response is stored properly."""
    language_survey.store_response('English')
    assert 'English' in language_survey.responses

def test_store_three_responses(language_survey):
    """Test that three individual responses are stored properly."""
    responses = ['English', 'Spanish', 'Mandarin']
    for response in responses:
        language_survey.store_response(response)

    for response in responses:
        assert response in language_survey.responses
```

我们现在需要导入 pytest，因为我们在使用 pytest 中定义的装饰器。我们将 `@pytest.fixture` 装饰器 ❶ 应用于新函数 `language_survey()` ❷。这个函数构建一个 AnonymousSurvey 对象并返回新的调查。

注意，两个测试函数的定义都发生了变化 ❸ ❺；每个测试函数现在都有一个名为 `language_survey` 的参数。当测试函数中的参数与带有 `@pytest.fixture` 装饰器的函数名匹配时，该 fixture 会被自动运行，其返回值会被传递给测试函数。在这个示例中，函数 `language_survey()` 为 `test_store_single_response()` 和 `test_store_three_responses()` 两者都提供了 `language_survey` 实例。

两个测试函数中都没有新增代码，但注意每个函数中都删除了两行 ❹ ❻：定义问题的那一行和创建 `AnonymousSurvey` 对象的那一行。

当我们再次运行测试文件时，两个测试仍然通过。在尝试扩展 `AnonymousSurvey` 以处理每个人的多个响应时，这些测试将特别有用。在修改代码以接受多个响应之后，你可以运行这些测试，确保你没有影响到存储单个响应或一系列独立响应的能力。

上面的结构几乎肯定会看起来很复杂；它包含了一些你迄今为止见过的最抽象的代码。你不必马上使用 fixtures；编写包含大量重复代码的测试总比完全不写测试要好。只需知道，当你写了很多测试以至于重复成为障碍时，有一种成熟的方法来处理重复。此外，在这个简单示例中，fixtures 并没有真正让代码变得更短或更容易理解。但在拥有许多测试的项目中，或者在需要很多行代码来构建一个会被多个测试使用的资源的情况下，fixtures 可以极大地改进你的测试代码。

当你想编写一个 fixture 时，编写一个生成多个测试函数所用资源的函数。将 `@pytest.fixture` 装饰器添加到新函数，并将此函数的名称作为参数添加到每个使用此资源的测试函数。从那时起，你的测试将更短，更易于编写和维护。

## 动手试一试

**11-3. Employee：** 编写一个名为 `Employee` 的类。`__init__()` 方法应接受名、姓和年薪，并将这些分别存储为属性。编写一个名为 `give_raise()` 的方法，默认将年薪增加 $5,000，但也接受不同的加薪金额。

为 `Employee` 编写一个测试文件，包含两个测试函数：`test_give_default_raise()` 和 `test_give_custom_raise()`。先不使用 fixture 编写你的测试，并确保它们都通过。然后编写一个 fixture，这样你就不必在每个测试函数中创建新的 Employee 实例。再次运行测试，并确保两个测试仍然通过。

## 小结

在本章中，你学习了使用 pytest 模块中的工具为函数和类编写测试。你学习了编写测试函数来验证函数和类应表现出的特定行为。你了解了 fixtures 如何能够高效地创建可供测试文件中多个测试函数使用的资源。

测试是一个重要的主题，许多较新的程序员并未接触过。作为新程序员，你不必为尝试的所有简单项目编写测试。但一旦你开始从事需要大量开发工作的项目，就应该测试函数和类的关键行为。你会更有信心项目中的新工作不会破坏已正常工作的部分，这将赋予你自由去改进代码。如果你意外破坏了现有功能，你会立刻知道，因此仍然可以轻松修复问题。对一个失败的测试做出响应，你


编写测试远比回复一位不满用户的错误报告要容易得多。

如果你在项目中包含一些初步测试，其他程序员会更加尊重你的项目。他们在试用你的代码时会感到更自在，也更愿意与你在项目上合作。如果你想为其他程序员正在开发的项目做贡献，你需要证明你的代码能通过现有测试，并且通常还需要为你在项目中引入的任何新行为编写测试。

多实践测试来熟悉代码测试的过程。为你的函数和类中最关键的行为编写测试，但在早期项目中不要追求全覆盖，除非你有特定的理由这么做。

数据可视化项目从**第15章**开始，你将学习生成数据，并使用Matplotlib和Plotly创建一系列实用且美观的数据可视化图表。**第16章**教你从在线来源获取数据，并将其输入可视化包，以创建天气数据图表和全球地震活动地图。最后，**第17章**展示如何编写程序来自动下载和可视化数据。学习制作可视化图像能让你探索数据科学领域，这是当今编程领域中需求最高的领域之一。

## Web应用程序

在Web应用程序项目（**第18、19和20章**）中，你将使用Django包创建一个简单的Web应用程序，让用户能够记录他们正在学习的不同主题的日志。用户将使用用户名和密码创建账户，输入主题，然后记录他们正在学习的内容。你还需要将应用部署到远程服务器，以便世界上任何人都能访问它。

完成此项目后，你将能够开始构建自己的简单Web应用程序，并准备好深入学习使用Django构建应用程序的更全面的资源。
