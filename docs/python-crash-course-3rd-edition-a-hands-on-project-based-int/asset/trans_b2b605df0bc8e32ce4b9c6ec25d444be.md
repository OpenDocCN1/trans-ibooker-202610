
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

