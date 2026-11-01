

| 35: # | 54: 6 | 73: I | 92: \ | 111: o |
|---|---|---|---|---|
| 36: $ | 55: 7 | 74: J | 93: ] | 112: p |
| 37: % | 56: 8 | 75: K | 94: ^ | 113: q |
| 38: & | 57: 9 | 76: L | 95: _ | 114: r |
| 39: ' | 58: : | 77: M | 96: ` | 115: s |
| 40: ( | 59: ; | 78: N | 97: a | 116: t |
| 41: ) | 60: < | 79: O | 98: b | 117: u |
| 42: * | 61: = | 80: P | 99: c | 118: v |
| 43: + | 62: > | 81: Q | 100: d | 119: w |
| 44: , | 63: ? | 82: R | 101: e | 120: x |
| 45: - | 64: @ | 83: S | 102: f | 121: y |
| 46: . | 65: A | 84: T | 103: g | 122: z |
| 47: / | 66: B | 85: U | 104: h | 123: { |
| 48: 0 | 67: C | 86: V | 105: i | 124: \| |
| 49: 1 | 68: D | 87: W | 106: j | 125: } |
| 50: 2 | 69: E | 88: X | 107: k | 126: ~ |
| 51: 3 | 70: F | 89: Y | 108: l | 127: DEL |

为了模仿 BSD comm，我应该先打印较小的值（B），然后从该文件再取一个值用于下一次迭代。GNU 版本则相反。注意你应该为此代码在导入中添加 use std::cmp::Ordering::*：

```
let mut line1 = lines1.next();
let mut line2 = lines2.next();

loop {
    match (&line1, &line2) {
        (Some(val1), Some(val2)) => match
val1.cmp(val2) { ①
            Equal => { ②
                println!("{}", val1);
                line1 = lines1.next();
                line2 = lines2.next();
            }
            Less => { ③
                println!("{}", val1);
                line1 = lines1.next();
            }
            _ => { ④
                println!("{}", val2);
                line2 = lines2.next();
            }
        }
        (Some(val1), None) => {
            println!("{}", val1); ⑤
            line1 = lines1.next();
        }
        (None, Some(val2)) => {
            println!("{}", val2); ⑥
            line2 = lines2.next();
        }
        (None, None) => break,
    };
}
```

- 1. 使用 `std::cmp` 比较第一个值和第二个值。这将返回 `std::cmp::Ordering` 的一个变体。
- 2. 当两个值相等时，打印第一个值并从每个文件中获取值。
- 3. 当第一个文件的值小于第二个文件的值时，打印第一个值并请求第一个文件的下一个值。
- 4. 否则，打印第二个文件的值并请求第二个文件的下一个值。
- 5. 当只有第一个文件的值时，打印它并继续请求第一个文件的值。
- 6. 当只有第二个文件的值时，打印它并继续请求第二个文件的值。

如果我使用一个非空文件和一个空文件运行此代码，它可以正常工作：

```
$ cargo run -- tests/inputs/file1.txt
tests/inputs/empty.txt
a
b
c
d
```

如果我使用 *file1.txt* 和 *file2.txt*，结果与预期输出已经很接近了：

```
$ cargo run -- tests/inputs/file1.txt
tests/inputs/file2.txt
B
a
b
c
d
```

我决定创建一个名为 `Column` 的枚举来表示我应该在哪个列中打印值。每个变体都包含一个 `String`，你可以将以下内容放在 `src/lib.rs` 的顶部，靠近你的 `Config` 声明处。确保在导入中添加 `use crate::Column::*`，这样你就可以引用 `Col1` 而不是 `Column::Col1`：

```
enum Column {
    Col1(String),
    Col2(String),
    Col3(String),
}
```

接下来，我选择创建一个闭包来处理输出的打印。这也意味着我必须根据配置确定前两列的内容：

```
let default_col1 = if config.suppress_col1 {
    ""
} else {
    &config.delimiter
};

let default_col2 = if config.suppress_col2 {
    ""
} else {
    &config.delimiter
};
```

```
let printer = |col: Column| {
    let out = match col {
        Col1(val) => { ③
            if config.suppress_col1 {
                "".to_string()
            } else {
                val
            }
        }
        Col2(val) => format!( ④
            "{}{}",
            default_col1,
            if config.suppress_col2 { "" } else { &val }
        ),
        Col3(val) => format!( ⑤
            "{}{}{}",
            default_col1,
            default_col2,
            if config.suppress_col3 { "" } else { &val }
        ),
    };
    if !out.trim().is_empty() { ⑥
        println!("{}", out);
    }
};
```

- 1. 抑制列 1 时，使用空字符串；否则使用输出分隔符。
- 2. 对列 2 做同样的处理。
- 3. 给定列 1 的文本，决定是否打印或抑制输出。
- 4. 给定列 2 的文本，使用列 1 的默认值，并选择打印或抑制该列。
- 5 给定列 3 的文本，使用列 1 和列 2 的默认值，并选择打印或抑制该列。
- 6 仅打印非空的输出行。

我可以这样使用这个想法：

```
let mut line1 = lines1.next(); ①
let mut line2 = lines2.next();

loop {
    match (&line1, &line2) {
        (Some(val1), Some(val2)) => match
val1.cmp(val2) {
            Equal => {
                printer(Col3(val1.to_string())); ②
                line1 = lines1.next();
                line2 = lines2.next();
            }
            Less => {
                printer(Col1(val1.to_string())); ③
                line1 = lines1.next();
            }
            _ => {
                printer(Col2(val2.to_string())); ④
                line2 = lines2.next();
            }
        },
        (Some(val1), None) => {
            printer(Col1(val1.to_string())); ⑤
            line1 = lines1.next();
        }
        (None, Some(val2)) => {
            printer(Col2(val2.to_string())); ⑥
            line2 = lines2.next();
        }
        (None, None) => break,
    };
}
```

① 从两个输入文件读取初始值。

- 2. 当值相同时，在列 3 中打印其中一个。
- 3. 当第一个值小于第二个值时，在列 1 中打印第一个值。
- 4. 否则，在列 2 中打印第二个值。
- 5. 当只有第一个文件的值时，在列 1 中打印它。
- 6. 当只有第二个文件的值时，在列 2 中打印它。

我喜欢有将输出分隔符从制表符更改为更明显的东西的选项：

```
$ cargo run -- -d="-->" tests/inputs/file1.txt
tests/inputs/file2.txt
--->B
a
b
--->--->c
d
```

经过这些更改，所有测试都通过了。

## 更进一步

- 修改程序以显示 GNU 输出并添加这些选项。
- 如我在章节介绍中所指出的，`comm` 对两个文件执行基本的连接操作，这与 `join` 程序类似。阅读该程序的手册页，并利用你编写 `commr` 的经验来编写 `joinr`。

## 总结

正如我所说，`comm` 对我来说一直是黑魔法，我每次都要查阅手册页来记住标志的含义。现在我写了自己的版本，我理解得好多了，而且非常喜欢它工作原理的优雅之处。考虑一下你学到了什么：

- 你可以通过在 `BufRead::lines` 上调用 `Iterator::next` 手动迭代文件句柄的行。
- 可以通过将可能性组合成元组来进行 `match`。
- 你可以使用 `std::cmp` 比较一个值和另一个值。结果是 `std::cmp::Ordering` 的一个变体。
- 你创建一个名为 `Column` 的 `enum`，其中变体可以保存字符串值，如 `Col1(String)`，这非常方便。

## 第 11 章. 尾声

> *世界上没有人能得到他们想要的，这很美。*
> *每个人都带着挫败和悲伤死去，这很美。*
> —They Might Be Giants

结束。

## 关于作者

Ken Youens-Clark 担任专业软件开发人员已有 25 年。起初他在北德克萨斯大学本科学习爵士研究（鼓），在多次更换专业后，他以英语文学学士学位勉强毕业。Ken 从 1990 年代中期开始在工作中学习编程，多年来在工业、研究和非营利组织工作。2019 年，他获得了亚利桑那大学生物系统工程的理学硕士学位。他此前出版了两本书，《Tiny Python Projects》（Manning，2020）和《Mastering Python for Bioinformatics》（O’Reilly，2021）。他与妻子、三个孩子和一只狗居住在美国亚利桑那州图森市。
