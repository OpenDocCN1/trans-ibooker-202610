
# 10.2 对 XML 执行 XPath 查询

## 问题

您想要使用 XML 的标准 `XPath 语法` 对 XML 文件执行高级查询。

## 解决方案

使用 PowerShell 的 `Select-Xml` cmdlet 对文件执行 XPath 查询。

例如，要在 RSS 源中查找所有长度小于 30 个字符的帖子标题：

```
PS > $query = "/rss/channel/item[string-length(title) < 30]/title"
PS > Select-Xml -XPath $query -Path .\powershell_blog.xml | Select -Expand Node

#text
-----
Remoting for non-Admins
```

## 讨论

尽管 XPath 查询语法是一门独立的语言，但它提供了一种强大的、以 XML 为中心的方式来为 XML 文件编写高级查询。`Select-Xml` cmdlet 允许您将这些概念应用于文件、XML 节点，甚至是纯文本。

### 注意

`Select-Xml` cmdlet 支持的 XPath 查询是一种流行的行业标准。但请注意。与 PowerShell 的其他部分不同，这些查询**区分大小写**！

`Select-Xml` cmdlet 生成一个 `SelectXmlInfo` 对象。这使您可以将单独的 XPath 查询链接在一起。要检索选择的实际结果，请访问 `Node` 属性：

```
PS > Get-Content page.html
<HTML>
    <HEAD>
        <TITLE>Welcome to my Website</TITLE>
    </HEAD>
    <BODY>
        <P>...</P>
    </BODY>
</HTML>
PS > $content = [xml] (Get-Content page.html)
PS > $result = $content | Select-Xml "/HTML/HEAD" | Select-Xml "TITLE"
PS > $result

Node                     Path                     Pattern
----                     ----                     -------
TITLE                    InputStream              TITLE


PS > $result.Node

#text
-----
Welcome to my Website
```

这甚至适用于通过 PowerShell 的 XML 支持访问的内容，如本例所示，它使用了从 PowerShell 博客下载的 RSS 源：

```
PS > $xml = [xml] (Get-Content powershell_blog.xml)
PS > $xml | Select-Xml $query | Select -Expand Node

#text
-----
Remoting for non-Admins
```

对于较简单的查询，您可能会发现 PowerShell 基于对象的 XML 导航概念更易于使用。有关通过 PowerShell 的 XML 类型处理 XML 的更多信息，请参阅 [附录 F](#app06.xhtml#selected_net_classes_and_their_uses) 中的 [表 F-11](#app06.xhtml#xml-id001)。有关 XPath 语法的更多信息，请参阅 [附录 C](#app03.xhtml#xpath_reference)。

## 另请参阅

- [附录 C，*XPath 快速参考*](#app03.xhtml#xpath_reference)
- [表 F-11](#app06.xhtml#xml-id001)

# 10.3 将对象转换为 XML

## 问题

您想要将命令输出转换为 XML，以便进一步处理或查看。

## 解决方案

使用 PowerShell 的 `ConvertTo-Xml` cmdlet 将命令的输出另存为 XML：

```
$xml = Get-Process | ConvertTo-Xml
```

然后，您可以使用 PowerShell 的 XML 支持（XML 导航、`Select-Xml` 等）来处理该内容。

## 讨论

尽管通常直接使用完整保真度的对象最为容易，但有时您可能希望将它们转换为 XML，以便由其他程序进一步处理。解决方案是 `ConvertTo-Xml` cmdlet。

### 注意

PowerShell 还包含一个听起来相似的 cmdlet，称为 `Export-CliXml`。与旨在为人类和程序生成有用输出的 `ConvertTo-Xml` cmdlet 不同，`Export-CliXml` cmdlet 专为以 PowerShell 为中心的数据交换而设计。有关更多信息，请参阅 [配方 10.5](#ch10.xhtml#easily_import_and_export_your_structured)。

`ConvertTo-Xml` cmdlet 为您提供了两种主要的转换目标。默认是 XML 文档，这是 PowerShell 中 `[xml]` 转换创建的同类型对象。这也是 `Select-Xml` cmdlet 支持的格式，因此您可以将 `ConvertTo-Xml` 的输出直接通过管道传递给它。

```
PS > $xml = Get-Process | ConvertTo-Xml
PS > $xml | Select-Xml '//Property[@Name = "Name"]' | Select -Expand Node

Name                     Type                     #text
----                     ----                     -----
Name                     System.String            audiodg
Name                     System.String            csrss
Name                     System.String            dwm
(...)
```

第二种格式是简单的字符串，适合重定向到文件中。要将 XML 保存到文件中，请使用 `-As` 参数并指定 `String` 作为参数，然后使用文件重定向操作符：

```
Get-Process | ConvertTo-Xml -As String > c:\temp\processes.xml
```

如果您已经从 `ConvertTo-Xml` 或 PowerShell 的 `[xml]` 转换获得了 XML 文档，仍可通过调用其 `Save()` 方法将其保存到文件中：

```
$xml = Get-Process | ConvertTo-Xml
$xml.Save("c:\temp\output.xml")
```

有关如何在 PowerShell 中处理 XML 数据的更多信息，请参阅 [配方 10.1](#ch10.xhtml#access_information_in_an_xml_file)。

## 另请参阅

- [配方 10.1，“访问 XML 文件中的信息”](#ch10.xhtml#access_information_in_an_xml_file)
- [配方 10.5，“轻松导入和导出结构化数据”](#ch10.xhtml#easily_import_and_export_your_structured)

# 10.4 修改 XML 文件中的数据

## 问题

您想要使用 PowerShell 修改 XML 文件中的数据。

## 解决方案

要修改 XML 文件中的数据，请将文件加载到 PowerShell 的 XML 数据类型中，更改您想要的内容，然后将文件保存回磁盘。[示例 10-4](#ch10.xhtml#modifying_an_xml_file_from_powershell) 演示了这种方法。

### 示例 10-4. 从 PowerShell 修改 XML 文件

```
PS > ## Store the filename
PS > $filename = (Get-Item phone.xml).FullName
PS >
PS > ## Get the content of the file, and load it
PS > ## as XML
PS > Get-Content $filename
<AddressBook>
  <Person contactType="Personal">
    <Name>Lee</Name>
    <Phone type="home">555-1212</Phone>
    <Phone type="work">555-1213</Phone>
  </Person>
  <Person contactType="Business">
    <Name>Ariel</Name>
    <Phone>555-1234</Phone>
  </Person>
</AddressBook>
PS > $phoneBook = [xml] (Get-Content $filename)
PS >
PS > ## Get the part with data we want to change
PS > $person = $phoneBook.AddressBook.Person[0]
PS >
PS > ## Change the text part of the information,
PS > ## and the type (which was an attribute)
PS > $person.Phone[0]."#text" = "555-1214"
PS > $person.Phone[0].type = "mobile"
PS >
PS > ## Add a new phone entry
PS > $newNumber = [xml] '<Phone type="home">555-1215</Phone>'
PS > $newNode = $phoneBook.ImportNode($newNumber.Phone, $true)
PS > [void] $person.AppendChild($newNode)
PS >
PS > ## Save the file to disk
PS > $phoneBook.Save($filename)
PS > Get-Content $filename
<AddressBook>
  <Person contactType="Personal">
    <Name>Lee</Name>
    <Phone type="mobile">555-1214</Phone>
    <Phone type="work">555-1213</Phone>
    <Phone type="home">555-1215</Phone>
  </Person>
  <Person contactType="Business">
    <Name>Ariel</Name>
    <Phone>555-1234</Phone>
  </Person>
</AddressBook>
```

## 讨论

在前面的解决方案中，您将 `Lee` 的电话号码（即 XML 原始第一个 `Phone` 节点的“文本”部分）从 `555-1212` 更改为 `555-1214`。您还将电话号码的类型（即 `Phone` 节点的一个属性）从 `"home"` 更改为 `"mobile"`。

向 XML 添加新信息也几乎同样容易。要向 XML 文件添加信息，您需要将其作为另一个节点的*子节点*添加到文件中。获取该子节点的最简单方法是编写表示 XML 的字符串，然后从中创建一个临时的 PowerShell XML 文档。从该临时文档中，您可以使用主 XML 文档的 `ImportNode()` 函数导入您关心的节点——具体来说，就是本例中的 `Phone` 节点。

---

### 交叉引用

- [配方 10.2，“对 XML 执行 XPath 查询”](#ch10.xhtml#perform_an_xpath_query_against_an_xml_fi)
- [配方 10.4，“修改 XML 文件中的数据”](#ch10.xhtml#modify_data_in_an_xml_file)
- [配方 12.1，“从 FTP 或 Internet 站点下载文件”](#ch12.xhtml#download_a_file_from_the_internet)
- [表 F-11](#app06.xhtml#xml-id001)
