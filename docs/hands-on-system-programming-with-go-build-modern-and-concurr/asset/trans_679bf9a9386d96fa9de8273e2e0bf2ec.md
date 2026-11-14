
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

上述代码的完整示例可在 [https://play.golang.org/p/qwaz3xCJhQT](https://play.golang.org/p/qwaz3xCJhQT) 查看。

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

上述代码的完整示例可在 [https://play.golang.org/p/lt_GBOLvUfk](https://play.golang.org/p/lt_GBOLvUfk) 查看。

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

上述代码的完整示例可在 [https://play.golang.org/p/KPHXRW5OxXT](https://play.golang.org/p/KPHXRW5OxXT) 查看。

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

完整示例请见：[https://play.golang.org/p/a-qt5Mk9E_J](https://play.golang.org/p/a-qt5Mk9E_J)。

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
