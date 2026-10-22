

```
let instance = Instance::new(&mut store, &module, &
[callback_func.into()])?;

    let myfunc =
        instance.get_typed_func::<(i32,i32), (i32, i32), _>(&mut store,
"myfunc")?;
    let (a, b) = myfunc.call(&mut store, (13, 43))?;

    println!("Swapping {} and {} produces {} and {}.", 13, 43, a, b);

    Ok(())
}
```

新的地方在于我们通过调用 `Func::wrap` 来定义一个回调函数。这会接收我们 `Store` 实例的可变引用以及一个 Rust 闭包，该闭包接收两个 `i32` 参数并返回一个包含两个 `i32` 参数的元组。我们使用符合惯用的 Rust 方式来表达这一功能，这非常酷！闭包的实现相当简单。我们只是返回一个参数顺序相反的元组。

现在我们有了回调，注意我们在创建模块 `Instance` 时将其传递给导入上下文。之后我们获取从 WebAssembly 模块导出的 `myfunc` 函数的包装器，并使用一个元组值调用它。这同样是符合惯用的 Rust 写法，也是传递两个参数非常自然的方式。这些参数会在幕后解构为我们的函数所期望的两个参数。调用导出函数的結果被捕获为一个元组，我们随后将其解构并打印出结果。

```
brian@tweezer ~/g/w/s/c/hello-mvr> cargo build --release
    Compiling hello-mvr v0.1.0 (/Users/brian/git-
personal/webassembly-the-definitive-guide/src/ch12/hello-mvr)
    Finished release [optimized] target(s) in 3.17s
brian@tweezer ~/g/w/s/c/hello-mvr> cargo run --release
    Finished release [optimized] target(s) in 0.22s
    Running `target/release/hello-mvr`
Swapping 13 and 43 produces 43 and 13.
```

## Reference Types

能够指定多个返回值是某些额外提案的必要前提。另一个使能提案是能够指定对不透明句柄的引用。这对于添加垃圾回收、具有类型化的引用、异常处理等至关重要，但它对于主机环境能够传入指向资源的不可见引用的能力也是关键，这些资源我们不希望 WebAssembly 模块必然拥有原始访问权限。请记住，我们谈论的是能够在任意操作系统上的任意语言中的任意结构之间传递任意引用。试图在如此灵活的同时保持性能绝非易事！

我们之前能够创建引用，但仅限于函数，并且仅限于一旦创建后就无法操作的 `Table` 实例。这在一定程度上是为了禁止模块操作内存中的敏感细节或更改哪个函数位于哪个槽位。回想第 7 章，我们必须对函数引用进行间接调用，而不是更常见的对其他函数的直接调用。

这个新提案⁴ 使我们能够操作 `Table` 成员、增长 `Table` 实例大小，并能够在 WebAssembly 模块与其主机环境之间传递 `externref` 引用。

Wasmtime 支持创建 `externref` 引用，因此让我们使用其 API 创建另一个示例应用程序。需要明确的是，任何 WASI 环境都必须支持这些基本提案，但我们关注的是 Wasmtime。

```
brian@tweezer ~/g/w/s/ch12> cargo new --bin hello-extref
    Created binary (application) `hello-extref` package
```

这只是对我们 Wat 文件语法的一个相对简单的更改。快速查看示例 12-7 你会发现我们可以将 `externref` 元素存储在 `Table` 中，或作为参数传入或作为函数结果返回。这是 Wasmtime 关于这些引用类型的示例的简化版本。我在这里不重点关注它们，但也可以对 `externref` 元素创建全局变量引用。

### Example 12-7. A Wat file with externref parameters, Table elements and results.

```
(module
  (table $table (export "table") 10 externref)

  (func (export "func") (param externref) (result externref)
    local.get 0
  )
)
```

我们的模块导出一个可容纳十个引用的 `Table`。我们还有一个函数，它只是返回其参数。不幸的是，示例 12-8 中的 Rust 代码比我们之前看过的其他示例复杂得多，因为我们在包装这些引用、克隆它们、解引用它们并按需提取包装的数据。我会带你逐步了解，以便我们能专注于新内容。

你会看到的第一件事是引入了一个 `Config` 实例。我们之前一直在使用默认配置来创建 `Engine` 实例，但我们需要打开对外部引用的支持，然后相应地配置我们的 `Engine`。从那以后，从设置角度来看应该看起来很熟悉。

为了展示我们对引用的灵活性，我们从 Wasmtime API 创建了两个 `ExternRef` 结构实例。其中一个可能是不可伪造的持票令牌或在模块进行共享函数调用时可以传回的东西。为我们的目的，我只使用一个字符串切片，内容为“secret key”。另一个引用指向一个字节数组。由于 `ExternRef` 结构是一个参数化类型，它能够包装这两种数据类型。

在创建引用之后，我们从模块中检索导出的 `Table` 并将我们引用的克隆存储在槽 3 和 4 中。我们使用 `Some` 包装器，这样更容易判断引用是否存在。如果不存在，我们可能会得到一个 `None` 实例。这是通过 Rust 中的一个名为 `Option` 的结构⁵ 实现的。由于表存储 `externref` 元素，模块无法查看其细节。然而，当我们取回它们时，它们仍然代表我们放入其中的结构。其他编程语言将根据静态类型和内存管理细节有不同的机制来实现此过程，但看起来基本相同。

### Example 12-8. A Wasmtime Rust application that uses externref elements

```
use std::error::Error;
use wasmtime::*;

fn main() -> Result<(), Box<dyn Error>> {
    let mut config = Config::new();
    config.wasm_reference_types(true);

    let engine = Engine::new(&config)?;
    let mut store = Store::new(&engine, ());
    let module = Module::from_file(&engine, "extref.wat")?;

    let instance = Instance::new(&mut store, &module, &[])?;

    let eref = ExternRef::new("secret key");
    let arr : [u8; 4] = [1, 2, 3, 4];

    let eref2 = ExternRef::new(arr);

    let table = instance.get_table(&mut store, "table").unwrap();
    table.set(&mut store, 3, Some(eref.clone()).into())?;
    table.set(&mut store, 4, Some(eref2.clone()).into())?;

    let ret = table.get(&mut store, 3)
        .unwrap()
        .unwrap_externref()
        .unwrap();

    let ret2 = table.get(&mut store, 4)
        .unwrap()
        .unwrap_externref()
        .unwrap();

    let str = *ret.data().downcast_ref::<&'static str>().unwrap();
    let arr2 = *ret2.data().downcast_ref::<[u8; 4]>().unwrap();
```

```
println!("Retrieved external reference: {} from table slot {}", str, 3);
println!("Retrieved external reference: {:?} from table slot {}", arr2, 4);

let func = instance.get_typed_func::<Option<ExternRef>, Option<ExternRef>, _>(
(&mut store, "func")?;

let ret = func.call(&mut store, Some(eref.clone()))?;

let str2 = *(ret.unwrap()).data().downcast_ref::<&'static str>()
.unwrap();

println!("Received {} back from calling extern-ref aware function.", str2);

Ok(())
}
```

提取引用看起来有点奇怪，但第一个 `unwrap()` 函数确定我们是否在 `Table` 实例的边界内进行了索引引用。它然后提取 `externref` 并验证它是否为该类型实例（而不是其他引用类型）。最后的 `unwrap()` 确保它没有产生空引用，这是该提案为 WebAssembly 平台带来的另一个补充。

接下来的步骤可能更奇怪一些，但我们正在解引用我们的引用，提取数据并将其强制转换为我们期望的类型，在本例中是一个具有静态生命周期的字符串切片（也称为字符串字面量）和一个包含四个元素的 `u8` 数组。假设这些向下转换成功，我们打印出从模块实例中取回的元素值。

我们的最后一步获取对模块导出函数的引用，该函数接收一个 `Option` 包装的 `ExternRef` 并返回一个 `Option` 包装的 `ExternRef`。回想示例 12-7，我们的函数只是返回其参数。我们使用我们 `ExternRef` 的克隆副本调用我们的函数，

