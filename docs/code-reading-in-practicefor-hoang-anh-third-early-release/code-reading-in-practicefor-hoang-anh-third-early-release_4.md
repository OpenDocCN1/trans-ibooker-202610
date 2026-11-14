## 第 3 章 概念考察

在前几章中，你在不深入代码内容的情况下考察了代码中的关系，并孤立地研究变量名而不看结构，给了你两个不同的视角来阅读代码。

本章将把这两种技术结合起来，考察代码中存在的编程概念。概念通常以两种方式体现：要么明确写在名称中（如 `customerArray`），要么从代码结构中确定（如下面的代码片段）：

```
foreach (customer in customers)
{
    customer.free_account();
}
```

你也可以通过查看类型和导入，或查看与概念相关的特定语法元素（如指针的 `*` 和对象的 `.`）来了解编程概念。

深入理解代码片段中使用的编程概念很有用，因为它能帮助你更好地在心理上处理代码，并帮助你理解可能缺少的先验知识。

本章的练习和技术侧重于代码的概念方面，帮助你回答诸如：这段代码库中重要的编程概念是什么？它们如何与领域和代码结构相关联？

### 运行示例




### 从名称和操作中学习

与前几章一样，我提供了一个示例代码片段，帮助你理解本章介绍的概念：在本章中，你将学习如何发掘嵌入在代码中的概念。如果它看起来完全让人不知所措，请不要担心！读完本章后，你将能够理解它……

#### 阅读示例片段

本章后面的章节包含使用示例片段完成的练习，但在继续阅读之前，你可能想先对代码有个大致了解。一个很好的开始方法是使用前几章的练习，来理解代码的结构和领域。

#### 示例代码：Lark 中的树模板

```python
from typing import Union, Optional, Mapping

from lark import Tree, Transformer
from lark.exceptions import MissingVariableError

Branch = Union[Tree[str], str]
TreeOrCode = Union[Tree[str], str]
MatchResult = Dict[str, Tree]
_TEMPLATE_MARKER = '$'


class TemplateConf:
   # 模板配置 允许针对模板的不同用途进行自定义

   def __init__(self, parse=None):
       self._parse = parse

   def test_var(self, var: Union[Tree, str]) -> Optional[str]:
       """给定一个树节点，如果它是一个模板变量，则返回其名称。否则，返回 None。

       此方法可被重写以进行自定义

       参数:
           var: Tree | str - 要测试的树节点
       """
       if isinstance(var, str) and var.startswith('$'):
           return var.lstrip('$')

       if isinstance(var, Tree) and var.data == 'var' and len(var.children) > 0:
           first_child = var.children[0]
           if isinstance(first_child, str) and first_child.startswith('$'):
               return first_child.lstrip('$')

       return None

   def _get_tree(self, template: TreeOrCode):
       if isinstance(template, str):
           assert self._parse
           template = self._parse(template)

       assert isinstance(template, Tree)
       return template

   def __call__(self, template):
       return Template(template, conf=self)

   def _match_tree_template(self, template, tree):
       template_var = self.test_var(template)
       if template_var:
           return {template_var: tree}

       if isinstance(template, str):
           if template == tree:
               return {}
           return None

       assert isinstance(template, Tree), template

       if template.data == tree.data and len(template.children) == len(tree.children):
           res = {}
           for t1, t2 in zip(template.children, tree.children):
               matches = self._match_tree_template(t1, t2)
               if matches is None:
                   return None

               res.update(matches)
           return res


class Template:
   """表示一个树模板，绑定到特定配置

   树模板是一个包含模板变量节点的树。
   这些变量将匹配任何树。
   (未来版本可能支持变量上的注解，以允许更复杂的模板)
   """

   def __init__(self, tree: Tree, conf = TemplateConf()):
       self.conf = conf
       self.tree = conf._get_tree(tree)

   def match(self, tree: TreeOrCode):
       """将树模板与一棵树进行匹配。

       不包含变量的树模板仅当 ``tree`` 等于该模板时才匹配。

       参数:
           tree (Tree): 要与模板匹配的树

       返回:
           Optional[Dict[str, Tree]]: 如果找到匹配项，则返回一个字典，映射
               模板变量名到其匹配的树节点。
               如果未找到匹配项，则返回 None。
       """
       tree = self.conf._get_tree(tree)
       return self.conf._match_tree_template(self.tree, tree)

   def search(self, tree: TreeOrCode):
       """在 ``tree`` 中搜索树模板的所有出现位置。
       """
       tree = self.conf._get_tree(tree)
       for subtree in tree.iter_subtrees():
           res = self.match(subtree)
           if res:
               yield subtree, res

   def apply_vars(self, vars: Mapping[str, Tree]):
       """将变量应用到模板树
       """
       return _ReplaceVars(self.conf, vars).transform(self.tree)


def translate(t1: Template, t2: Template, tree: TreeOrCode):
   """搜索树并将 t1 的每次出现翻译为 t2。
   """
   tree = t1.conf._get_tree(tree)      # 确保它是一棵树，如果必要且可能则进行解析
   for subtree, vars in t1.search(tree):
       res = t2.apply_vars(vars)
       subtree.set(res.data, res.children)
   return tree


class TemplateTranslator:
   """用于翻译模式集合的工具类
   """

   def __init__(self, translations: Mapping[TreeOrCode, TreeOrCode]):
       assert all(isinstance(k, Template) and isinstance(v, Template) for k, v in translations.items())
       self.translations = translations

   def translate(self, tree: Tree):
       for k, v in self.translations.items():
           tree = translate(k, v, tree)
       return tree
```

#### 背景

上面的代码片段来自一个名为 Lark 的 Python 解析器框架。如果你想在浏览器或 IDE 中查看代码，可以在这里找到：`https://github.com/lark-parser/lark/blob/953171821ed307f700fddf27f3fcb9483346bd46/lark/tree_templates.py`。

如果你做过领域练习，你可能已经注意到，这段代码的领域主要是编程领域：解析和模板化。

#### 练习 4.1 检查代码库中所有变量名中的概念

对于这个练习，记下在 Lark 代码片段中找到的变量。根据这些名称，你会联想到哪些编程概念？

| 变量名 | 编程概念 |
| --- | --- |
| `Branch` | 树, 解析 |
| `TreeOrCode` | 树, 解析 |
| `MatchResult` |  |
| `_TEMPLATE_MARKER` |  |
| `TemplateConf` |  |
| `Template` |  |
| `TemplateTranslator` |  |
| `translate` |  |
| `test_var` |  |
| `_get_tree` |  |
| `_match_tree_template` |  |
| `match` |  |
| `search` |  |
| `apply_vars` |  |
| `__call__` |  |
| `transform` |  |
| `iter_subtrees` |  |
| `set` |  |
| `lstrip` |  |
| `startswith` |  |
| `isinstance` |  |
| `assert` |  |
| `zip` |  |
| `update` |  |
| `yield` |  |
| `Mapping` |  |
| `Union` |  |
| `Optional` |  |
| `Dict` |  |
| `Tree` |  |
| `Transformer` |  |
| `MissingVariableError` |  |

当代码库组织良好时，名称可以让你对代码在做什么有可靠的理解。像 `passwordHashed` 这样的名称清楚地表明正在对密码执行哈希操作。不过，通常从名称中识别概念需要对领域有理解。例如，考虑 Lark 代码片段的前几行。让我们看看这段代码片段顶部的变量 `Branch`：

```
Branch = Union[Tree[str], str]
TreeOrCode = Union[Tree[str], str]
MatchResult = Dict[str, Tree]
_TEMPLATE_MARKER = '$'
```

名称 `Branch` 本身可以指代编程中已知的几个概念，对于许多开发人员来说，他们可能想到的第一个 *branch* 是像 GitHub 这样的版本控制系统中的分支。但 *branch* 也可以是一个领域概念，表示商店的某个实例（底特律分店）。如果你对解析了解一些，并且对 Lark 库有点熟悉，你可能会推断出这里指的 branch 是语法分析树的子树的名称。

#### 从行为中学习

变量名可以成为概念信息的来源。但是，名称并不是代码中了解一段代码中存在的概念的唯一地方。你也可以从对变量执行的操作或与变量进行的操作中识别概念。例如，如果你看到一行代码，如 `settings.next`，你可以推断出 `settings` 是一个可以迭代的类型，从而产生假设 `setting` 将是一个迭代器或流。




行为不仅仅局限于命名，你还可以从语法中学到更多。语法也是理解的丰富来源。许多语法元素与概念相关联，例如，使用点表示法的变量，如 `var.children`，表明 `var` 类似于一个具有可访问字段的对象或类。在大多数编程语言中，`children[0]` 意味着 `children` 是一个列表或数组。如果你熟悉该编程语言，当然这些东西不难识别。但根据我自己阅读代码以及在代码阅读研讨会中与他人一起阅读代码的经验，在认知负荷极高的环境下（如阅读未知的代码库），即使使用已知语言，你也必须有意识地关注这些提示，否则很容易错过它们。

此外，这些语法概念通常高度依赖于编程语言，因此在不太熟悉的语言代码库中全神贯注地阅读它们会更难。例如，在 C++ 中，如果你看到带有星号的类型，如 `mobj_t* thing`，以及使用 `thing->flags` 进行字段访问，则可以合理地得出结论：变量 `thing` 是一个指针。但这需要类 C 语言的知识，如果你习惯了 `.` 风格的访问，要意识到 `->` 也表示字段访问并检索内存，可能比你想象的更难，即使你很了解它！（见下一个侧边栏）。

这再次强调了在阅读代码时，对编程语言语法有扎实理解的重要性。一个像星号这样的符号很容易被忽略，但会从根本上改变你对变量类型的看法，从而改变你对代码库中概念的理解。

#### 练习 4.2 检查代码库中所有变量名中的概念

对于这个练习，写下代码库中应用于变量的操作。你能在操作中发现哪些编程概念？

| 变量名 | 与名称相关的操作 | 编程概念 |
| --- | --- | --- |
| `.update()` | `res.update(matches)` | `dictionary` |
| `[0]` | `return _get_template_name(var.children[0])` | `list` |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |

### 利用变量角色获得更深入的理解

变量名可以让你对代码库中的概念有很好的印象，但仅从变量本身你还能了解更多关于概念的信息。深入理解一段代码使用了什么概念的一个好方法是使用 **roles of variables**（变量角色）框架来分析它。

正如芬兰东大学的 Jorma Sajaniemi 所描述的，变量角色是一个框架，它给变量在代码中不同的使用方式赋予了精确的名称。

例如，在 `for i in range(classes):` 这样的循环中的变量 `i` 不仅仅是一个变量，甚至不仅仅是一个整数，它有一个更具体的角色，即逐个遍历范围的元素。即使不看循环体，你也可以相当确定地认为 `i` 将被用来索引列表 `classes`。如果你看到另一个名为 `max_class_size` 的整数变量，它对大多数程序员会有不同的含义，即使没有看到其他任何东西。这样的变量不太可能用于索引列表，而更可能保存一个表示班级学生最高人数的值。

虽然大多数程序员都知道并理解这些变量做不同的事情，但变量角色给了我们一个精确的词汇来思考这些差异。例如，上面例子中的 `i` 是一个 **stepper**（步进器），一个遍历固定值列表的变量，而 `max_class_size` 是一个 **most-wanted holder**（最想要持有者），一个存储已搜索值的变量。

Sajaniemi^(1) 描述了 11 种不同的角色，涵盖了你日常遇到的大多数程序。他的研究甚至表明，这 11 种角色涵盖了初学者编写的 99% 的程序。表 4.X 描述了变量在程序中可以扮演的不同角色。

| 角色 | 描述 | 示例 | 如何识别此角色 |
| --- | --- | --- | --- |
| **Fixed value**（固定值） | 一个在初始化后值不再改变的变量扮演固定值的角色。 | 固定值变量的例子包括数学常量如 `pi`，或从文件或数据库读取的数据。 | 在某些语言中常量可以被显式表达（如果值被改变会抛出错误），在其他语言中命名约定可能暗示常量值，如 Python 中的大写。但常量也可能只是一个初始化一次然后只赋值的变量。 |
| **Stepper**（步进器） | 在循环迭代时，总有一个变量遍历一系列值。这就是步进器的角色，其值在迭代开始时就可以预测。 | 遍历 `for` 循环的变量 `i` 是典型的步进器，但更复杂的步进器也是可能的，例如每次迭代都从 `i` 开始的内层循环变量 `j`。 | 通常是与循环一起使用的整数 `i`、`j`、`m` 或 `n`。 |
| **Flag**（标志） | 一个用于指示某事已发生或为真的变量。 | 典型例子有 `is_set`、`is_available` 或 `is_error`。标志通常是布尔值，但也可以是整数甚至字符串。 | 标志通常可以从变量名识别，指示状态，如 `is____` 或 `has____`。标志也常用于条件中，直接使用（`if is_set`）或带等值检查（`if value == "set"` 或 `1`，针对非布尔标志）。 |
| **Walker**（走者） | 走者也遍历数据结构，类似于步进器。区别在于遍历数据结构的方式。步进器总是遍历预先已知的一系列值。而走者是以在循环开始前未知的方式遍历数据结构的变量。 | 取决于编程语言，走者可以是指针或整数索引。走者可以遍历列表（例如在搜索中），但更常遍历栈或树等数据结构。 | 当我们不知道它们将遍历的确切值集合时，走者可用于循环（如搜索），或与链表等数据结构相关联，用于查找应添加新元素的位置。 |
| **Most recent holder**（最近持有者） | 一个在遍历一系列值时保存最近遇到的值的变量是最近持有者。 | 例如，它可能存储从文件读取的最新行，即 `line = file.readline()`，或步进器最后引用的数组元素的副本 `element = list[i]`。 | 最近持有者只能出现在循环体内，并且在循环的每次迭代中都会被更新。 |
| **Most wanted holder**（最想要持有者） | 当你遍历一系列值时，通常是为了搜索某个特定值。保存此搜索结果或迄今找到的最佳值的变量称为最想要持有者。 | 最想要持有者的例子包括存储最小值、最大值或满足特定条件的第一个值的变量。 | 与最近持有者一样，最想要持有者也只能出现在循环体内，但只有在找到更匹配搜索的值时（即比当前存储的最大值更高的值）才会更新。 |
| **Gatherer**（收集器） | 收集器是一个收集数据并将其聚合为单个值的变量。 | 这可以是一个从零开始并在遍历循环时收集值的变量，如下所示：<br><br>```<br>sum = 0<br>for i in range(list):<br>    sum += list[i]<br>``` |   |



### 变量角色

然而，其值也可以在函数式语言或包含特定函数式特性的语言中直接计算，即 `functional_total = sum(list)`。收集器的形式因编程语言而异。在命令式代码中，它们只能与循环结合出现，而在函数式语言中也可以用模式匹配来表达。

#### 容器

容器是指任何可添加和移除多个元素的数据结构。

容器的例子包括列表、数组、栈和树。在许多情况下，我们可以通过语法识别容器，因为它们使用特殊符号初始化，如 `customers = []` 或 `symbols = {}`。

#### 跟随者

某些算法要求你跟踪前一个或后一个值。扮演此角色的变量称为跟随者，且总是与另一个变量耦合。

跟随者变量的例子包括遍历链表时指向前一个元素的指针，或二分查搜中的下界索引。

#### 组织者

通常数据需要以某种方式处理。例如，在某些语言中，如果不先将字符串转换为字符数组，就无法访问字符串中的单个字符。或者你检索了一组元素，但也想存储该列表的排序版本以便稍后以各种方式过滤，一次排序比每次排序更有效。这些都是组织者的例子，组织者是仅用于重新排列或以不同方式存储值的变量。组织者通常是临时变量。

例子包括排序或过滤变量，例如在 pandas 中：

```
filtered = df[df["variable"] == "A"]
```

组织者总是与其他变量相关联，它们重塑或转换这些变量的值。

#### 临时变量

临时变量是仅短暂使用的变量，通常称为 `temp` 或 `t`。这些变量可用于交换数据，或存储在方法或函数中多次使用的计算结果。

示例：`temp`、`foo`、`bar`、`thing`

临时变量通常根据其名称识别，名称可能以 `temp` 开头或结尾，但并非总是如此。识别临时变量的另一种方法是注意到它们仅在紧邻的几行代码中使用。

#### 练习 4.3：检查代码库中所有变量名的角色

在本练习中，再次检查代码库中的变量列表，并尝试确定它们的角色。找到所有角色后，你可以填写表格的最后一列，记录与变量相关的编程概念。

| 变量名 | 角色 | 相关变量 | 编程概念 |
| --- | --- | --- | --- |
| `template` in `template = self._parse(template)` | 组织者 | `template` | 解析 |
| `res` in `res.update(matches)` | 收集者 | `matches` | 树模板 |

### 利用框架和库的依赖理解概念

要完全理解代码片段中涉及的概念，还必须注意从框架和库导入了哪些概念。例如，考虑你之前检查过的 Python Lark 代码片段中的这些导入语句：

```
from typing import Union, Optional, Mapping, Dict, Tuple, Iterator
from lark import Tree, Transformer
from lark.exceptions import MissingVariableError
```

第一行代码立即提供了一些正在使用的编程概念的见解：容器类型如 `Tuple` 和 `Mapping`，以及（函数式）数据结构如 `Union` 和 `Optional`。该导入行代码还告诉你（如果你对 Python 和该库有一定了解），你正在导入内置类型的类型化版本，如 `Dictionary` 和 `Tuple`，以便 Python 可以提供类型提示。

第二行告诉你 `Tree` 正在被使用和转换，最后一行告诉你在 Lark 支持的所有异常中，只导入了 `MissingVariableError`。因此，如果你想深入阅读这个特定的代码库，弄清楚还有哪些其他异常存在以及为什么这里只需要这一个会非常有趣。

检查代码片段中导入的使用位置也非常有帮助。例如在 Lark 代码片段中，导入的 `Tree` `Transformer` 仅在 `_ReplaceVars` 类中使用。这有助于你看到该类是该文件中唯一的转换器，因此在代码片段中扮演重要角色。

#### 练习 4.4：检查依赖

在本练习中，检查依赖项并（可选）阅读这些依赖项。从导入的概念中你能了解到关于代码的什么信息？代码中的哪些地方使用了这些概念？

| 导入的概念 | 导入自 | 编程概念 | 使用于 |
| --- | --- | --- | --- |
| `Transformer` | `lark` | (解析)树转换 | `_ReplaceVars` 类 |

### 读后

与其他章节一样，在检查代码中的编程概念后，你可以做一些事情。

#### 扩展概念文档

关于编程概念和行为的部分信息可以作为添加到自述文件的优秀文档。虽然项目的核心维护者可能非常熟悉大多数编程概念，但在项目文档中解释基础知识对项目新手来说是非常有价值的补充。

#### 记录变量角色

在检查代码库中变量的角色后，你可以做出的另一个贡献是将该信息添加到代码库中。你在练习 4.3 中制作的表格可以作为一个很好的起点。根据代码库的风格，变量的角色可以编码到变量名中、作为注释添加或在代码库外部记录。作为具体的第一步，明确标记所有临时变量（例如使用 `temp_` 前缀）可以使未来的重构容易得多。

#### 扩展依赖文档

练习 4.4 帮助我们理解了代码库中的依赖关系……更多内容即将推出。

### 总结

代码可以从不同维度阅读：结构、领域、概念、上下文和协作。与第 4 章一样，本章探讨了变量、函数和对象的命名方式，但从编程概念的角度出发。除了名称，代码上的操作（无论是用户定义的函数如 `.parse()`，还是语法如 `[]`）也能帮助我们理解代码库中存在的编程概念。

下一章将探讨如何更深入地理解代码片段的上下文，包括注释和文档。

^[1] https://www.ppig.org/files/2005-PPIG-17th-sjaniemi.pdf
