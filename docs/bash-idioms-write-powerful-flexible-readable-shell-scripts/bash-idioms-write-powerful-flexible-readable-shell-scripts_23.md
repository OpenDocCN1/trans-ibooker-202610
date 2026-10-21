## 使用 jq 处理 JSON

jq 不是 bash，但我们想涵盖它，因为 JSON 非常常见，尤其是在处理云基础设施时，我们见过一些使用 awk、grep 和 sed 等工具来尝试解析 JSON 的糟糕代码。尽可能使用 jq 代替。

![](img/553ffa1bc15093729b691f706befc0ac_127_0.png)

### jq 并非总是可用

不幸的是，尽管 JSON 已变得非常普遍，但 jq 并非像 awk、grep 和 sed 那样默认安装。它也不包含在 Windows 的 Git Bash 中，而 Git Bash 包含 awk、sed、cut、grep、tr 等。

有时你可以避开这个问题。例如，对于 AWS，你可以使用 --output=text，然后使用常规工具。否则，你可以在默认构建中安装 jq，或使用 Python 等工具寻找变通方法，这超出了本书的范围。

本书的源代码包含一个 atlas.json 文件，因此我们可以将其用于一些示例。如果我们想找到本书的标题，可以这样做：

```
$ jq '.title' atlas.json
"bash Idioms"
$ jq -r '.title' atlas.json  # "Raw" mode, without the quotes
bash Idioms
```

#### 但 `grep` 也能工作...有点
```
$ grep 'title' atlas.json
  "titlepage.html",
  "title": "bash Idioms",
```

如果我们想知道各种输出格式是否启用了语法高亮呢？用 grep 做起来比较棘手，但用 jq 很简单：

```
### What do we have?
$ jq '.formats | keys' atlas.json
[
  "epub",
  "html",
  "mobi",
  "pdf"
]

### Bad; this has no context and is useless
$ grep 'syntaxhighlighting' atlas.json
  "syntaxhighlighting": true,
  "syntaxhighlighting": true,
  "syntaxhighlighting": true,
  "syntaxhighlighting": true,
```

```
### Good; this tells us that PDF output does have syntax highlighting
$ jq '.formats.pdf.syntaxhighlighting' atlas.json
true
```

```
### Better, but very confusing, show formats and syntax value, see jq docs
$ jq -r '.formats | keys[] as $k | "\($k), \(.[$k] \
| .syntaxhighlighting)"' atlas.json
epub, true
html, true
mobi, true
pdf, true
```

![](img/553ffa1bc15093729b691f706befc0ac_128_0.png)

## 在 Firefox 中读取 JSON

Firefox 拥有出色的内置 JSON 查看器，可以帮助你更好地了解所看到的内容以及如何让 jq 输出它。只需将你的 JSON 输出保存为 myfile.json，然后在 Firefox 中打开 file:///path/to/myfile.json 即可。
