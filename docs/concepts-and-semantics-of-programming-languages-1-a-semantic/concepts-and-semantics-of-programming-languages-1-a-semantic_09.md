## 8. 异常

### 8.1. 错误：通知与传播

执行程序时可能会发生错误。它们可能源于程序员所做的选择，程序员可以指定某些条件，这些条件必须停止执行。例如，计算年龄列表平均值的程序无法以与正值相同的方式处理负年龄值。还可能发生其他与程序员意图不直接相关的错误，例如尝试除以 0 或由于权限缺失而尝试非法访问文件。

为了编写健壮的程序，任何程序员都必须意识到这些可能的错误，并在错误发生时防止其后果。最基本的解决方案是在整个程序中包含多个检查，并实现一个从错误检测到管理的机制来突出错误结果。应检查每个函数调用的结果，如果该值表明在调用执行期间发生了错误，则必须返回错误。下面使用简单的伪语言概述了若干错误通知和处理方法。

```
f ()
    ...
    if ... then return error
    ...
g ()
    r = f ()
    if r is error then return error
    ...
h ()
    ...
    r = g ()
    if r is error then return error
...
```

这种完全依赖程序员的机制存在多个缺点。算法中错误传播是显式的。此外，忘记检查函数调用结果的有效性可能不被察觉。此外，必须能够在返回值中通知错误。当函数的值域为部分集时，这很容易：只需将错误编码为值域外的值即可。例如，计算其参数之间差值的绝对值的函数（假设参数均为严格正数）只能返回正整数或零整数值。因此，错误可以使用 −1、−2 等表示，如下所示：

```
abs_diff_positive_ints (x, y)
if x = 0 or y = 0 then return (-1)
if x < 0 or y < 0 then return (-2)
if x > y then return (x - y)
else return (y - x)
```

如果函数的值域为全集，则这种编码错误值的机制变得不可能：所有可返回的值都是合法值。换句话说，没有可用值来表示错误。在下面（过于简单化的）计算受力质量加速度的示例中，如果质量为零，我们不知道应该返回什么值。

```
accel (force, mass)
if mass = 0 then return ???
else return (force / mass)
```

某些语言原生包含 n 元组。在这种情况下，可以返回一个对，第一个组件是一个标志，指示结果是否有意义，另一个组件是结果的值。如果标志指示错误，则结果可以选择任何值，因为已知它是无意义的。

```
accel (force, mass)
if mass = 0 then return (false, 42)
else return (true, (force / mass))
```

在具有和类型的语言中，返回值的类型也可以修改为包含有效性或无效性的概念。

```
type result = Error | Valid (float)
accel (force, mass)
if mass = 0 then return (Error)
else return (Valid (force / mass))
```

对于没有原生 n 元组或和类型的语言，有三种可能的解决方案。

#### 8.1.1. 全局变量

第一个解决方案（极难维护）包含使用一个全局变量作为有效性标志。

```
var validity
accel (force, mass)
if mass = 0 then
    validity = false
    return 42
else
    validity = true
    return (force / mass)
f ()
...
v = accel (...)
if validity then ...
else ...
```

如第 7.4 节所示，此解决方案降低了可维护性和可读性。定义与需要返回错误的函数数量相同的全局变量，或在多个函数之间共享同一个全局变量，都不能简化代码审查和维护。尽管如此，此机制在 C 标准库中用于详细说明某些系统调用和函数中的错误。全局变量 errno 由这些函数作为副作用修改，添加错误信息。

#### 8.1.2. 记录定义

如第 7.4 节所示，即使语言不原生支持 n 元组，仍然可以创建一个两字段的记录。但是，必须为每种可能的返回值类型创建一个记录类型，因此这种解决错误通知问题的方案也不理想。

#### 8.1.3. 按地址传递

某些语言（如 C）允许按地址传递参数（参见第 7.4 节）。与传递“常规”值不同，我们可以传递内存区域的地址，例如调用函数中变量的地址。这样，被调用的函数将在此地址写入。这样，可以将信息从被调用函数传递给调用者，无论是无错误结果还是错误通知，如示例 8.1 所示。

#### 示例 8.1.-

```
float accel(float force, float mass, bool *valid) {
 if (mass == 0) {
  *valid = false ;
  return 42 ;
 }
 *valid = true ;
 return (force / mass) ;
}
```

#### 8.1.4. 引入异常

上文介绍的所有错误报告和传播技术都有一个共同缺点：明确是程序员的责任。许多语言（Ada、Modula、ML、Java、C++、Python 等）使用一种称为 *异常* 的不同机制。异常用于中断计算，以考虑错误或异常配置。当运行中的程序遇到此类情况时，它会引发异常以警告不会提供有效结果。如果程序未能处理异常，则其执行将立即停止。

考虑异常包括在程序中显式预见此类异常会发生的情况，并编写处理该情况的算法。这称为由 *异常处理器* *捕获异常*。在程序的某点引发异常会将控制权转移到程序调用堆栈中该异常的最近处理器，如示例 8.2 所述。

#### 示例 8.2.-

```
exception NullMass
accel (force, mass)
 if mass = 0 then raise NullMass
 return (force / mass)
f ()
 ...
 try {
  v = accel (...)
  ...
 }
 with x {
  if x = NullMass then error ("Error, mass is null.")
  else raise x
 }
```

在示例 8.2 中，raise 是引发异常的构造。如果引发，accel 的执行将被中断。异常将通过调用链传播

导致 accel 的函数调用，直到找到专用处理程序。直接调用函数 f 中有一个处理程序（try ... with 构造），它捕获异常并执行规定的处理。如果 f 中没有针对 NullMass 的处理程序，异常将传播到调用 f 的函数，依此类推，直到找到处理程序，可能一直到程序的根函数，同时中断它，从而终止整个程序的执行。函数 f 中的处理程序捕获可能的异常，并将其绑定到变量 x。如果捕获的异常不是预期的异常，程序会显式地再次抛出它以确保其被检测到。正如我们将看到的，许多语言会自动处理异常的抛出，如果它不是处理程序预期的情况之一。我们在此选择了一种简化的处理程序构造，而不失一般性，以便于阐述。

异常提供了一种在软件中处理错误的简单且灵活的方式。如果引发异常的调用没有显式处理该异常，它会自动在函数调用图中向上游传播，直到找到一个“知道”如何处理该异常的函数。如果没有针对此异常的处理程序，则程序执行会被中断，从而突出显示算法中未预见的配置的存在。我们可以看到，这种机制与以前用于以传统方式报告错误的机制有根本区别，在传统方式中，函数返回的“不可能值”必须手动编码和传播。

异常可以是不同种类的“实体”，具体取决于语言。尽管如此，抛出和捕获异常这两个函数始终存在。

### 8.2. 简单形式化：ML 风格的异常

在本节中，我们将对函数式语言 Exp2 的语法、执行语义和异常类型检查进行形式化介绍。此介绍基于 ML 语言家族使用的模型。我们故意仅考虑常量异常以简化介绍。我们将在 8.3 节中简要讨论某些其他语言中使用的方法，突出这些方法与此处介绍的模型之间的主要区别。这些区别不影响异常的基本原理。

#### 8.2.1. 抽象语法

让我们扩展 3.1.1 节中给出的 Exp2 的抽象语法。表 8.1 展示了添加到表 3.1 中规则的三条额外规则，用于定义与异常相关的构造：

- 1) 按其名称 Exc 声明常量异常；
- 2) 引发异常表达式 \( e \)；
- 3) 捕获异常，将识别出的异常绑定到变量 \( x \)。

| e ::= ... 先前的表达式 | |
| --- | --- |
| exception Exc in e | 异常声明 |
| raise e | 异常引发 |
| try e1 with x -> e2 | 异常捕获 |

**表 8.1.** *将* **Exp**<sub>2</sub> *的语法扩展以包含异常*

执行 try/with 构造时，如果 \( e_1 \) 的求值引发异常 \( Exc \)，则执行结果为 \( e_2 \) 的求值，其中 \( x \) 绑定到 \( Exc \)。

#### 8.2.2. 值

在 **Exp**<sub>2</sub> 中，异常是一个常量值，就像 \( \mathbb{Z} \) 中的整数、\( \mathbb{B} \) 中的布尔值或 \( \mathbb{F} \) 中的闭包一样。异常是值这一事实意味着它们可以是任意表达式求值的结果，在—可能—被引发之前。因此，我们必须区分结果是异常值和结果是显式引发的异常。在第一种情况下，不报告错误；结果只是一个允许后来错误通知的值。在第二种情况下，结果是一个实际且有特征的错误。

在 3.2.2 节中，“错误结果”（由 **E** 表示）是“高亮”错误而不是值。在现有值之外，我们必须引入未引发异常的值。设 **E** 为异常值集合，\(\hat{\mathbb{E}}\) 为引发异常集合。因此，我们语言中的值集现在定义为 \(\mathbf{V} = \mathbb{Z} \cup \mathbb{B} \cup \mathbb{F} \cup \mathbb{E}\)。包含表达式求值的所有可能结果（即值或引发异常）的集合现在定义为 \(\mathcal{R} = \mathbf{V} \cup \hat{\mathbb{E}}\)。

#### 8.2.3. 类型代数

由于异常是值，它们必须具有类型。我们选择将此类型命名为 **exn**。它扩展了表 5.1 中介绍的类型代数。

#### 8.2.4. 操作语义

现在，让我们扩展框 3.3 中介绍的按值调用的大步操作语义规则。直觉上，求值结果现在可以由以下和类型表示：

```
type ’a result = Raised of exn | Value of ’a
```

要验证结果是否属于“错误”或“值”集合，只需查看结果构造函数即可。

声明异常时，其存在通过将名称绑定到一个值来记录在环境中。为简化起见，我们将此值视为仅仅是异常的名称。

在 ML 语言家族中，异常只是一种特定的和类型构造函数，可以在整个程序中扩展。出于此原因，异常可以使用参数进行参数化，就像任何其他和类型构造函数一样。例如，声明如下的异常：

```
exception SyntaxError of (string * int * int)
```

它可以通过指定文件名、行号以及检测到传播错误所在行的字符编号来指示语法错误的存在。

```
(E_dexc)   (Exc, EC) ⊕ Env ⊢ e ~ v
          -------------------------------
          Env ⊢ exception Exc in e ~ v

(E_raise)  Env ⊢ e ~ v
          ------------------------------- (v ∈ IE)
          Env ⊢ raise e ~ v̂

(E_try)    Env ⊢ e₁ ~ v₁
          -------------------------------
          Env ⊢ try e₁ with x → e₂ ~ v₁

(E_catch)  Env ⊢ e₁ ~ v̂₁   (x, v₁) ⊕ Env ⊢ e₂ ~ v₂
          -----------------------------------------------------------
          Env ⊢ try e₁ with x → e₂ ~ v₂
```

框 8.1. 异常的操作语义

规则 E_dexc 用于通过将异常名称绑定到环境中同名的新值来声明异常。规则 E_raise 描述错误引发表达式的求值。此求值的结果不是值，而是属于 ÎE 的引发表达式。规则 E_try 和 E_catch 用于错误捕获。规则 E_try 假设在环境 Env 中表达式 e_1 的求值不引发异常，因此不使用异常处理程序，返回值为 e_1 的值。规则 E_catch 假设在环境 Env 中表达式 e_1 的求值引发异常（v̂₁），并且该异常 v₁ 被处理程序捕获。因此，处理程序表达式 e₂ 在将 x 绑定到拦截的异常值的扩展环境 Env 中执行。

此规则中未记录首次引发异常 v̂₁ 的环境，该环境是在为求值 e₁ 而求值表达式 e 之后出现的。如 8.1.4 节所述，如果引发异常的调用未显式处理该异常，则异常机制负责将其作为结果传播给构成函数调用图的调用，直到找到处理该异常的函数。请注意，\(\widehat{v_1}\) 的这种传输由结论为该规则 \(E_{catch}\) 左前提的子树描述；该子树使用第 3 章中看到的函数应用或操作的求值规则构建。

#### 8.2.5. 类型检查

框 5.1 中介绍的类型规则现在可以通过三条新规则进行扩展，如框 8.2 所示。

\((T_{catch}) \frac{(Exc, \forall \emptyset.\mathbf{exn}) \oplus \text{Env} \vdash e : \tau}{\text{Env} \vdash \mathbf{exception}\ Exc\ \mathbf{in}\ e : \tau} \qquad (T_{raise}) \frac{\alpha\ \text{fresh}}{\text{Env} \vdash \mathbf{raise}\ e : \alpha}\)

\((T_{try}) \frac{\text{Env} \vdash e_1 : \tau \quad (x, \forall \emptyset.\mathbf{exn}) \oplus \text{Env} \vdash e_2 : \tau}{\text{Env} \vdash \mathbf{try}\ e_1\ \mathbf{with}\ x \rightarrow e_2 : \tau}\)

框 8.2. \(\mathbf{Exp}_2\) 中异常的类型规则

规则 \(T_{catch}\) 仅通过将异常名称（也是其值）绑定到所有异常的类型 exn 来扩展环境。规则 \(T_{raise}\) 用于对异常引发进行类型检查。由于此构造引发错误而不是返回值，严格来说它不具有真正的类型。该规则将其赋予由 \(\alpha\) 表示的多态类型，\(\alpha\) 是一个新鲜类型变量。此选择保留了类型算法的属性，并允许在任何类型上下文中而不受类型约束地引发异常。因此，if ... then 1 else raise Exc 是类型正确的，if ... then "ok" else raise Exc 也是。最后，规则 \(T_{try}\) 要求表达式结果的类型相同，无论是否使用异常处理程序。换句话说，无论 \(e_1\) 的求值是否以错误结束，表达式的结果必须是同一类型。

研究人员已将 ML 语言（OCaml）的类型系统扩展为包括未捕获异常的静态检测 [PES 99]。在这种情况下，类型代数更为复杂，因为它使用异常表达式进行标注，并且需要实现循环类型。为了获得更精确的分析结果，类型推断要求递归函数的标识符被泛化以对其函数体进行类型检查（参见 5.3.3 节）。关于类型项大小的启发式方法允许保持推断过程的可判定性。

### 8.3. 其他语言中的异常

在本节中，我们将简要概述不同语言中处理异常的方式。例如，在 C++、Python 和 Java 中，异常与语言的对象机制紧密相关。异常由继承自基类“异常”的类定义。此类中的对象可以被引发


并被第 8.1.4 节中介绍的类似构造捕获。允许创建从其他异常继承的异常这一选项，在处理程序捕获引发异常的能力方面引入了微妙差异。能够捕获特定异常的处理程序也能够捕获继承自此异常的所有异常。因此，处理程序中异常匹配案例的顺序对于确定哪个案例实际捕获异常具有重要意义。

#### 8.3.1. OCaml 中的异常

早前介绍的 match/with 构造是 OCaml 中使用的构造的简化版本。与只有一个捕获案例不同，可以使用任意数量的案例，如同模式匹配一样。因此，为了让处理程序捕获不同的异常，而不是编写：

示例 8.3.--

```ocaml
try e with
| x -> )
match x with
| Ex1 (v) -> ...
| Ex2 -> ...
)
```

我们可以更直接地编写：

示例 8.4.--

```ocaml
try e with
| Ex1 (v) -> ...
| Ex2 -> ...
```

#### 8.3.2. Python 中的异常

在示例 8.5 中，类 MyExc 直接继承自 Python 的 Exception 基类，并引入一个异常。

示例 8.5.--

```python
class MyExc (Exception) :
def what (self) : return ("MyExc")

class MyDerExc (MyExc) :
def what (self) : return ("MyDerExc")
```

这两个类定义引入了由开发者定义的两个异常。MyDerExc 继承自 MyExc。这两个类使用一个 what 方法，返回与它们所表示的表达式相关联的消息。

让我们继续考察示例 8.5，定义一个处理程序来捕获 Exception 的异常（在引发它们之后）。

```python
try : raise (MyExc)
except Exception as e : print (e.what ())

try : raise (MyDerExc)
except Exception as e : print (e.what ())
```

由于 MyExc 和 MyDerExc 都是 Exception，处理程序会捕获它们并显示绑定到每个异常的消息：

MyExc
MyDerExc

现在，让我们使用相同的示例定义一个两案例处理程序；第一个案例捕获 MyDerExc，第二个案例捕获 MyExc。我们引发异常 MyDerExc。

```python
try :
    raise (MyDerExc)
except MyDerExc as e :
    print ("MyDerExc", e.what ())
except MyExc as e :
    print ("MyExc", e.what ())
```

处理程序中的第一个案例能够捕获 MyDerExc；因此，使用此案例，程序显示：

MyDerExc MyDerExc

现在，让我们颠倒处理程序中案例的顺序。案例 MyExc 首先被遇到。

```python
try :
    raise (MyDerExc)
except MyExc as e :
    print ("MyExc", e.what ())
except MyDerExc as e :
    print ("MyDerExc", e.what ())
```

程序现在显示：

MyExc MyDerExc

这表明使用了处理程序中的第一个案例：这是用于捕获 MyExc 的案例，但打印的 e.what() 值是被捕获异常 MyDerExc 的名称。继承关系意味着异常 MyDerExc 可以由 MyExc 的处理程序捕获。因此，处理程序中的第二个案例是多余的，永远不会被访问。

#### 8.3.3. Java 中的异常

将示例 8.5 中的程序用 Java 编写会导致编译错误，程序被拒绝，如示例 8.6 所示。

示例 8.6.--

```java
class MyExc extends Exception {
public String what () { return ("MyExc") ; }
} ;

class MyDerExc extends MyExc {
public String what () { return ("MyDerExc") ; }
} ;

public class exn_class {
public static void main (String[] args) {
try { throw new MyExc () ; }
catch (Exception e) { e.printStackTrace () ; }
try { throw new MyDerExc () ; }
catch (Exception e) { e.printStackTrace () ; }
try { throw new MyDerExc () ; }
catch (MyDerExc e) { System.out.println (e.what ()) ; }
catch (MyExc e) { System.out.println (e.what ()) ; }
try { throw new MyDerExc () ; }
catch (MyExc e) { System.out.println (e.what ()) ; }
catch (MyDerExc e) { System.out.println (e.what ()) ; }
}
}

```

```
$ javac exn_class.java
exn_class.java:20: exception MyDerExc has already been caught
   catch (MyDerExc e) { System.out.println (e.what ()) ; }
   ^
1 error
```

编译器已将源代码最后一行中的 catch 识别为冗余，任何继承自 MyExc 的异常都会被 MyExc 的处理程序捕获。

#### 8.3.4. C++ 中的异常

在 C++ 中，使用示例 8.5，编译期间会生成一个警告，但生成的程序可以执行，并且行为与在 Python 中相同，如示例 8.7 所示。

示例 8.7.--

```cpp
#include <iostream>
#include <exception>
using namespace std ;

class MyExc : public exception {
public :
virtual const char* what () const throw () { return ("MyExc") ; }
} ;

class MyDerExc : public MyExc {
public :
virtual const char* what () const throw () { return ("MyDerExc") ; }
} ;

class MyExc exc ;
class MyDerExc dec ;

int main () {
try { throw exc ; }
catch (exception& e) { cout << e.what () << '\n' ; }
try { throw dec ; }
catch (exception& e) { cout << e.what () << '\n' ; }
try { throw dec ; }
catch (MyDerExc& e) { cout << "MyDerExc " << e.what () << '\n' ; }
catch (MyExc& e) { cout << "MyExc " << e.what () << '\n' ; }
try { throw dec ; }
catch (MyExc& e) { cout << "MyExc " << e.what () << '\n' ; }
catch (MyDerExc& e) { cout << "MyDerExc"<< e.what () << '\n' ; }
return (0) ;
}
```

```
$ g++ exn_class.cpp
exn_class.cpp:28:10: warning: exception of type 'MyDerExc &' will be caught by
  earlier handler [-Wexceptions]
   catch (MyDerExc& e)
   ~
exn_class.cpp:27:10: note: for type 'MyExc &'
 catch (MyExc& e)
   ~~
1 warning generated.
$ ./a.out
MyExc
MyDerExc
MyDerExc MyDerExc
```

C++ 还有一个特殊特性：异常不一定是类对象。任何类型的任何值都可以作为异常引发，如示例 8.8 所示。

### 示例 8.8.--

```cpp
#include <iostream>
#include <exception>
using namespace std ;

struct pair_t { int left ; int right ; } ;

int main () {
try { throw 42 ; }
catch (int e) { cout << e << '\n' ; }
try { throw "Catch me" ; }
catch (char const *e) { cout << e << '\n' ; }
try { throw 3.14159 ; }
catch (double e) { cout << e << '\n' ; }
try {
struct pair_t pair = { 0, 1 } ;
throw pair ;
}
catch (struct pair_t e) { cout << e.left << ", " << e.right << '\n' ; }
return (0) ;
}
```

```
$ ./a.out
42
Catch me
3.14159
0, 1
```

第一个处理程序捕获一个整数异常，第二个处理程序捕获一个字符串异常。重要的是要注意，异常的声明类型是 char const *，而不仅仅是 char *。由于字符串 "Catch me" 是常量，编译器会将其隐式限定为 const。如果省略此细节，处理程序将不会捕获异常。第三个处理程序涉及 float 类型的异常。再次，编译器将常量 3.14159 视为 double 类型，而不是 float。因此，异常的声明类型必须是 double，否则异常不会被捕获。最后一个处理程序显示，可以将用户数据结构构造的值（而不仅仅是基本标量值）作为异常引发。
