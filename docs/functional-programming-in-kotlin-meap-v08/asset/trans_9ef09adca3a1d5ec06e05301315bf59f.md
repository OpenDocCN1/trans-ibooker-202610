
可能的表示。我们应该能够想出一个简单、纯函数式的 Parser 表示，用于实现 Parsers 接口。但首先，让我们表达将作为实现起点使用的顶层构造。

### 清单 9.10 解析器组合子库的顶层表示

```
interface Parser<A>

data class ParseError(val stack: List<Pair<Location, String>>)

abstract class Parsers<PE> {
    internal abstract fun <A> or(p1: Parser<A>, p2: Parser<A>): Parser<A>
}

open class ParsersImpl<PE>() : Parsers<PE>() {
    override fun <A> or(p1: Parser<A>, p2: Parser<A>): Parser<A> = TODO()
}

abstract class ParserDsl<PE> : ParsersImpl<PE>() {
    infix fun <A> Parser<A>.or(p: Parser<A>): Parser<A> =
        this@ParserDsl.or(this, p)
}

object Example : ParserDsl<ParseError>() {
    init {
        val p1: Parser<String> = TODO()
        val p2: Parser<String> = TODO()
        val p3 = p1 or p2
    }
}
```

- 1. 无论我们为 Parser 发现什么表示，这只是一个示例
- 2. Parsers 类保存所有未实现的原始和组合子代数定义
- 3. ParsersImpl 是 Parsers 的具体实现
- 4. ParserDsl 添加语法糖以使使用组合子更方便
- 5. 通过 this@ParserDsl 访问 ParserImpl 中的 or 函数，这是防止循环引用的变通方法。
- 6. 一个使用我们组合子库的对象

## 9.6.1 逐步构建代数实现

我们终于要讨论一个具体的 Parsers 实现，它满足迄今为止积累的所有特性。与其直接跳到最终的 Parser 表示，我们将逐步构建它。我们将通过检查代数的原语，然后推理支持每个原语所需的信息来实现这一点。

让我们从 string 组合子开始：

```
fun string(s: String): Parser<String>
```

我们也知道需要支持 run 函数：

```
fun <A> run(p: Parser<A>, input: String): Either<PE, A>
```

作为第一次尝试，我们可以假设我们的 Parser 只是 run 函数的实现：

```
typealias Parser<A> = (String) -> Either<ParseError, A>
```

我们可以用它来实现 string 原语如下：

### 清单 9.11 以 Location 为基础的字符串原语实现

```
override fun string(s: String): Parser<String> =
{ input: String ->
    if (input.startsWith(s))
        Right(s)
    else Left(Location(input).toError("Expected: $s"))
}

private fun Location.toError(msg: String) =
    ParseError(listOf(Pair(this, msg)))
```

- ① 使用 toError 构造一个 ParseError
- ② 将 Location 转换为 ParseError 的扩展

string 的 else 分支必须构建一个 ParseError。目前构造它们很不方便，因此我们在 Location 上引入了一个名为 toError 的辅助扩展函数。

## 9.6.2 依次排列解析器

到目前为止，一切顺利。我们有一个至少支持 string 的 Parser 表示。让我们继续讨论解析器的序列化。不幸的是，要表示像 "abra" product "cadabra" 这样的解析器，我们现有的表示是不够的。如果 "abra" 的解析成功，那么在对剩余字符运行 "cadabra" 解析器之前，我们希望将这些字符视为已消耗。因此，为了支持序列化，我们需要一种让 Parser 指示它消耗了多少字符的方法。考虑到 Location 包含完整的输入字符串和该字符串中的偏移量，捕获这一点其实很容易。

### 清单 9.12 将结果表示为 ADT 以跟踪已消耗的字符

```
typealias Parser<A> = (Location) -> Result<A>

sealed class Result<out A>
data class Success<out A>(val a: A, val consumed: Int) : Result<A>()
data class Failure(val get: ParseError) : Result<Nothing>()
```

- ① Parser 的函数定义现在返回一个 Result<A>
- ② Success 类型携带已消耗字符的计数

我们刚刚引入了一个更丰富的替代数据类型 Result，而不是像以前那样使用简单的 Either。

成功时，我们返回类型为 A 的值，以及已消耗的输入字符数。调用者随后可以使用此计数来更新 `Location` 状态。这种类型开始触及 `Parser` 的本质——它是一种可能失败的状态动作，类似于我们在第 6 章中构建的那样。它接收输入状态，并在成功时返回一个值以及足够的信息来控制状态应如何更新。

将 `Parser` 理解为只是状态动作，为我们提供了一种构建表示的方法，该表示支持我们迄今为止规定的所有花哨的组合子和定律。我们只需考虑每个原语要求我们的状态类型跟踪什么，然后逐步了解每个组合子如何转换此状态。

**EXERCISE 9.11 (Hard)**
为此 `Parser` 表示实现 `string`、`regex`、`succeed` 和 `slice`。已包含一些私有辅助函数存根，以引导你找到正确的方向。

**NOTE**
`slice` 可能比它本可以的效率更低，因为它仍必须构造一个值然后丢弃它。不要在当前练习中处理这个问题。

```
override fun string(s: String): Parser<String> = TODO()

private fun firstNonMatchingIndex(
    s1: String,
    s2: String,
    offset: Int
): Option<Int> = TODO()

private fun State.advanceBy(i: Int): State = TODO()

override fun regex(r: String): Parser<String> = TODO()

private fun String.findPrefixOf(r: Regex): Option<MatchResult> = TODO()

override fun <A> succeed(a: A): Parser<A> = TODO()

override fun <A> slice(p: Parser<A>): Parser<String> = TODO()

private fun State.slice(n: Int): String = TODO()
```

## 9.6.3 通过标记解析器捕获错误消息

在我们的原语列表中继续向下，让我们接下来查看 `scope`。在失败的情况下，我们希望将一条新消息推入 `ParseError` 堆栈。让我们在 `ParseError` 上引入一个辅助函数来实现这一点。我们称之为 `push` 48。

### 清单 9.13 将错误推入 ParseError 堆栈头的扩展函数

```
fun ParseError.push(loc: Location, msg: String): ParseError =
    this.copy(stack = Pair(loc, msg) cons this.stack)
```

有了它，我们就可以使用接下来将描述的 Result 上的 mapError 扩展方法来实现 scope：

### 清单 9.14 使用 push 记录错误的 scope 实现

```
fun <A> scope(msg: String, pa: Parser<A>): Parser<A> =
    { state -> pa(state).mapError { pe -> pe.push(state, msg) } }
```

mapError 扩展方法允许在失败时转换错误，其定义如下：

### 清单 9.15 在 Result 失败时映射 ParseError 的扩展函数

```
fun <A> Result<A>.mapError(f: (ParseError) -> ParseError): Result<A> =
    when (this) {
        is Success -> this
        is Failure -> Failure(f(this.get))
    }
```

因为我们在内部解析器返回后才将内容推入堆栈，所以堆栈底部将具有在解析后期发生的更详细的消息。例如，如果 scope(msg1, a product scope(msg2, b)) 在解析 b 时失败，堆栈上的第一个错误将是 msg1，其后是 a 生成的任何错误，然后是 msg2，最后是 b 生成的错误。

我们可以类似地实现 tag，但它不是将内容推入错误堆栈，而是替换已有的内容。我们可以使用 mapError 和一个也称为 tag 的 ParseError 扩展再次编写它，这将在之后讨论：

### 清单 9.16 使用 tag 记录错误的 tag 实现

```
fun <A> tag(msg: String, pa: Parser<A>): Parser<A> =
    { state ->
        pa(state).mapError { pe ->
            pe.tag(msg) ①
        }
    }
```

- ① 调用 ParseError 上的辅助方法，也命名为 tag。

我们向 ParseError 添加了一个也命名为 tag 的辅助扩展函数。我们将做出一个设计决策，即 tag 会修剪错误堆栈，截断来自内部作用域的更详细消息，仅使用堆栈底部最近的位置。这就是它的样子：
