# 第 3 章 Python 教程

本章向可能没有偏见检测和缓解背景的感兴趣开发者介绍 AIF 360 的基本功能。它提供了一个 Python 教程，通过使用优化预处理算法进行收入估计的示例，演示如何检测和缓解种族偏见。您也可以在 [GitHub 上找到本教程](https://oreil.ly/CaIql)，那里的作者教程和演示笔记本中提供了更复杂的机器学习工作流。^(1)

正如这些示例所示，偏见检测和/或缓解工具包需要针对特定的关注偏见进行定制。更具体地说，它需要知道称为*受保护属性*的感兴趣属性：种族是受保护属性的一个例子；年龄是第二个。

我们将使用成年人口普查收入数据集，将其拆分为训练集和测试集。我们将寻找创建机器学习模型过程中的偏见，该模型旨在根据各种个人属性帮助决定个人的年收入是否应设定为大于 50,000 美元。受保护属性将是种族，“1”（白人）和“0”（非白人）分别为特权群体和非特权群体的值。在本教程中，我们首先检查初始训练数据中的偏见，缓解偏见，然后重新检查。

涉及的步骤如下：

1.  编写导入语句。
2.  设置偏见检测选项，加载数据集，并拆分为训练集和测试集。
3.  在原始训练数据集上计算公平性指标。
4.  通过转换原始数据集来缓解偏见。
5.  在转换后的训练数据集上计算公平性指标。

## 步骤 1：导入语句

与任何 Python 程序一样，第一步是导入必要的包。在下面的代码中，我们从 AIF360 包导入几个组件。我们导入了一个自定义版本的成人数据集（其中某些连续特征被分类）、用于检查偏见的指标，以及与我们用于缓解偏见的算法相关的类。我们还导入了一些其他有用的包：



## 偏见缓解教程 (AIF360)

### 步骤 2：加载数据集，指定受保护属性，并将数据集拆分为训练集和测试集

在步骤 2 中，我们加载初始数据集，将受保护属性设置为 `race`。然后我们将原始数据集拆分为训练数据集和测试数据集。虽然在本教程中我们仅使用训练数据集，但正常的工作流程也会使用测试数据集来评估机器学习模型开发过程中的功效（准确性、公平性等）。最后，我们为种族属性的特权值 (`1`) 和非特权值 (`0`) 设置了两个变量（将在步骤 3 中使用）。这些是检测和缓解偏见的关键输入，将在第 3 步和第 4 步中用到：

```python
import sys
sys.path.append("../")

import numpy as np

from aif360.metrics import BinaryLabelDatasetMetric

from aif360.algorithms.preprocessing.optim_preproc import \
    OptimPreproc
from aif360.algorithms.preprocessing.optim_preproc_helpers. \
    data_preproc_functions import load_preproc_data_adult
from aif360.algorithms.preprocessing.optim_preproc_helpers. \
    distortion_functions import get_distortion_adult
from aif360.algorithms.preprocessing.optim_preproc_helpers. \
    opt_tools import OptTools

from IPython.display import Markdown, display
In [2]:
np.random.seed(1)
```

```python
dataset_orig = load_preproc_data_adult(['race']) 

dataset_orig_train, dataset_orig_test = \
    dataset_orig.split([0.7], shuffle=True) 

privileged_groups = [{'race': 1}] # White
unprivileged_groups = [{'race': 0}] # Not white
```

### 步骤 3：在原始训练数据集上计算公平性指标

现在我们已经确定了受保护属性 `race`，并定义了特权值和非特权值，可以使用 AIF360 来检测数据集中的偏见。一个简单的测试是比较特权群体和非特权群体的有利结果百分比，用后者减去前者。负值表示非特权群体的有利结果较少。这在 `BinaryLabelDatasetMetric` 类的 `mean_difference()` 方法中实现。下面的代码执行此检查并显示输出：

```python
metric_orig_train = BinaryLabelDatasetMetric(dataset_orig_train, 
              unprivileged_groups=unprivileged_groups, 
              privileged_groups=privileged_groups) 
display(Markdown("#### Original training dataset"))
print("Difference in mean outcomes between unprivileged and \
    privileged groups = %f" % \
    metric_orig_train.mean_difference())
```

原始训练数据集  
非特权群体和特权群体之间平均结果的差异 = -0.104553

### 步骤 4：通过转换原始数据集来缓解偏见

上一步显示，特权群体在训练数据集中获得的正面结果多出 10.5%。因为这是不可取的，我们将尝试缓解训练数据集中的这种偏见。如前所述，这称为预处理缓解，因为它发生在模型创建之前。

我们选择优化预处理算法[²]，该算法实现于 `aif360.algorithms.preprocessing` 目录下的 `OptimPreproc` 类中。该算法转换数据集，使特权群体和非特权群体在受保护属性上的正面结果更加公平。

该算法需要一些调优参数，这些参数在 `optim_options` 变量中设置，并连同其他参数（包括步骤 3 中定义的包含非特权群体和特权群体的两个变量）作为参数传递。

然后我们调用 `fit()` 和 `transform()` 方法来执行转换，生成一个新的转换后训练数据集 (`dataset_transf_train`)。最后，我们确保转换后数据集与原始数据集之间的特征对齐，以便进行比较：

```python
optim_options = {
    "distortion_fun": get_distortion_adult, 
    "epsilon": 0.05, 
    "clist": [0.99, 1.99, 2.99], 
    "dlist": [.1, 0.05, 0] 
}
OP = OptimPreproc(OptTools, optim_options) 
 
OP = OP.fit(dataset_orig_train) 
dataset_transf_train = OP.transform(dataset_orig_train, \
    transform_Y=True) 

dataset_transf_train = dataset_orig_train.align_datasets( \
    dataset_transf_train)
```

优化预处理：目标收敛至 0.000000

### 步骤 5：在转换后的数据集上计算公平性指标

现在我们有了转换后的数据集，可以通过使用步骤 3 中用于原始训练数据集的相同指标，来检查我们消除偏见的努力有多有效。同样，我们使用 `BinaryLabelDatasetMetric` 类中的 `mean_difference()` 函数：

```python
metric_transf_train = BinaryLabelDatasetMetric( \
    dataset_transf_train, unprivileged_groups= \
    unprivileged_groups, privileged_groups=privileged_groups) 
display(Markdown("#### Transformed training dataset"))
print("Difference in mean outcomes between unprivileged and \
    privileged groups = %f" % metric_transf_train. \
    mean_difference())
```

转换后的训练数据集  
非特权群体和特权群体之间平均结果的差异 = -0.051074

我们看到缓解步骤非常有效：平均结果的差异现在是 –0.051074。因此，特权群体的优势从 10.5% 降至 5.1%——减少了一半多！

一个更完整的用例将采取下一步，查看转换后的数据集如何影响训练模型的准确性和公平性。这在 AIF360 工具包 examples 目录下的演示笔记本 `demo_reweighing_preproc.ipynb` 中实现。

[¹]: 此演示与[信用评分教程](https://oreil.ly/H_-eJ)非常相似。它旨在作为一个替代介绍，使用不同的数据集和缓解算法。  
[²]: F. Calmon 等人，“用于防止歧视的优化预处理”，载于《神经信息处理系统进展》，2017 年。
