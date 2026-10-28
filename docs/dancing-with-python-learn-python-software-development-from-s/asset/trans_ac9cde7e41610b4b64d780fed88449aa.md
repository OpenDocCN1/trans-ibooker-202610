

## B.2 qiskit.org

Qiskit 开源量子计算软件开发工具包（SDK）的主页是

https://qiskit.org/

了解 **qiskit** 的权威来源是 Qiskit 教科书，网址为

https://qiskit.org/textbook/preface.html

它提供了量子计算的综述以及交互式 Python 示例。[LQCQ]

Qiskit 文档的主网页是

https://qiskit.org/documentation/index.html

所有版本的发布说明位于

https://qiskit.org/documentation/release_notes.html

你可以在 Python 中执行以下命令来查看已安装的 **qiskit** 版本及其组成模块：

```python
import qiskit

qiskit.__qiskit_version__
```

```text
{'qiskit-terra': '0.18.0', 'qiskit-aer': '0.8.2', 'qiskit-ignis': '0.6.0', 'qiskit-ibmq-provider': '0.16.0', 'qiskit-aqua': '0.9.4', 'qiskit': '0.28.0', 'qiskit-nature': None, 'qiskit-finance': None, 'qiskit-optimization': None, 'qiskit-machine-learning': None}
```

这是在发布说明中引用的版本：

```python
qiskit.__qiskit_version__["qiskit"]
```

```text
'0.28.0'
```

## B.3 Python 专家网站

有很多网站可以找到关于用 Python 做各种事情的信息。其中许多是正确的，并且很多使用 Python 3。请寻找最近五年内撰写的文章。如果你看到使用 *print* 且后面没有左括号的代码，那就不要读了。他们讨论的是 Python 2，而不是 Python 3。

我认为 **Real Python** 网站

https://realpython.com/

一直提供关于 Python 编码大多数方面的有用且全面的文章。你可以免费阅读的文章数量有限，之后他们会要求你注册并付费。（我与他们没有商业关系。）

**W3Schools** 上的 Python 教程

https://www.w3schools.com/python/

是查找 Python 示例的好地方。这些示例可能会让你想起完成特定任务的语法或函数或方法。

我建议你访问并使用我们在本书中使用的四个主要 Python 包的网站：**matplotlib**、**numpy**、**pandas** 和 **sklearn**：

- https://matplotlib.org/
- https://numpy.org/
- https://pandas.pydata.org/
- https://sklearn.org/

## B.4 提问与获取答案

当你在网上搜索如何用 Python 做某事时，你很有可能会最终到达 **Stack Overflow**：

https://stackoverflow.com/questions/tagged/python

你会看到关于简单和具有挑战性问题的问答。如果你登录并参与，随着时间推移，你可以对问题和答案进行点赞和点踩。许多答案是有帮助的，但有些是不正确的或由非专家发布的。无论如何，你很可能找到一些可以用于或修改为你项目所用的代码。

即使你暂时不需要这些代码，浏览人们提出的问题也能让你学到很多。你会了解别人在用 Python 做什么。你可以提出自己的问题，但**始终**要先进行彻底的网络搜索。如果你不先做基本研究而浪费专家的时间，他们会感到沮丧。

## 完整的 UniPoly 类

本附录包含我们在*第 7 章，将对象组织成类*中开发的一元多项式 **UniPoly** 类的完整且最终的定义。

```python
"""The unipoly module contains the UniPoly class for polynomials.
"""

# The next line is for the spell-checker in Visual Studio Code
# cspell:ignore radd rsub rmul

class UniPoly:
    """Polynomial with one variable and integer coefficients.

    UniPoly creates a univariate polynomial with a single term
    and an integer coefficient. Polynomials may use different
    variables, here called 'indeterminates', but their names
    must each be a single alphabetic character. These polynomials
    are immutable.

    Parameters
    ----------
    coefficient : int, optional
        The coefficient of the term. Default value is 1.
    indeterminate : str, optional
        The "variable." Must be a single alphabetic character.
        Default value is 'x'.
    exponent : int, optional
        Default value is 0.
    """

    def __init__(self, coefficient=1, indeterminate='x', exponent=0):
        if not isinstance(indeterminate, str) or len(indeterminate) != 1 or \
                not indeterminate[0].isalpha():
            raise ValueError(
                "The indeterminate for a UniPoly must "
                "be an alphabetic str of length 1.")

        self.__indeterminate = indeterminate

        if coefficient != 0:
            self.__terms = {exponent: coefficient}
        else:
            self.__terms = dict()

    # ----------------------------------------------------------------------------
    # Magic methods
    # ----------------------------------------------------------------------------

    def __add__(self, other):
        """Adds a polynomial to a polynomial or an integer.

        Parameters
        ----------
        other : :class:`UniPoly` or int
            A polynomial or integer.

        Notes
        -----
        If the second argument is not a polynomial or integer,
        or if the indeterminates in non-constant polynomials
        are different, returns NotImplemented.

        Returns
        -------
        :class:`UniPoly` or NotImplemented
            The sum of self and other, or NotImplemented.
        """

        if not self.__terms:
            return other

        # if other is an integer, create a constant polynomial
        if isinstance(other, int):
            other = UniPoly(other, self.__indeterminate, 0)

        # both objects are polynomials

        if isinstance(other, UniPoly):
            # if other == 0, return self
            if not other.__terms:
                return self

            if self.__indeterminate != other.__indeterminate:
                if self.is_constant and other.is_constant:
                    return UniPoly(self.constant_coefficient + other.constant_coefficient, self.__indeterminate, 0)

                return NotImplemented

            new_poly = UniPoly(0, self.__indeterminate, 0)

            for exponent, coefficient in self.__terms.items():
                if exponent not in other.__terms:
                    new_poly.__terms[exponent] = coefficient
                else:
                    sum = self.__terms[exponent] \
                        + other.__terms[exponent]
                    if sum:
                        new_poly.__terms[exponent] = sum

            for exponent, coefficient in other.__terms.items():
                if exponent not in self.__terms:
                    new_poly.__terms[exponent] = coefficient

            return new_poly

        return NotImplemented

    def __radd__(self, other):
        """Compute other + self.

        Returns
        -------
        :class:`UniPoly`
            The sum of other and self.
        """
        # Addition is commutative, so we call __add__.
        return self.__add__(other)

    def __bool__(self):
        """Returns True if the polynomial is non-zero, False otherwise.

        Returns
        -------
        bool
        """
        return bool(self.__terms)

    def __call__(self, x):
        """Evaluates a polynomial by substituting `x` for the indeterminate.

        Python calls this when it sees a polynomial being used in function application. If `p` is a polynomial, then `p(3.4)` substitutes 3.4 for the indeterminate and does the math as expressed by the polynomial.

        Parameters
        ----------
        x : any object that supports -, +, *, **
            The value to be substituted for the indeterminate.

        Returns
        -------
        Probably `type(x)`
        """
        result = 0

        for exponent, coefficient in self.__terms.items():
            result += coefficient * x**exponent

        return result

    def __copy__(self):
        """Copies a polynomial, returning a new polynomial.

        This creates a deep copy of a polynomial, even if the original was zero.

        See Also
        --------
        `deepcopy`

        Returns
        -------
        :class:`UniPoly`
            A complete copy of the polynomial.
        """
        new_poly = UniPoly(0, self.__indeterminate)
        for exponent in self.__terms:
            new_poly.__terms[exponent] = self.__terms[exponent]

        return new_poly

    def __deepcopy__(self, memo):
        """Copies a polynomial, returning a new polynomial.

        This creates a deep copy of a polynomial, even if the original was zero.

        See Also
        --------
        `copy`

        Returns
        -------
        :class:`UniPoly`
            A complete copy of the polynomial.
        """
        return self.__copy__()

    def __eq__(self, other):
        """Tests for equality between a polynomial and polynomial or `int`.

        Parameters
```

