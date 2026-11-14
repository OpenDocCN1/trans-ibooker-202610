# 使用 Go 进行数据编码

本章将向您展示如何在应用程序中使用更常见的编码来交换数据。编码是转换数据的过程，当一个应用程序必须与另一个应用程序通信时可以使用它——使用相同的编码将使这两个程序能够相互理解。本章将首先解释如何处理基于文本的协议，如 `JSON`，然后介绍如何使用二进制协议，如 `gob`。

本章将涵盖以下主题：

*   使用基于文本的编码，如 `JSON` 和 `XML`
*   了解二进制编码，如 `gob` 和 `protobuf`

## 技术要求

本章要求安装 Go 并设置好您喜欢的编辑器。更多信息请参阅 第 3 章 *Go 概览*。

为了使用协议缓冲区，您需要安装 `protobuf` 库。说明请见 <https://github.com/golang/protobuf>。

## 理解基于文本的编码

最易于人类阅读的数据序列化格式是基于文本的格式。在本节中，我们将分析一些最常用的基于文本的编码——`CSV`、`JSON`、`XML` 和 `YAML`。

### CSV

**逗号分隔值** (**CSV**) 是一种以表格形式存储数据的文本编码类型。每一行是一个表条目，一行中的值由一个特殊字符分隔，通常是逗号——因此得名 CSV。CSV 文件的每条记录必须具有相同的值数量，第一条记录可用作标题来描述每个记录字段：

```
name,age,country
```

字符串值可以用引号括起来，以允许使用逗号。

### 解码值

Go 允许用户从任何 `io.Reader` 创建 `CSV` 读取器。可以使用 `Read` 方法逐条读取记录：

```go
func main() {
    r := csv.NewReader(strings.NewReader("a,b,c\ne,f,g\n1,2,3"))
    for {
        r, err := r.Read()
        if err != nil {
            log.Fatal(err)
        }
        log.Println(r)
    }
}
```

上述代码的完整示例请见 <https://play.golang.org/p/wZgVzMqAN_K>。

请注意，每条记录都是一个字符串切片，读取器期望每行的长度保持一致。如果某行的条目比第一行多或少，将会导致错误。也可以使用 `ReadAll` 一次性读取所有记录。使用该方法的前一个示例将如下所示：

```go
func main() {
    r := csv.NewReader(strings.NewReader("a,b,c\ne,f,g\n1,2,3"))
    records, err := r.ReadAll()
    if err != nil {
        log.Fatal(err)
    }
    for _, r := range records {
        log.Println(r)
    }
}
```

上述代码的完整示例请见 <https://play.golang.org/p/RJ-wxBB5fs6>。

### 编码值

可以使用任何 `io.Writer` 创建 `CSV` 写入器。生成的写入器将是缓冲的，因此为了不丢失数据，有必要调用其 `Flush` 方法：这将确保缓冲区被清空，所有内容都写入写入器。

`Write` 方法接收一个字符串切片并将其编码为 `CSV` 格式。让我们在下面的示例中看看它是如何工作的：



# Go 语言中的 CSV 和 JSON

## CSV 写入

```
func main() {
    const million = 1000000
    type Country struct {
        Code, Name string
        Population int
    }
    records := []Country{
        {Code: "IT", Name: "Italy", Population: 60 * million},
        {Code: "ES", Name: "Spain", Population: 46 * million},
        {Code: "JP", Name: "Japan", Population: 126 * million},
        {Code: "US", Name: "United States of America", Population: 327 * million},
    }
    w := csv.NewWriter(os.Stdout)
    defer w.Flush()
    for _, r := range records {
        if err := w.Write([]string{r.Code, r.Name, strconv.Itoa(r.Population)}); err != nil {
            fmt.Println("error:", err)
            os.Exit(1)
        }
    }
}
```

上述代码的完整示例可在 [`play.golang.org/p/qwaz3xCJhQT`](https://play.golang.org/p/qwaz3xCJhQT) 查看。

碰巧的是，对于读取器，有一种方法可以一次写入多条记录。它被称为 `WriteAll`，我们可以在下一个示例中看到它：

```
func main() {
    const million = 1000000
    type Country struct {
        Code, Name string
        Population int
    }
    records := []Country{
        {Code: "IT", Name: "Italy", Population: 60 * million},
        {Code: "ES", Name: "Spain", Population: 46 * million},
        {Code: "JP", Name: "Japan", Population: 126 * million},
        {Code: "US", Name: "United States of America", Population: 327 * million},
    }
    w := csv.NewWriter(os.Stdout)
    defer w.Flush()
    var ss = make([][]string, 0, len(records))
    for _, r := range records {
        ss = append(ss, []string{r.Code, r.Name, strconv.Itoa(r.Population)})
    }
    if err := w.WriteAll(ss); err != nil {
        fmt.Println("error:", err)
        os.Exit(1)
    }
}
```

上述代码的完整示例可在 [`play.golang.org/p/lt_GBOLvUfk`](https://play.golang.org/p/lt_GBOLvUfk) 查看。

`Write` 和 `WriteAll` 的主要区别在于，第二种操作使用更多资源，并且要求我们在调用它之前将记录转换为字符串切片的切片。

## 自定义选项

读取器和写入器都有一些创建后可更改的选项。这两个结构体共享 `Comma` 字段，即用于分隔字段的字符。另一个仅属于写入器的重要字段是 `FieldsPerRecord`，它是一个整数，决定读取器每条记录应期望的字段数量：

* 如果大于 `0`，它将是所需的字段数量。
* 如果等于 `0`，它将被设置为第一条记录的字段数量。
* 如果为负数，将跳过所有字段数量检查，允许读取不一致的记录集。

让我们看一个不检查一致性且使用空格作为分隔符的读取器的实际示例：

```
func main() {
    r := csv.NewReader(strings.NewReader("a b\ne f g\n1"))
    r.Comma = ' '
    r.FieldsPerRecord = -1
    records, err := r.ReadAll()
    if err != nil {
        log.Fatal(err)
    }
    for _, r := range records {
        log.Println(r)
    }
}
```

上述代码的完整示例可在 [`play.golang.org/p/KPHXRW5OxXT`](https://play.golang.org/p/KPHXRW5OxXT) 查看。

## JSON

JavaScript 对象表示法（`JSON`）是一种轻量级的、基于文本的数据交换格式。其特性使人类能轻松读写，且开销小，非常适合基于 Web 的应用程序。

构成 `JSON` 的实体主要有两种类型：

* 名称/值对的集合：在各种编程语言中，名称/值对表示为对象、结构体或字典。
* 值的有序列表：这些是集合或值的列表，通常表示为数组或列表。

对象用大括号括起来，每个键由冒号分隔，每个值与下一个键/值对由逗号分隔。列表用方括号括起来，元素由逗号分隔。这两种类型可以组合，因此列表也可以是一个值，对象也可以是列表中的元素。名称和值之外的空格、换行符和制表符会被忽略，它们用于缩进数据以使其更易读。

以此 `JSON` 对象为例：

```json
{
    "name: "Randolph",
    "surname": "Carter",
    "job": "writer",
    "year_of_birth": 1873
}
```

它可以压缩成一行，去除缩进，当数据长度很重要时（如 Web 服务器或数据库），这是一种良好的实践：

```json
{"name:"Randolph","surname":"Carter","job":"writer","year_of_birth":1873}
```

在 Go 中，与 `JSON` 字典和列表关联的默认类型分别是 `map[string]interface{}` 和 `[]interface{}`。这两种类型（因其通用性）能够容纳任何 `JSON` 数据结构。

## 字段标签

`struct` 也可以承载一组特定的 `JSON` 数据；所有导出的键将拥有与相应字段相同的名称。为了自定义键，Go 允许我们在 `struct` 的字段声明后跟随一个字符串，该字符串应包含关于该字段的元数据。

这些标签采用冒号分隔的键/值形式。值是一个带引号的字符串，可以使用逗号包含附加信息（如 `job,omitempty`）。如果有多个标签，则使用空格分隔它们。让我们看一个使用结构体标签的实际示例：

```
type Character struct {
    Name        string `json:"name" tag:"foo"`
    Surname     string `json:"surname"`
    Job         string `json:"job,omitempty"`
    YearOfBirth int    `json:"year_of_birth,omitempty"`
}
```

此示例展示了如何为同一字段使用两个不同的标签（我们同时有 `json` 和 `foo`），并展示了如何指定特定的 `JSON` 键以及引入 `omitempty` 标签，该标签用于输出时避免编组零值字段。

## 解码器

解码 `JSON` 数据有两种方式——第一种是使用字节切片作为输入的 `json.Unmarshal` 函数，第二种是使用通用 `io.Reader` 获取编码内容的 `json.Decoder` 类型。我们将在示例中使用后者，因为它使我们能够使用 `strings.Reader` 等结构体。解码器的另一个优势是可以通过以下方法进行自定义：

* `DisallowUnknownFields`：如果发现接收数据结构未知的字段，解码将返回错误。
* `UseNumber`：数字将作为 `json.Number` 存储，而不是 `float64`。

这是一个使用 `json.Decoder` 类型进行数据解码的实际示例：

```
r := strings.NewReader(`{
    "name":"Lavinia",
    "surname":"Whateley",
    "year_of_birth":1878
}`)
d := json.NewDecoder(r)
var c Character
if err := d.Decode(&c); err != nil {
    log.Fatalln(err)
}
log.Printf("%+v", c)
```

完整示例请见：[`play.golang.org/p/a-qt5Mk9E_J`](https://play.golang.org/p/a-qt5Mk9E_J)。

## 编码器

数据编码的工作方式类似，有一个接受字节切片的 `json.Marshal` 函数和一个使用 `io.Writer` 的 `json.Encoder` 类型。后者因灵活性和可定制性显而易见而更好。它允许我们通过以下方法更改输出：

* `SetEscapeHTML`：如果为 true，指定是否应在 `JSON` 引用字符串内转义有问题的 HTML 字符。
* `SetIndent`：这允许我们指定每行开头的前缀，以及用于缩进输出 `JSON` 的字符串。

以下示例使用编码器将数据结构编组到标准输出，使用制表符进行缩进：

```
e := json.NewEncoder(os.Stdout)
e.SetIndent("", "\t")
c := Character{
    Name: "Charles Dexter",
    Surname: "Ward",
    YearOfBirth: 1902,
}
if err := e.Encode(c); err != nil {
    log.Fatalln(err)
}
```



在这里我们可以看到 `Job` 字段中 `omitempty` 标签的作用。由于值为空字符串，其编码被跳过。如果没有该标签，姓氏后面会有一行 `"job":"",`。

# Marshaler 和 Unmarshaler

编码和解码通常使用反射包完成，这相当慢。在求助于反射之前，编码器和解码器会检查数据类型是否实现了 `json.Marshaler` 和 `json.Unmarshaler` 接口，如果实现了则使用相应的方法：

```go
type Marshaler interface {
    MarshalJSON() ([]byte, error)
}

type Unmarshaler interface {
    UnmarshalJSON([]byte) error
}
```

实现这些接口通常能实现更快的编码和解码，并赋予执行其他操作的能力，这些操作否则是不可能的，例如读取或写入非导出字段；它还可以嵌入一些操作，如对数据运行检查。

如果目标只是包装默认行为，则有必要定义另一个具有相同数据结构的类型，使其丢失所有方法。否则，在方法内部调用 `Marshal` 或 `Unmarshal` 将导致递归调用，最终引发栈溢出。

在这个实际示例中，我们定义了一个自定义的 `UnmarshalJSON` 方法，当 `Job` 字段为空时为其设置默认值：

```go
func (c *Character) UnmarshalJSON(b []byte) error {
    type C Character
    var v C
    if err := json.Unmarshal(b, &v); err != nil {
        return err
    }
    *c = Character(v)
    if c.Job == "" {
        c.Job = "unknown"
    }
    return nil
}
```

完整示例请见：[`play.golang.org/p/4BjFKiMiVHO`](https://play.golang.org/p/4BjFKiMiVHO)。

`UnmarshalJSON` 方法需要指针接收者，因为它必须实际修改数据类型的值，但对于 `MarshalJSON` 方法，并没有真正的必要，使用值接收者是一个好习惯——除非数据类型在 `nil` 时应表现出不同行为：

```go
func (c Character) MarshalJSON() ([]byte, error) {
    type C Character
    v := C(c)
    if v.Job == "" {
        v.Job = "unknown"
    }
    return json.Marshal(v)
}
```

完整示例请见：[`play.golang.org/p/Q-q-9y6v6u-`](https://play.golang.org/p/Q-q-9y6v6u-)。

# 接口

在处理接口类型时，编码部分非常直接，因为应用程序知道接口中存储的是哪种数据结构，并将继续进行编组。执行相反的操作则不那么直接，因为应用程序收到的是一个接口而不是数据结构，不知道该怎么做，因此最终什么也不做。

一种行之有效的策略（即使它涉及少量样板代码）是使用具体类型容器，这将允许我们在 `UnmarshalJSON` 方法中处理接口。让我们通过定义一个接口和几个不同的实现来创建一个快速示例：

```go
type Fooer interface {
    Foo()
}

type A struct{ Field string }

func (a *A) Foo() {}

type B struct{ Field float64 }

func (b *B) Foo() {}
```

然后，我们定义一个包装该接口并带有 `Type` 字段的类型：

```go
type Wrapper struct {
    Type  string
    Value Fooer
}
```

然后，在编码前填充 `Type` 字段：

```go
func (w Wrapper) MarshalJSON() ([]byte, error) {
    switch w.Value.(type) {
    case *A:
        w.Type = "A"
    case *B:
        w.Type = "B"
    default:
        return nil, fmt.Errorf("invalid type: %T", w.Value)
    }
    type W Wrapper
    return json.Marshal(W(w))
}
```

解码方法是更重要的一个：它使用 `json.RawMessage`，这是一种特殊的字节切片类型，用于延迟解码。我们将首先从字符串字段获取类型，并将值保留在原始格式中，以便使用正确的数据结构对其进行解码：

```go
func (w *Wrapper) UnmarshalJSON(b []byte) error {
    var W struct {
        Type  string
        Value json.RawMessage
    }
    if err := json.Unmarshal(b, &W); err != nil {
        return err
    }
    var value interface{}
    switch W.Type {
    case "A":
        value = new(A)
    case "B":
        value = new(B)
    default:
        return fmt.Errorf("invalid type: %s", W.Type)
    }
    if err := json.Unmarshal(W.Value, &value); err != nil {
        return err
    }
    w.Type, w.Value = W.Type, value.(Fooer)
    return nil
}
```

完整示例请见：[`play.golang.org/p/GXMK_hC8Bpv`](https://play.golang.org/p/GXMK_hC8Bpv)。

# 生成结构体

有一个非常有用的应用程序，给定一个 JSON 字符串，它会自动生成 Go 类型，并尝试推断字段类型。你可以在以下地址找到一个部署好的实例：[`mholt.github.io/json-to-go/`](https://mholt.github.io/json-to-go/)。

它节省了一些时间，而且大多数情况下，经过简单转换后数据结构已经是正确的。有时它需要一些更改，例如数字类型，如果你希望某个字段是 `float`，但你的示例 JSON 中是一个整数。

# JSON Schema

JSON Schema 是一种描述 JSON 数据的词汇，可用于验证数据的有效性。它可用于测试，也可用作文档。Schema 指定元素的类型，并可以对其值添加额外的检查。如果类型是数组，它还可以指定每个元素的类型和详细信息。如果类型是对象，它描述其字段。让我们看看示例中使用的 `Character` 结构体的 JSON Schema：

```json
{
    "type": "object",
    "properties": {
        "name": { "type": "string" },
        "surname": { "type": "string" },
        "year_of_birth": { "type": "number"},
        "job": { "type": "string" }
    },
    "required": ["name", "surname"]
}
```

我们可以看到，它指定了一个包含所有字段的对象，并指明了哪些字段是强制性的。有一些第三方 Go 包允许我们非常容易地根据 Schema 验证 JSON，例如 [github.com/xeipuuv/gojsonschema](https://github.com/xeipuuv/gojsonschema)。

# XML

**可扩展标记语言** (**XML**) 是另一种广泛使用的数据编码格式。它像 JSON 一样既可被人阅读也可被机器阅读，并于 1996 年由 **万维网联盟** (**W3C**) 定义。它专注于简洁性、可用性和通用性，实际上被用作许多格式的基础，包括 RSS 或 XHTML。

# 结构

每个 XML 文件都以声明语句开始，该语句指定文件中使用的版本和编码，以及文件是否独立（使用的 Schema 是内部的）。这是一个 XML 声明示例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

声明之后是 XML 元素树，这些元素由具有以下形式的标签定界：

- `<tag>`：开始标签，定义元素的开始
- `</tag>`：结束标签，定义元素的结束
- `<tag/>`：自闭合标签，定义无内容的元素

通常，元素是嵌套的，因此标签内部还有其他标签：

```xml
<outer>
    <middle>
        <inner1>content</inner1>
        <inner2/>
    </middle>
</outer>
```

每个元素可以拥有以属性形式存在的附加信息，属性是位于开始标签或自闭合标签内的、以空格分隔的键/值对。键和值由等号分隔，值由双引号定界。以下是带有属性的元素示例：

```xml
<tag attribute="value" another="something">content</tag>
<selfclosing a="1000" b="-1"/>
```

# 文档类型定义




## 文档类型定义 (DTD)

**文档类型定义** (**DTD**) 是一个 XML 文档，用于定义其他 XML 文档的结构和约束。它可用于验证 XML 的有效性，前提是其内容符合预期。XML 可以且应该指定自己的模式，以简化验证过程。DTD 的元素如下：

*   **模式**：这代表文档的根节点。
*   **复杂类型**：它允许元素包含内容。
*   **序列**：这指定了必须按描述顺序出现的子元素。
*   **元素**：这代表一个 XML 元素。
*   **属性**：这代表父标签的一个 XML 属性。

这是本章中使用的 `Character` 结构体的示例模式声明：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="character">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="name" type="xs:string" use="required"/>
        <xs:element name="surname" type="xs:string" use="required"/>
        <xs:element name="year_of_birth" type="xs:integer"/>
        <xs:element name="job" type="xs:string"/>
      </xs:sequence>
      <xs:attribute name="id" type="xs:string" use="required"/>
    </xs:complexType>
 </xs:element>
</xs:schema>
```

我们可以看到，这是一个包含元素 (`character`) 的模式，该元素是一个由其他元素序列组成的复杂类型。

## 解码与编码 (XML)

正如我们在 JSON 中所见，数据解码和编码可以通过两种不同的方式实现：使用 `xml.Unmarshal` 和 `xml.Marshal` 提供或返回字节切片，或者结合 `io.Reader` 或 `io.Writer` 使用 `xml.Decoder` 和 `xml.Encoder` 类型。

我们可以通过将 `Character` 结构体中的 `json` 标签替换为 `xml`，或者直接添加它们来实现：

```go
type Character struct {
    Name        string `xml:"name"`
    Surname     string `xml:"surname"`
    Job         string `xml:"job,omitempty"`
    YearOfBirth int    `xml:"year_of_birth,omitempty"`
}
```

然后，我们使用 `xml.Decoder` 来解码数据：

```go
r := strings.NewReader(`<?xml version="1.0" encoding="UTF-8"?>
<character>
 <name>Herbert</name>
 <surname>West</surname>
 <job>Scientist</job>
</character>
}`)
d := xml.NewDecoder(r)
var c Character
if err := d.Decode(&c); err != nil {
    log.Fatalln(err)
}
log.Printf("%+v", c)
```

完整示例请见此处：<https://play.golang.org/p/esopq0SMhG_T>。

编码时，`xml` 包将从所用的数据类型获取根节点的名称。如果数据结构有一个名为 `XMLName` 的字段，则相关的 XML `struct` 标签将用于根节点。因此，数据结构变为如下所示：

```go
type Character struct {
    XMLName     struct{} `xml:"character"`
    Name        string   `xml:"name"`
    Surname     string   `xml:"surname"`
    Job         string   `xml:"job,omitempty"`
    YearOfBirth int      `xml:"year_of_birth,omitempty"`
}
```

编码操作也非常直接：

```go
e := xml.NewEncoder(os.Stdout)
e.Indent("", "\t")
c := Character{
    Name:        "Henry",
    Surname:     "Wentworth Akeley",
    Job:         "farmer",
    YearOfBirth: 1871,
}
if err := e.Encode(c); err != nil {
    log.Fatalln(err)
}
```

完整示例请见此处：<https://play.golang.org/p/YgZzdPDoaLX>。

## 字段标签

可以使用数据结构中的 `XMLName` 字段来更改根标签的名称。字段标签还有其他一些非常有用的特性：

*   带有 `\-` 的标签会被忽略。
*   带有 `attr` 选项的标签将成为父元素的属性。
*   带有 `innerxml` 选项的标签将原样写入，适用于惰性解码。
*   `omitempty` 选项的工作方式与 JSON 中相同；它不会为零值生成标签。
*   标签可以包含 XML 中的路径，使用 `\>` 作为分隔符，例如 `a > b > c`。
*   匿名结构体字段的处理方式就好像其值的字段在外部结构体中一样。

让我们看一个使用其中一些特性的实际例子：

```go
type Character struct {
    XMLName     struct{} `xml:"character"`
    Name        string   `xml:"name"`
    Surname     string   `xml:"surname"`
    Job         string   `xml:"details>job,omitempty"`
    YearOfBirth int      `xml:"year_of_birth,attr,omitempty"`
    IgnoreMe    string   `xml:"-"`
}
```

此结构生成以下 XML：

```xml
<character year_of_birth="1871">
  <name>Henry</name>
  <surname>Wentworth Akeley</surname>
  <details>
    <job>farmer</job>
  </details>
</character>
```

完整示例请见此处：<https://play.golang.org/p/6zdl9__M0zF>。

## Marshaler 和 Unmarshaler

正如我们在 JSON 中所见，`xml` 包提供了一些接口，用于自定义类型在编码和解码操作期间的行为——这可以避免使用反射，或用于建立不同的行为。该包提供用于获得此行为的接口如下：

```go
type Marshaler interface {
    MarshalXML(e *Encoder, start StartElement) error
}

type MarshalerAttr interface {
    MarshalXMLAttr(name Name) (Attr, error)
}

type Unmarshaler interface {
    UnmarshalXML(d *Decoder, start StartElement) error
}

type UnmarshalerAttr interface {
    UnmarshalXMLAttr(attr Attr) error
}
```

这里有两对函数——一对用于将类型作为元素进行解码或编码，另一对用于将其作为属性进行处理。让我们看看实际效果。首先，我们为自定义类型定义一个 `MarshalXMLAttr` 方法：

```go
type Character struct {
    XMLName     struct{} `xml:"character"`
    ID          ID       `xml:"id,attr"`
    Name        string   `xml:"name"`
    Surname     string   `xml:"surname"`
    Job         string   `xml:"job,omitempty"`
    YearOfBirth int      `xml:"year_of_birth,omitempty"`
}

type ID string

func (i ID) MarshalXMLAttr(name xml.Name) (xml.Attr, error) {
    return xml.Attr{
        Name:  xml.Name{Local: "codename"},
        Value: strings.ToUpper(string(i)),
    }, nil
}
```

然后，我们编组一些数据，将看到属性名被替换为 `codename`，其值变为大写，正如该方法指定的那样：

```go
e := xml.NewEncoder(os.Stdout)
e.Indent("", "\t")
c := Character{
    ID:           "aa",
    Name:         "Abdul",
    Surname:      "Alhazred",
    Job:          "poet",
    YearOfBirth:  700,
}
if err := e.Encode(c); err != nil {
    log.Fatalln(err)
}
```

完整示例请见此处：<https://play.golang.org/p/XwJrMozQ6RY>。

## 生成结构体

正如 JSON 所见，有一个第三方包可以从编码文件生成 Go 结构体。对于 XML，我们有 <https://github.com/miku/zek>。

它处理任何类型的 XML 数据，包括带有属性的元素、元素间的间距或注释。

# YAML

**YAML** 是一个递归首字母缩写，代表 **YAML Ain't Markup Language**（YAML 不是标记语言），它是另一种广泛使用的数据编码格式的名称。它的成功部分归功于它比 JSON 和 XML 更易于编写、轻量级且灵活。

## 结构

YAML 使用缩进表示作用域，使用换行符分隔实体。序列中的元素以短横线加空格开头。键和值由冒号分隔，井号用于注释。示例 YAML 文件如下所示：

```yaml
# list of characters
characters: 
    - name: "Henry"
      surname: "Armitage"
      year_of_birth: 1855
      job: "librarian"
    - name: "Francis"
      surname: "Wayland Thurston"
      job: "anthropologist"
```

JSON 和 YAML 之间一个更重要的区别是，前者只能使用字符串作为键，而后者可以使用任何类型的标量值（字符串、数字和布尔值）。

## 解码与编码 (YAML)





# YAML

YAML 不包含在 Go 标准库中，但有许多第三方库可用。更常用于处理此格式的包是 `go-yaml` 包（`https://gopkg.in/yaml.v2`）。

它是使用以下标准编码包结构构建的：

*   有编码器和解码器。
*   有 `Marshal`/`Unmarshal` 函数。
*   它允许 `struct` 标签。
*   类型的行为可以通过实现所定义接口的方法来自定义。

接口略有不同——`Unmarshaler` 接收默认的封送函数作为参数，随后可将其用于不同于该类型的数据结构：

```
type Marshaler interface {
    MarshalYAML() (interface{}, error)
}

type Unmarshaler interface {
    UnmarshalYAML(unmarshal func(interface{}) error) error
}
```

我们可以像使用 JSON 标签一样使用 `struct` 标签：

```
type Character struct {
    Name        string `yaml:"name"`
    Surname     string `yaml:"surname"`
    Job         string `yaml:"job,omitempty"`
    YearOfBirth int    `yaml:"year_of_birth,omitempty"`
}
```

我们可以用它们来编码数据结构，或者在本例中，编码一个结构体列表：

```
var chars = []Character{{
    Name:        "William",
    Surname:     "Dyer",
    Job:         "professor",
    YearOfBirth: 1875,
}, {
    Surname: "Danforth",
    Job:     "student",
}}
e := yaml.NewEncoder(os.Stdout)
if err := e.Encode(chars); err != nil {
    log.Fatalln(err)
}
```

解码的工作方式相同，如下所示：

```
r := strings.NewReader(`- name: John Raymond
 surname: Legrasse
 job: policeman
- name: "Francis"
 surname: Wayland Thurston
 job: anthropologist`)
// 定义一个新的解码器
d := yaml.NewDecoder(r)
var c []Character
// 解码 reader
if err := d.Decode(&c); err != nil {
 log.Fatalln(err)
}
log.Printf("%+v", c)
```

我们可以看到，创建 `Decoder` 只需要 `io.Reader` 和用于执行解码的接收结构体。

# 了解二进制编码

二进制编码协议使用字节，因此它们的字符串表示形式对人类不友好。它们通常不可作为字符串阅读，且难以编写，但体积较小，从而实现了应用程序间更快的通信。

## BSON

BSON 是 JSON 的二进制版本。它被 MongoDB 使用，并支持 JSON 中不可用的一些数据类型，例如日期和二进制数据。

有几个包实现了 BSON 编码和解码，其中两个非常普及。一个在官方 MongoDB Go 驱动程序内部，`github.com/mongodb/mongo-go-driver`。另一个非官方，但自 Go 语言诞生之初就已存在，它是非官方 MongoDB 驱动程序 `gopkg.in/mgo.v2` 的一部分。

第二个在接口和函数方面都与 JSON 包非常相似。这些接口被称为 getter 和 setter：

*   `GetBSON` 返回将要被编码的实际数据结构。
*   `SetBSON` 接收 `bson.Raw`，它是 `[]byte` 的包装器，可用于 `bson.Unmarshal`。

这些 getter 和 setter 的用例如下：

```
type Setter interface {
    SetBSON(raw Raw) error
}

type Getter interface {
    GetBSON() (interface{}, error)
}
```

### 编码

BSON 是为文档/实体设计的格式；因此，用于编码和解码的数据结构应该是结构体或映射，但不能是切片或数组。`mgo` 版本的 `bson` 不提供常规的编码器，只提供 marshal：

```
var char = Character{
    Name: "Robert",
    Surname: "Olmstead",
}
b, err := bson.Marshal(char)
if err != nil {
    log.Fatalln(err)
}
log.Printf("%q", b)
```

### 解码

同样的情况也适用于 `Unmarshal` 函数：

```
r := []byte(",\x00\x00\x00\x02name\x00\a\x00\x00" +
 "\x00Robert\x00\x02surname\x00\t\x00\x00\x00" +
 "Olmstead\x00\x00")
var c Character
if err := bson.Unmarshal(r, &c); err != nil {
 log.Fatalln(err)
}
log.Printf("%+v", c)
```

## gob

`gob` 编码是另一种内置于标准库的二进制编码，实际上是由 Go 语言本身引入的。它是数据项的流，每个数据项前面都有一个类型声明，并且它不允许指针。它使用指针的值，禁止使用 `nil` 指针（因为它们没有值）。该包在处理具有指针且会创建递归结构的类型时也存在问题，这可能导致意外行为。

数字具有任意精度，可以是浮点数、有符号或无符号整数。有符号整数可以存储在任何有符号整数类型中，无符号整数可以存储在任何无符号整数类型中，浮点值可以接收在任何浮点变量中。然而，如果变量无法表示该值（例如溢出），解码将失败。字符串和字节切片采用非常高效的表示形式存储，该形式尝试复用相同的底层数组。结构体将只解码导出字段，因此函数和通道将被忽略。

### 接口

`gob` 用于替换默认 marshal 和 unmarshal 行为的接口位于 `encoding` 包中：

```
type BinaryMarshaler interface {
        MarshalBinary() (data []byte, err error)
}

type BinaryUnmarshaler interface {
        UnmarshalBinary(data []byte) error
}
```

在解码阶段，任何不存在的结构体字段都会被忽略，因为字段名也是序列化的一部分。

### 编码

让我们尝试使用 `gob` 编码一个结构体：

```
var char = Character{
    Name:    "Albert",
    Surname: "Wilmarth",
    Job:     "assistant professor",
}
s := strings.Builder{}
e := gob.NewEncoder(&s)
if err := e.Encode(char); err != nil {
    log.Fatalln(err)
}
log.Printf("%q", s.String())
```

### 解码

解码数据也非常简单；它的工作方式与我们已经见过的其他编码包相同：

```
r := strings.NewReader("D\xff\x81\x03\x01\x01\tCharacter" +
    "\x01\xff\x82\x00\x01\x04\x01\x04Name" +
    "\x01\f\x00\x01\aSurname\x01\f\x00\x01\x03" +
    "Job\x01\f\x00\x01\vYearOfBirth\x01\x04\x00" +
    "\x00\x00\x00*\xff\x82\x01\x06Albert\x01\bWilmarth" +
    "\x01\x13assistant professor\x00")
d := gob.NewDecoder(r)
var c Character
if err := d.Decode(&c); err != nil {
    log.Fatalln(err)
}
log.Printf("%+v", c)
```

现在，让我们尝试用不同的结构体解码相同的数据——原始结构体以及一些带有额外或缺失字段的结构体。我们将这样做以观察该包的行为。让我们定义一个通用的解码函数并将不同类型的结构体传递给解码器：

```
func runDecode(data []byte, v interface{}) {
    if err := gob.NewDecoder(bytes.NewReader(data)).Decode(v); err != nil {
        log.Fatalln(err)
    }
    log.Printf("%+v", v)    
}
```

让我们尝试更改结构体中字段的顺序，看看 `gob` 解码器是否仍能工作：

```
runDecode(data, new(struct {
    YearOfBirth int    `gob:"year_of_birth,omitempty"`
    Surname     string `gob:"surname"`
    Name        string `gob:"name"`
    Job         string `gob:"job,omitempty"`
}))
```

让我们移除一些字段：

```
runDecode(data, new(struct {
    Name string `gob:"name"`
}))
```

让我们在中间插入一个字段：

```
runDecode(data, new(struct {
    Name        string `gob:"name"`
    Surname     string `gob:"surname"`
    Country     string `gob:"country"`
    Job         string `gob:"job,omitempty"`
    YearOfBirth int    `gob:"year_of_birth,omitempty"`
}))
```

我们可以看到，即使我们打乱、添加或移除字段，该包仍能继续工作。但如果我们尝试将现有字段的类型更改为另一种类型，则会失败：

```
runDecode(data, new(struct {
    Name []byte `gob:"name"`
}))
```

### 接口

关于此包的另一个注意事项是，如果您正在使用接口，则应首先使用以下函数注册其实现：





## Gob 类型注册

```
func Register(value interface{})
func RegisterName(name string, value interface{})
```

这将使包感知指定的类型，并允许我们在接口类型上调用 `decode`。首先定义一个接口及其结构体实现：

```
type Greeter interface {
    Greet(w io.Writer)
}

type Character struct {
    Name        string `gob:"name"`
    Surname     string `gob:"surname"`
    Job         string `gob:"job,omitempty"`
    YearOfBirth int    `gob:"year_of_birth,omitempty"`
}

func (c Character) Greet(w io.Writer) {
    fmt.Fprintf(w, "Hello, my name is %s %s", c.Name, c.Surname)
    if c.Job != "" {
        fmt.Fprintf(w, " and I am a %s", c.Job)
    }
}
```

如果不使用 `gob.Register` 函数运行以下代码，会返回错误：

```
gob: name not registered for interface: "main.Character"
```

但如果注册了类型，则可正常工作。注意数据是通过编码包含 `Character` 结构的 `Greeter` 指针获得的：

```
func main() {
    gob.Register(Greeter(Character{}))
    r := strings.NewReader("U\x10\x00\x0emain.Character" +
        "\xff\x81\x03\x01\x01\tCharacter\x01\xff\x82\x00" +
        "\x01\x04\x01\x04Name\x01\f\x00\x01\aSurname" +
        "\x01\x04\x00\x01\x03Job\x01\f\x00\x01\vYearOfBirth" +
        "\x01\x04\x00\x00\x00\x1f\xff\x82\x1c\x01\x05John" +
        " \x01\aKirowan\x01\tprofessor\x00")
    var char Greeter
    if err := gob.NewDecoder(r).Decode(&char); err != nil {
        log.Fatalln(err)
    }
    char.Greet(os.Stdout)
}
```

# Proto

**Protocol Buffer** 是 Google 开发的一种序列化协议。它与语言、平台无关，开销小，效率极高。核心思想是：一次性定义数据结构，然后使用工具为目标应用语言生成源代码。

# Structure

生成代码所需的主文件是 `.proto` 文件，使用特定语法。我们关注最新的协议语法版本 `proto3`。

在第一行指定文件使用的语法版本：

```
syntax = "proto3";
```

可使用 `import` 语句引用其他文件的定义：

```
import "google/protobuf/any.proto";
```

文件其余部分包含消息（数据类型）和服务的定义。服务是用于定义 RPC 服务的接口：

```
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}

service SearchService {
  rpc Search (SearchRequest) returns (SearchResponse);
}
```

消息由字段组成，服务由方法组成。字段类型分为标量类型（包括各种整数、有符号整数、浮点数、字符串和布尔值）和其他消息类型。每个字段有一个关联的数字作为其标识符，一旦选定不应更改，以保持与旧版本消息的兼容性。

使用 `reserved` 关键字可防止某些字段或 ID 被重用，这对避免 Bug 非常有用：

```
message Foo {
  // 锁定字段 ID
  reserved 2, 15, 9 to 11;
  // 锁定字段名
  reserved "foo", "bar";
}
```

# Code generation

从 `.proto` 文件生成代码需要 `protoc` 应用和官方 proto 生成包：

```
go get -u github.com/golang/protobuf/protoc-gen-go
```

安装的包附带 `protoc-gen-go` 命令；这使 `protoc` 命令能使用 `--go_out` 标志在指定文件夹生成 Go 源文件。Go 1.4 版可通过 `go generate` 命令指定特殊注释自动生成代码，这些注释以 `//go:generate` 开头，后跟命令，示例如下：

```
//go:generate protoc -I=$SRC_PATH --go_out=$DST_DIR source.proto
```

这允许指定导入查找的源路径、输出目录和源文件。路径相对于包含注释的包目录，可用 `go generate $pkg` 命令调用。

从简单的 `.proto` 文件开始：

```
syntax = "proto3";

message Character {
    string name = 1;
    string surname = 2;
    string job = 3;
    int32 year_of_birth = 4;
}
```

在同一文件夹创建包含生成代码注释的 Go 源文件：

```
package gen

//go:generate protoc --go_out=. char.proto
```

现在可运行 `go generate` 命令，生成与 `.proto` 文件同名且扩展名为 `.pb.go` 的文件。该文件包含 `.proto` 文件中定义的类型和服务的 Go 源码：

```
// Code generated by protoc-gen-go. DO NOT EDIT.
// source: char.proto
...
type Character struct {
  Name        string `protobuf:"bytes,1,opt,name=name"`
  Surname     string `protobuf:"bytes,2,opt,name=surname"`
  Job         string `protobuf:"bytes,3,opt,name=job" json:"job,omitempty"`
  YearOfBirth int32  `protobuf:"varint,4,opt,name=year_of_birth,json=yearOfBirth"`
}
```

# Encoding

该包允许使用 `proto.Buffer` 类型编码 `pb.Message` 值。`protoc` 生成的类型实现了该接口，因此 `Character` 类型可直接使用：

```
var char = gen.Character{
    Name:        "George",
    Surname:     "Gammell Angell",
    YearOfBirth: 1834,
    Job:         "professor emeritus",
}
b := proto.NewBuffer(nil)
if err := b.EncodeMessage(&char); err != nil {
    log.Fatalln(err)
}
log.Printf("%q", b.Bytes())
```

生成的编码数据与其他编码相比几乎没有开销。

# Decoding

解码操作也需使用 `proto.Buffer` 方法和生成的类型执行：

```
b := proto.NewBuffer([]byte(
    "/\n\x06George\x12\x0eGammell Angell" +
    "\x1a\x12professor emeritus \xaa\x0e",
))
var char gen.Character
if err := b.DecodeMessage(&char); err != nil {
    log.Fatalln(err)
}
log.Printf("%+v", char)
```

# gRPC protocol

Google 使用 Protocol Buffer 编码构建了一种称为 **gRPC** 的 Web 协议。它是一种远程过程调用，使用 HTTP/2 建立连接，使用 Protocol Buffer 进行数据的编组和解组。

第一步是在目标语言中生成服务端相关代码。这将生成服务端接口和客户端的工作实现。接下来需手动创建服务端实现，最后目标语言将使该实现可用于 gRPC 服务端，并使用客户端连接和交互。

`go-grpc` 包中有不同示例，包括一对客户端/服务端。客户端使用生成的代码，只需一个可用的 gRPC 连接到服务端，即可使用服务中指定的方法：

```
conn, err := grpc.Dial(address, grpc.WithInsecure())
if err != nil {
    log.Fatalf("did not connect: %v", err)
}
defer conn.Close()
c := pb.NewGreeterClient(conn)

// Contact the server and print out its response
r, err := c.SayHello(ctx, &pb.HelloRequest{Name: name})
```

完整代码见 [grpc/grpc-go/blob/master/examples/helloworld/greeter_client/main.go](https://github.com/grpc/grpc-go/blob/master/examples/helloworld/greeter_client/main.go)。

服务端是客户端接口的实现：

```
// server is used to implement helloworld.GreeterServer.
type server struct{}

// SayHello implements helloworld.GreeterServer
func (s *server) SayHello(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
    log.Printf("Received: %v", in.Name)
    return &pb.HelloReply{Message: "Hello " + in.Name}, nil
}
```




此接口实现可连同有效的 gRPC 服务器一起传递给生成的注册函数 `RegisterGreeterServer`，并可使用 TCP 监听器服务传入连接：

```go
func main() {
    lis, err := net.Listen("tcp", port)
    if err != nil {
        log.Fatalf("failed to listen: %v", err)
    }
    s := grpc.NewServer()
    pb.RegisterGreeterServer(s, &server{})
    if err := s.Serve(lis); err != nil {
        log.Fatalf("failed to serve: %v", err)
    }
}
```

完整代码见 [grpc/grpc-go/blob/master/examples/helloworld/greeter_server/main.go](https://github.com/grpc/grpc-go/blob/master/examples/helloworld/greeter_server/main.go)。

# 总结

在本章中，我们探讨了 Go 标准包和第三方库提供的编码方法。它们可分为两大类。第一类是基于文本的编码方法，人类和机器都易于读写。然而，它们的开销较大，往往比二进制编码慢得多。二进制编码方法开销小，但人类不可读。

在基于文本的编码中，我们有 JSON、XML 和 YAML。前两者由标准库处理，最后一个需要外部依赖。我们探讨了 Go 如何允许我们指定结构体标签以更改默认的编码和解码行为，以及如何在这些操作中使用这些标签。接着，我们检查并实现了定义在序列化和反序列化操作期间自定义行为的接口。有一些第三方工具允许我们从 JSON 文件或 JSON Schema 生成数据结构，JSON Schema 是用于定义其他 JSON 文档结构的 JSON 文件。

XML 是另一种广泛使用的文本格式，HTML 基于此。我们检查了 XML 语法和组合元素，然后展示了一种称为 DTD 的特定文档类型，用于定义其他 XML 文件的内容。我们学习了 XML 中编码和解码的工作原理，以及关于 `struct` 标签与 JSON 的差异，这些标签允许我们为类型定义嵌套 XML 元素，或从属性存储/加载字段。我们以第三方 YAML 包结束了基于文本的编码部分。

我们展示的第一种二进制编码是 BSON，一种被 MongoDB 使用的 JSON 二进制版本（由第三方包处理）。`gob` 是另一种二进制编码方法，但它是 Go 标准库的一部分。我们了解到，编码和解码连同涉及的接口，均以标准包的方式工作——类似于 JSON 和 XML。

最后，我们研究了 Protocol Buffer 编码，如何编写 `.proto` 文件及其 Go 代码生成用法，以及如何使用它编码和解码数据。我们还介绍了 gRPC 编码的实际示例，它利用这种编码创建客户端/服务端应用程序。

在下一章中，我们将开始深入 Go 的并发模型，从内置类型——通道和 Goroutine 开始。

# 问题

1. 文本编码和二进制编码之间的权衡是什么？

2. Go 默认如何处理数据结构？

3. 如何改变这种行为？

4. 结构体字段如何在 XML 属性中编码？

5. 解码 `gob` 接口值需要什么操作？

6. 什么是 Protocol Buffer 编码？
