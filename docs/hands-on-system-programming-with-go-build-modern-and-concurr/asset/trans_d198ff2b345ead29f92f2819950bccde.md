

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

