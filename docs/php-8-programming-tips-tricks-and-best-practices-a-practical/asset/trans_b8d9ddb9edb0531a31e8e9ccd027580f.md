
自 PHP 8 起，SimpleXMLElement 类现在实现了标准 PHP 库（SPL）RecursiveIterator 接口，并包含了 SimpleXMLIterator 类的功能。在 PHP 8 中，SimpleXMLIterator 现在是 SimpleXMLElement 的一个空扩展。考虑到 XML 常常用于表示复杂的树形结构数据，这一看似简单的更新具有重要意义。

例如，请查看温莎王朝家族树的局部视图，如下所示：

![](img/dbf2674d5b31107c344104b5edcb21c5_271_0.png)

图 7.1 – 复杂树形结构数据的示例

如果我们用 XML 来建模，它可能看起来像这样：

```
<?xml version="1.0" encoding="UTF-8"?>
<!-- /repo/ch07/tree.xml -->
<family>
    <branch name="Windsor">
        <descendent gender="M">George V</descendent>
<spouse gender="F">Mary of Teck</spouse>
<branch name="George V">
    <descendent gender="M">George VI</descendent>
    <spouse gender="F">Elizabeth Bowes-Lyon</spouse>
    <branch name="George VI">
        <descendent gender="F">Elizabeth II</descendent>
        <spouse gender="M">Prince Philip</spouse>
        <branch name="Elizabeth II">
            <descendent gender="M">Prince Charles</descendent>
            <spouse gender="F">Diana Spencer</spouse>
            <spouse gender="F">Camilla Parker Bowles</spouse>
            <branch name="Prince Charles">
                <descendent gender="M">William</descendent>
                <spouse gender="F">Kate Middleton</spouse>
            </branch>
            <!-- not all nodes are shown -->
        </branch>
    </branch>
</branch>
</branch>
</family>
```

然后我们开发代码来解析树。然而在 PHP 8 之前的版本中，我们需要定义一个递归函数来解析整个树。为此，我们将遵循以下步骤：

- 1. 我们首先定义一个递归函数，用于显示后代的姓名和配偶（如有），如下面的代码片段所示。该函数还会识别后代的性别并检查是否存在子嗣。如果为真，则函数会再次调用自身：

```
function recurse($branch) {
    foreach ($branch as $node) {
        echo $node->descendent;
        echo ($node->descendent['gender'] == 'F')
            ? ', daughter of '
            : ', son of ';
        echo $node['name'];
        if (empty($node->spouse)) echo "\n";
        else echo ", married to {$node->spouse}\n";
        if (!empty($node->branch))
            recurse($node->branch);
    }
}
```

- 2. 然后我们从外部 XML 文件创建一个 SimpleXMLElement 实例并调用递归函数，如下所示：

```
// /repo/ch07/php7_simple_xml.php
$fn = __DIR__ . './includes/tree.xml';
$xml = simplexml_load_file($fn);
recurse($xml);
```

此代码块在 PHP 7 和 PHP 8 中均可运行。以下是 PHP 7 下的输出：

```
root@php8_tips_php7 [ /repo/ch07 ]#
php php7_simple_xml.php
George V, son of Windsor, married to Mary of Teck
George VI, son of George V, married to Elizabeth Bowes-Lyon
Elizabeth II, daughter of George VI, married to Philip
Prince Charles, son of Elizabeth II, married to Diana Spencer
William, son of Prince Charles, married to Kate Middleton
Harry, son of Prince Charles, married to Meghan Markle
Princess Anne, daughter of Elizabeth II, married to M. Phillips
Princess Margaret, daughter of George VI, married to A. Jones
Edward VIII, son of George V, married to Wallis Simpson
Princess Mary, daughter of George V, married to H.Lascelles
Prince Henry, son of George V, married to Lady Alice Montagu
Prince George, son of George V, married to Princess Marina
Prince John, son of George V
```

然而在 PHP 8 中，由于 SimpleXMLElement 现在实现了 RecursiveIterator，产生相同结果的代码更简单。

- 3. 与前面示例一样，我们从外部文件定义一个 SimpleXMLElement 实例。然而无需定义递归函数——我们只需要定义一个 RecursiveIteratorIterator 实例，如下所示：

```php
// /repo/ch07/php8_simple_xml.php
$fn = __DIR__ . '/includes/tree.xml';
$xml = simplexml_load_file($fn);
$iter = new RecursiveIteratorIterator($xml,
    RecursiveIteratorIterator::SELF_FIRST);
```

- 4. 之后，我们只需要一个简单的 foreach() 循环，其内部逻辑与前一个示例相同。无需检查是否存在分支节点，也无需递归——这些都由 RecursiveIteratorIterator 实例处理！所需的代码如下所示：

```php
foreach ($iter as $branch) {
    if (!empty($branch->descendent)) {
        echo $branch->descendent;
        echo ($branch->descendent['gender'] == 'F')
            ? ', daughter of '
            : ', son of ';
        echo $branch['name'];
        if (empty($branch->spouse)) echo "\n";
        else echo ", married to {$branch->spouse}\n";
    }
}
```

此代码示例在 PHP 8 下的输出如下所示。如你所见，输出完全相同：

```
root@php8_tips_php8 [ /repo/ch07 ]# php php8_simple_xml.php
George V, son of Windsor, married to Mary of Teck
George VI, son of George V, married to Elizabeth Bowes-Lyon
Elizabeth II, daughter of George VI, married to Philip
Prince Charles, son of Elizabeth II, married to Diana Spencer
William, son of Prince Charles, married to Kate Middleton
Harry, son of Prince Charles, married to Meghan Markle
Princess Anne, daughter of Elizabeth II, married to M. Phillips
Princess Margaret, daughter of George VI, married to A. Jones
Edward VIII, son of George V, married to Wallis Simpson
Princess Mary, daughter of George V, married to H.Lascelles
Prince Henry, son of George V, married to Lady Alice Montagu
Prince George, son of George V, married to Princess Marina
Prince John, son of George V
```

> 重要提示
> 请注意，使用 Docker 容器运行这些示例时，此处显示的输出已略作修改以适应页面宽度。

现在让我们来看一下其他 XML 扩展的更改。

## 了解其他 XML 扩展更改

PHP 8 的其他 XML 扩展也有一些更改。大多数情况下，这些更改很小，不会造成显著的向后兼容代码中断的潜在风险。然而，如果我们不讨论这些额外更改就疏忽了。我们建议您浏览本小节中的剩余更改，以便提高您的认识。使用这些 XML 扩展将使您能够排查在 PHP 8 更新后行为不一致的应用程序代码。

### 对 libxml 扩展的更改

**libxml** 扩展利用了 **Expat C 库**，提供了各种 PHP XML 扩展使用的 XML 解析函数（https://libexpat.github.io/）。

服务器上安装的 libxml 版本有新的要求。运行 PHP 8 时的最低版本必须为 2.9.0（或更高）。此更新要求的主要好处之一是提高对 **XML 外部实体** (**XXE**) 处理攻击的防护。

推荐的 libxml 最低版本默认禁用了依赖于 libxml 扩展的 PHP XML 扩展加载外部 XML 实体的能力。这反过来减少了为防止 XXE 攻击而采取昂贵且耗时的额外步骤的需求。

> **提示**
> 有关 XXE 攻击的更多信息，请使用以下链接咨询 **开放式 Web 应用程序安全项目** (**OWASP**)：https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing.

### 对 XMLReader 扩展的更改

`XMLReader` 扩展补充了 `XMLWriter` 扩展。`XMLWriter` 扩展旨在生成 XML 文档，而 `XMLReader` 扩展旨在读取。

两个方法 `XMLReader::open()` 和 `XMLReader::xml()` 现在被定义为 **静态方法**。您仍然可以创建 `XMLReader` 实例，但如果您扩展 `XMLReader` 并重写其中任何一个方法，请确保将其声明为静态。

### 对 XMLParser 扩展的更改

`XMLParser` 扩展是 PHP 最古老的 XML 扩展之一。因此，它几乎完全由过程函数组成，而不是类和方法。然而在 PHP 8 中，该扩展遵循了从资源转向对象的趋势。因此，当您运行 `xml_parser_create()` 或 `xml_parser_create_ns()` 时，会创建一个 `XMLParser` 实例而不是资源。

如 *涉及 is_resource() 的潜在代码中断* 部分所述，您只需将使用 `is_resource()` 的任何检查替换为 `!empty()` 即可。资源到对象迁移的另一个副作用是使 `xml_parser_free()` 函数变得多余。要停用解析器，只需使用 `XMLParser` 对象即可。

现在您已经了解了与 XML 扩展相关的更改，这将帮助您更高效地解析和管理 XML 数据。通过利用本节中提到的新功能，您可以编写比 PHP 8 之前更高效、性能更好的代码。现在让我们来看一下 `mbstring` 扩展。
