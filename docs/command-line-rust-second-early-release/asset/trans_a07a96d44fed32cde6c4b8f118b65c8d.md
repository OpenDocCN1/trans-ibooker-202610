
```
assert!(parse_pos("").is_err());

let res = parse_pos("0");
assert!(res.is_err());
assert_eq!(res.unwrap_err().to_string(), "illegal list value: \"0\"");

let res = parse_pos("a");
assert!(res.is_err());
assert_eq!(res.unwrap_err().to_string(), "illegal list value: \"a\"");

let res = parse_pos("1,a");
assert!(res.is_err());
assert_eq!(res.unwrap_err().to_string(), "illegal list value: \"a\"");

let res = parse_pos("2-1");
assert!(res.is_err());
assert_eq!(
    res.unwrap_err().to_string(),
    "First number in range (2) must be lower than second number (1)"
);

let res = parse_pos("1");
assert!(res.is_ok());
assert_eq!(res.unwrap(), vec![0]);

let res = parse_pos("1,3");
assert!(res.is_ok());
assert_eq!(res.unwrap(), vec![0, 2]);

let res = parse_pos("1-3");
assert!(res.is_ok());
assert_eq!(res.unwrap(), vec![0, 1, 2]);

let res = parse_pos("1,7,3-5");
assert!(res.is_ok());
assert_eq!(res.unwrap(), vec![0, 6, 2, 3, 4]);
}
}
```

此时，我希望你能读懂上面的代码，足以理解该函数的工作方式。我建议你在此时停止阅读并编写能通过此测试的代码。

After cargo test test_parse_pos passes, your program should reject an invalid range and print an error message:

在 cargo test test_parse_pos 通过后，你的程序应拒绝无效范围并打印错误信息：

```
$ cargo run -- -f foo,bar tests/inputs/books.tsv
illegal list value: "foo"
```

It should also reject invalid ranges:

它还应拒绝无效范围：

```
$ cargo run -- -f 3-2 tests/inputs/books.tsv
First number in range (3) must be lower than second number (2)
```

When given valid arguments, your program should be able to display a structure like so:

当给出有效参数时，你的程序应能显示如下结构：

```
$ cargo run -- -f 1 -d , tests/inputs/movies1.csv
Config {
    files: [
        "tests/inputs/movies1.csv", ①
    ],
    delimiter: 44, ②
    extract: Fields( ③
        [
            0,
        ],
    ),
}
```

- ① The positional argument goes into files.
- ① 位置参数进入 files。
- ② The -d value of a comma has a byte value of 44.
- ② -d 值为逗号，其字节值为 44。
- ③ The -f value of 1 creates the Extract::Fields([0]) variant.
- ③ -f 值为 1 创建 Extract::Fields([0]) 变体。

When parsing a TSV file, use the tab as the default delimiter, which has a byte value of 9:

解析 TSV 文件时，使用制表符作为默认分隔符，其字节值为 9：

```
$ cargo run -- -f 2-3 tests/inputs/movies1.tsv
Config {
    files: [
        "tests/inputs/movies1.tsv",
    ],
    delimiter: 9,
    extract: Fields(
        [
            1,
            2,
        ],
    ),
}
```

Note that the options for -f|--fields, -b|--bytes, and -c|--chars are all mutually exclusive and should be rejected:

请注意，-f|--fields、-b|--bytes 和 -c|--chars 选项互斥，应被拒绝：

```
$ cargo run -- -f 1 -b 8-9 tests/inputs/movies1.tsv
error: The argument '--fields <FIELDS>' cannot be used with '--bytes <BYTES>'

USAGE:
    cutr <FILE>... --bytes <BYTES> --delim <DELIMITER> --
fields <FIELDS>
```

Try to get just this much of your program working before you proceed. You should be able to pass cargo test dies:

在继续之前，先尝试让程序实现到此程度。你应该能够通过 cargo test dies：

```
running 8 tests
test dies_chars_bytes ... ok
test dies_chars_bytes_fields ... ok
test dies_chars_fields ... ok
test dies_bytes_fields ... ok
test dies_not_enough_args ... ok
test dies_bad_digit_field ... ok
test dies_bad_digit_bytes ... ok
test dies_bad_digit_chars ... ok
```

## Parsing the Position List

## 解析位置列表

I assume you wrote a passing parse_pos function, so compare your version to mine:

我假设你已编写了一个通过的 parse_pos 函数，现在将其与我的版本进行比较：

```
fn parse_pos(range: &str) -> MyResult<PositionList> {
    let mut fields: Vec<usize> = vec![]; ①
    let range_re = Regex::new(r"(\d+)?-(\d+)?").unwrap();
    ②
    for val in range.split(',') { ③
        if let Some(cap) = range_re.captures(val) { ④
            let n1: &usize = &cap[1].parse()?; ⑤
            let n2: &usize = &cap[2].parse()?;

            if n1 < n2 { ⑥
                for n in *n1..=*n2 { ⑦
                    fields.push(n);
                }
            } else {
                return Err(From::from(format!( ⑧
                    "First number in range ({}) \
                    must be lower than second number ({})",
                    n1, n2
                )));
            }
        } else {
            match val.parse() { ⑨
                Ok(n) if n > 0 => fields.push(n),
                _ => {
                    return Err(From::from(format!(
                        "illegal list value: \"{}\"",
                        val
                    )))
                }
            }
        }
    }

    // Subtract one for field indexes
    Ok(fields.into_iter().map(|i| i - 1).collect()) ⑩
}
```

① Create a mutable vector to hold all the positions.
① 创建一个可变向量来保存所有位置。
- 2. Create a regular expression to capture two numbers separated by a dash.
- 2. 创建一个正则表达式来捕获用连字符分隔的两个数字。
- 3. Split the range values on a comma.
- 3. 按逗号分割范围值。
- 4. See if this part of the list matches the regex.
- 4. 查看列表的这一部分是否匹配正则表达式。
- 5. Convert the two captured numbers to `usize` integer values.
- 5. 将捕获的两个数字转换为 `usize` 整数值。
- 6. If the first number is less than the second, iterate through the range and add the values to `fields`.
- 6. 如果第一个数字小于第二个数字，则遍历范围并将值添加到 `fields`。
- 7. Use the * operator to dereference the two number values.
- 7. 使用 * 运算符解引用这两个数字值。
- 8. Return an error about an invalid range.
- 8. 返回关于无效范围的错误。
- 9. If it’s possible to convert the value to a `usize` integer, add it to the list or else throw an error.
- 9. 如果可以将值转换为 `usize` 整数，则将其添加到列表，否则抛出错误。
- 10. Return the given list with all the values adjusted down by 1.
- 10. 返回给定列表，并将所有值下调 1。

## NOTE

## 注意

In the regular expression, I use r"" to denote a raw string so that Rust won’t try to interpret the string. For instance, you’ve seen that Rust will interpret \n as a newline. Without this, the compiler would complain that \d is an unknown character escape:

在正则表达式中，我使用 r"" 表示原始字符串，这样 Rust 就不会尝试解释该字符串。例如，你已经看到 Rust 会将 \n 解释为换行符。如果没有它，编译器会抱怨 \d 是未知的字符转义：

```
error: unknown character escape: `d`
  --> src/lib.rs:155:34
  |
155 |     let range_re = Regex::new("(\d+)?-
(\d+)?").unwrap();
  |                     ^ unknown character
escape
  |
  = help: for more information, visit
```

https://static.rust-lang.org/doc/master/reference.html#literals

I would like to highlight two new pieces of syntax in the preceding code. First, I used parentheses in the regular expression `(\d+)-(\d+)` to indicate one or more digits followed by a dash followed by one or more digits as shown in Figure 8-1. If the regular expression matches the given string, then I can use the `Regex::captures` to extract the digits from the string. Note that they are available in 1-based counting, so the contents of the first capturing parentheses are available in position `1` of the captures. Because the captured values matched digit characters, they should be parsable as `usize` values.

我希望强调前述代码中的两个新语法。首先，我在正则表达式 `(\d+)-(\d+)` 中使用了括号，表示一个或多个数字，后跟连字符，再跟一个或多个数字，如图 8-1 所示。如果正则表达式匹配给定字符串，则我可以使用 `Regex::captures` 从字符串中提取数字。请注意它们采用 1 起始计数，因此第一个捕获括号的内容可在 captures 的位置 `1` 中获取。因为捕获的值匹配数字字符，所以它们应可解析为 `usize` 值。

![](img/fbe8ef8a7ecb95c985397f6370ec2c66_264_0.png)

Figure 8-1. The parentheses in the regular expression will capture the values they surround

Figure 8-1. 正则表达式中的括号将捕获它们所包围的值

The second piece of syntax is the * operator in for n in *n1..=*n2. If you remove these and try to compile the code, you will see the following error:

第二个语法是 for n in *n1..=*n2 中的 * 运算符。如果你移除它们并尝试编译代码，你会看到以下错误：

```
error[E0277]: the trait bound `&usize: Step` is not
satisfied
  --> src/lib.rs:165:34
165 |                     for n in n1..=n2 {
  |                               ^^^^^^ the trait
```

`Step` is not
|
for `&usize`                     implemented

This is one case where the compiler’s message is a bit cryptic and does not include the solution. The problem is that n1 and n2 are &usize references. A reference is a pointer to a piece of memory, not a copy of the value, and so the pointer must be dereferenced to use the underlying value. There are many times when Rust silently dereferences values, but this is one time when the * operator is required.

这是一个编译器消息有点晦涩且不包含解决方案的情况。问题在于 n1 和 n2 是 &usize 引用。引用是指向内存块的指针，而不是值的副本，因此必须解引用指针才能使用底层值。Rust 在很多情况下会静默解引用值，但这是一个需要 * 运算符的情况。

Here is how I incorporate these ideas into my get_args. First, I define all the arguments:

以下是我在 get_args 中如何结合这些思想。首先，我定义所有参数：

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("cutr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust cut")
        .arg(
            Arg::with_name("files") ①
                .value_name("FILE")
                .help("Input file(s)")
                .required(true)
                .default_value("-")
                .min_values(1),
        )
        .arg(
            Arg::with_name("delimiter") ②
                .value_name("DELIMITER")
                .help("Field delimiter")
                .short("d")
                .long("delim")
                .default_value("\\t"),
        )
        .arg(
            Arg::with_name("fields") ③
                .value_name("FIELDS")
                .help("Selected fields")
                .short("f")
                .long("fields")
```
