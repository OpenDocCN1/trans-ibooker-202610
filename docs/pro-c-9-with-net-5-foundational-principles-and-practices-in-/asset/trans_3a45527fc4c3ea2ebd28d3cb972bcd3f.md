

## 定义逐字字符串（8.0 版更新）

当你用 @ 符号为字符串字面量加上前缀时，你就创建了所谓的逐字字符串。使用逐字字符串，你会禁用字面量的转义字符处理并原样打印出字符串。当处理表示目录和网络路径的字符串时，这非常有用。因此，与其使用 \\ 转义字符，不如直接写如下内容：

```
// 以下字符串被原样打印，
// 因此所有转义字符都会被显示。
Console.WriteLine(@"C:\MyApp\bin\Debug");
```

还要注意，逐字字符串可用于保留跨越多行的字符串的空白。

// 逐字字符串会保留空白。
string myLongString = @"This is a very
    very
        very
            long string";
Console.WriteLine(myLongString);

使用逐字字符串，你还可以通过重复 " 标记来直接在字面量字符串中插入双引号。

```
Console.WriteLine(@"Cerebus said ""Darrr! Pret-ty sun-sets""");
```

逐字字符串也可以是插值字符串，只需同时指定插值运算符（$）和逐字运算符（@）。

```
string interp = "interpolation";
string myLongString2 = $@"This is a very
    very
        long string with {interp}";
```

C# 8 中的新特性，顺序无关紧要。使用 $@ 或 @$ 都可以。

## 处理字符串和相等性

正如第 4 章将全面解释的那样，引用类型是分配在垃圾回收的托管堆上的对象。默认情况下，当你对引用类型执行相等性测试（通过 C# 的 == 和 != 运算符）时，如果引用指向内存中的同一对象，则返回 true。然而，即使字符串数据类型确实是引用类型，相等运算符已被重新定义为比较字符串对象的值，而不是它们在内存中引用的对象。

```
static void StringEquality()
{
    Console.WriteLine("=> String equality:");
    string s1 = "Hello!";
    string s2 = "Yo!";
    Console.WriteLine("s1 = {0}", s1);
    Console.WriteLine("s2 = {0}", s2);
    Console.WriteLine();

// Test these strings for equality.
Console.WriteLine("s1 == s2: {0}", s1 == s2);
Console.WriteLine("s1 == Hello!: {0}", s1 == "Hello!");
Console.WriteLine("s1 == HELLO!: {0}", s1 == "HELLO!");
Console.WriteLine("s1 == hello!: {0}", s1 == "hello!");
Console.WriteLine("s1.Equals(s2): {0}", s1.Equals(s2));
Console.WriteLine("Yo!.Equals(s2): {0}", "Yo!".Equals(s2));
Console.WriteLine();
}
```

C# 的相等运算符默认对字符串对象执行区分大小写、文化不敏感、逐字符的相等性测试。因此，"Hello!" 不等于 "HELLO!"，后者也不同于 "hello!"。此外，考虑到字符串与 System.String 之间的联系，注意你也可以使用 String 的 Equals() 方法以及内置的相等运算符来测试相等性。最后，由于每个字符串字面量（如 "Yo!"）都是有效的 System.String 实例，你可以从固定字符序列中访问字符串相关的功能。

## 修改字符串比较行为

如前所述，字符串相等运算符（Compare()、Equals() 和 ==）以及 IndexOf() 函数默认区分大小写且文化不敏感。如果你的程序不关心大小写，这可能会导致问题。一种解决方法是将所有内容转换为大写或小写然后再比较，像这样：

```
if (firstString.ToUpper() == secondString.ToUpper())
{
    //Do something
}
```

这会为每个字符串创建一个副本，其中所有字母均为小写。在大多数情况下这可能不是问题，但在处理非常大的字符串时可能会带来性能损失。即使不是性能问题，每次都要这样写也很麻烦。而且如果忘记调用 ToUpper() 呢？这可能导致程序中难以发现的错误。

更好的做法是使用前面列出的方法的重载，这些重载接受 StringComparison 枚举的值来精确控制比较方式。表 3-7 描述了 StringComparison 的值。

### 表 3-7. StringComparison 枚举的值

| C# 相等/关系运算符 | 含义 |
|---|---|
| CurrentCulture | 使用文化敏感的排序规则和当前文化比较字符串 |
| CurrentCultureIgnoreCase | 使用文化敏感的排序规则和当前文化比较字符串并忽略比较字符串的大小写 |
| InvariantCulture | 使用文化敏感的排序规则和不变文化比较字符串 |
| InvariantCultureIgnoreCase | 使用文化敏感的排序规则和不变文化比较字符串并忽略比较字符串的大小写 |
| Ordinal | 使用序数（二进制）排序规则比较字符串 |
| OrdinalIgnoreCase | 使用序数（二进制）排序规则比较字符串并忽略比较字符串的大小写 |

要查看使用 StringComparison 选项的效果，创建一个名为 StringEqualitySpecifyingCompareRules() 的新方法并添加以下代码：

```
static void StringEqualitySpecifyingCompareRules()
{
    Console.WriteLine("=> String equality (Case Insensitive:" );
    string s1 = "Hello!";
    string s2 = "HELLO!";
    Console.WriteLine("s1 = {0}", s1);
    Console.WriteLine("s2 = {0}", s2);
    Console.WriteLine();

// Check the results of changing the default compare rules.
Console.WriteLine("Default rules: s1={0},s2={1} s1.Equals(s2): {2}", s1, s2, s1.Equals(s2));
Console.WriteLine("Ignore case: s1.Equals(s2, StringComparison.OrdinalIgnoreCase): {0}", s1.Equals(s2, StringComparison.OrdinalIgnoreCase));
Console.WriteLine("Ignore case, Invariant Culture: s1.Equals(s2, StringComparison.InvariantCultureIgnoreCase): {0}", s1.Equals(s2, StringComparison.InvariantCultureIgnoreCase));
Console.WriteLine();
Console.WriteLine("Default rules: s1={0},s2={1} s1.IndexOf(\"E\"): {2}", s1, s2, s1.IndexOf("E"));
Console.WriteLine("Ignore case: s1.IndexOf(\"E\", StringComparison.OrdinalIgnoreCase): {0}", s1.IndexOf("E", StringComparison.OrdinalIgnoreCase));
Console.WriteLine("Ignore case, Invariant Culture: s1.IndexOf(\"E\", StringComparison.InvariantCultureIgnoreCase): {0}", s1.IndexOf("E", StringComparison.InvariantCultureIgnoreCase));
Console.WriteLine();
}
```

这里的示例很简单，并且在大多数文化中使用相同的字母，如果你的应用程序需要考虑不同的文化集，使用 StringComparison 选项是必须的。

## 字符串是不可变的

System.String 的一个有趣方面是，在你用初始值分配字符串对象后，其字符数据就无法更改。乍一看，这似乎完全是谎言，因为你总是将字符串重新分配给新值，并且 System.String 类型定义了许多似乎以一种或另一种方式修改字符数据的方法（例如大写和小写）。然而，如果你更仔细地观察幕后发生的事情，你会注意到字符串类型的方法实际上是返回一个以修改格式的新字符串对象。

```
static void StringsAreImmutable()
{
    Console.WriteLine("=> Immutable Strings:\a");
    // Set initial string value.
    string s1 = "This is my string.";
    Console.WriteLine("s1 = {0}", s1);

// Uppercase s1?
string upperString = s1.ToUpper();
Console.WriteLine("upperString = {0}", upperString);

// Nope! s1 is in the same format!
Console.WriteLine("s1 = {0}", s1);
}
```

如果你检查以下相关输出，你可以验证在调用 ToUpper() 时原始字符串对象（s1）并未被大写。相反，你会得到一个以修改格式的字符串副本。

```
s1 = This is my string.

upperString = THIS IS MY STRING.
s1 = This is my string.
```

使用 C# 赋值运算符时，不可变性定律同样适用。为了说明这一点，实现以下 StringsAreImmutable2() 方法：

```
static void StringsAreImmutable2()
{
    Console.WriteLine("=> Immutable Strings 2:\a");
    string s2 = "My other string";
    s2 = "New string value";
}
```

现在，编译你的应用程序并运行 ildasm.exe（参见第 1 章）。以下输出显示了如果你为 StringsAreImmutable2() 方法生成 CIL 代码会找到的内容：

```
.method private hidebysig static void  StringsAreImmutable2() cil managed
{
    // Code size       21 (0x15)
    .maxstack  1
    .locals init (string V_0)
    IL_0000:  nop
    IL_0001:  ldstr      "My other string"
    IL_0006:  stloc.0
    IL_0007:  ldstr      "New string value" /* 70000B3B */
    IL_000c:  stloc.0
    IL_000d:  ldloc.0
    IL_0013:  nop
    IL_0014:  ret
} // end of method Program::StringsAreImmutable2
```

虽然你尚未检查 CIL 的低级细节，但请注意对 ldstr（加载字符串）操作码的多次调用。简单来说，CIL 的 ldstr 操作码在托管堆上加载一个新的字符串对象。包含值 "My other string" 的先前字符串对象最终将被垃圾回收。

