

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

