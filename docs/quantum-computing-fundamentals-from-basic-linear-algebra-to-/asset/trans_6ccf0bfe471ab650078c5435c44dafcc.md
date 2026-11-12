
你可能会注意到这里比本章前面讨论的随机数生成器中的更多。具体来说，有两个之前的代码中没有：

```
open Microsoft.Quantum.Intrinsic;
open Microsoft.Quantum.Arrays;
```

这些被包含进来是因为它们包含了我们在实现格罗弗算法时会用到的特定类。代码的下一部分是函数 ReflectMarked。如图 16.11 所示，它前面带有注释，这些注释旨在帮助解释函数的作用。

![](img/49061be7fe11d9f7f3e56114ab15acdc_323_0.png)

### 图 16.11 ReflectMarked

该函数接受一个我们命名为 InQubits 的 Qubit 数组。消息部分并非严格必要，但告知用户正在发生什么是个好主意。请注意我们有 X 和 H。它们是 Q# 量子开发工具包的一部分的量子门。这就是量子开发工具包如此有用的原因之一。你需要的各种门已经内置好了。

H 是 Hadamard 门，属于 Microsoft.Quantum.Intrinsic 命名空间。该命名空间中还包含其他门。你经常会看到的一些门如表 16.3 所示。

### 表 16.3 Microsoft.Quantum.Intrinsic 中的操作

| X, Y 和 Z | Pauli-X、-Y 和 -Z 门。 |
|---|---|
| T | T 门。 |
| Rx, Ry 和 Rz | 这些是绕 x、y 和 z 轴的旋转。 |
| R | 这会绕给定的 Pauli 轴应用旋转。 |
| M | 这会在 Pauli-Z 基上对单个量子比特进行测量。 |
| Measure | 在指定的 Pauli 基上对一个或多个量子比特执行联合测量。 |
| CNOT | 这会应用一个受控（非）门。 |

我们还看到一个新内容——ApplyToEach 操作。它是 Microsoft.Quantum.Canon 命名空间的一部分。它将单个量子比特操作应用于给定寄存器中的每个元素。

然后我们继续看代码中的下一个函数 ReflectUniform，如图 16.12 所示。

```
// Reflects about the superposition state.
operation ReflectUniform(InQubits : Qubit[]) : Unit {
    within {
        // Transform the uniform superposition to all-zero.
        Adjoint PermuteUniformZeros(InQubits);
        // Transform the all-zero state to all-ones
        SetAllOnes(InQubits);
    } apply {
        // Now that we've transformed the uniform superposition to the
        // all-ones state, reflect about the all-ones state, then leave
        // the within/apply block transform us back.
        ReflectAboutAllOnes(InQubits);
    }
}
```

### 图 16.12 ReflectUniform

你可以看到这调用了两个函数：SetAllOnes 和 ReflectAboutAllOnes。这两个函数都传入了一个数组 InQubits。我们还看到了一个名为 Adjoint 的东西。这是一个函子。函子（而非函数）是 Q# 特有的。引用 Microsoft 的话：

> “函子是允许访问可调用对象特定特化实现的工厂。Q# 目前支持两种函子：Adjoint 和 Controlled 函子，它们都可以应用于提供必要特化的操作。”²

Adjoint 函子定义了量子态的酉变换。这通常表示为 U（酉变换）。

在搜索数据的准备过程中还有三个函数，我们在之前的代码片段中见过它们的调用。这些函数如图 16.13 所示。

```
// Reflects about the all-ones state.
operation ReflectAboutAllOnes(InQubits : Qubit[]) : Unit {
    Controlled Z(Most(InQubits), Tail(InQubits));
}

operation PermuteUniformZeros(InQubits : Qubit[]) : Unit is Adj + Ctl {
    ApplyToEachCA(H, InQubits);
}

operation SetAllOnes(InQubits : Qubit[]) : Unit is Adj + Ctl {
    ApplyToEachCA(X, InQubits);
}
```

### 图 16.13 格罗弗算法的其他函数

请注意，ReflectAboutAllOnes 只是使用受控 Z 门。PrepareUniformZeros 和 SetAllOnes 都使用函数 ApplyToEach。请回顾，此函数将单个量子比特操作应用于给定寄存器中的每个元素。

现在我们有了实际搜索输入的函数。如图 16.14 所示。

```
@EntryPoint()
operation SearchForMarkedInput(nQubits : Int) : Result[] {
    using (qubits = Qubit[nQubits]) {
        // Initialize a uniform superposition over all possible inputs.
        PrepareUniformSuperposition(qubits);
        // The search itself consists of repeatedly reflecting about the
        // marked state and our start state, which we can write out in Q#
        // as a for loop.
        for (idxIteration in 0..NumberOfIterations(nQubits) - 1) {
            ReflectMarked(qubits);
            ReflectUniform(qubits);
        }

        // Measure and return the answer.
        return ForEach(MResetZ, qubits);
    }
}
```

### 图 16.14 格罗弗算法的入口点

请注意这是程序的入口点，它应该帮助你演示其他函数的用法。首先，我们初始化一个均匀叠加态，然后通过反射操作进行搜索。最后，我们测量每个量子比特。

还有一个函数会简单地告诉你找到单个标记项需要多少次迭代。此函数在 NumberOfIterations 中被调用，如图 16.15 所示。

```
function NumberOfIterations(nQubits : Int) : Int {
    let items = 1 <<< nQubits; // 2^numQubits
    // compute number of iterations:
    let angle = ArcSin(1. / Sqrt(IntAsDouble(items)));
    let iterations = Round(0.25 * PI() / angle - 0.5);
    return iterations;
}
```

### 图 16.15 NumberOfIterations 函数

与随机数生成器一样，建议你实际编写此代码并使其运行。运行后，尝试不同的变体。修改少量代码并观察结果。这将帮助你更好地理解代码。

## 16.4 Deutsch-Jozsa 算法

这是第 10 章中的另一个算法。我们将在此简要回顾它，然后讲解实现该算法的代码。

### 16.4.1 回顾 Deutsch-Jozsa 算法

回顾第 10 章，这是 Deutsch 算法的一个修改。问题其实相当直接。Deutsch 算法研究一元函数。Deutsch-Jozsa 算法将其推广到 n 元函数。回顾此问题涉及一个黑箱量子计算机，它实现某个函数，该函数接受 n 位二进制值，并为每个这样的值输出 0 或 1。输出要么是平衡的（1 和 0 的数量相等），要么是常数的（全 1 或全 0）。算法的目标是确定函数是常数还是平衡。

实际算法从前 n 位处于 |0> 态开始，最后一位处于 |1> 态。然后对每一位应用 Hadamard 变换。现在我们有一个将称作 f(x) 的函数。该函数将态 |x>|y> 映射到 |x>|y ⊕ f(x)>。在这里，符号 ⊕ 表示模 2 加法。当 x 的值通过神谕 f(x) 处理时，会产生 1 或 0。如果 f(x) 是平衡的，输出为 0；如果 f(x) 是常数的，输出为 1。对于 n 位输入，如果 f(x) 是常数，最终输出将是 n 个 0。任何其他输出（0 和 1 的任何组合，或全 1）都表示 f(x) 是平衡的。

### 16.4.2 Deutsch-Jozsa 算法的代码

我们从命名空间的打开以及其他命名空间的 open 语句开始。如图 16.16 所示。

```
namespace DeutschJozsa {
    open Microsoft.Quantum.Intrinsic;
    open Microsoft.Quantum.Canon;
    open Microsoft.Quantum.Arrays;
    open Microsoft.Quantum.Convert;
    open Microsoft.Quantum.Measurement;
}
```

### 图 16.16 Deutsch-Jozsa 的开头

让我们从入口点开始，如图 16.17 所示。

```
[@EntryPoint()]
operation RunDeutschJozsa(nQubits : Int, mElements : Int[]) : Bool {
    return IsConstant(
        BooleanFunction(nQubits, mElements), nQubits
    );
}
```

### 图 16.17 Deutsch-Jozsa 入口点
