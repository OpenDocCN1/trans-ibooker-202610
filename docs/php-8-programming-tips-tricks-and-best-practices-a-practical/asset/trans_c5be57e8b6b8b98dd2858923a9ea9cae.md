
因此，使用命名参数，以下函数调用在 PHP 8 中是可以接受的：

```php
// /repo/ch01/php8_named_args.php
// not all code is shown
$rotation = range(40, -40, 10);
foreach ($rotation as $key => $offset) {
    $char->writeFill();
    [$x, $y] = RotateText::calcXYadjust($char, $offset);
    $angle = ($offset > 0) ? $offset : 360 + $offset;
    imagettftext(
        angle         : $angle,
        color         : $char->fgColor,
        font_filename : FONT_FILE,
        image         : $char->image,
        size          : 60,
        x             : $x,
        y             : $y,
        text          : $char->text);
    $fn = IMG_DIR . '/' . $baseFn . '_' . $key . '.png';
    imagepng($char->image, $fn);
    $images[] = basename($fn);
}
```

上面展示的代码示例将一串失真的字符写入一组 PNG 图像文件。每个字符相对于其相邻图像顺时针旋转 10 度。请注意如何应用命名参数使 imagettftext() 函数的参数更易于理解。

命名参数也可以应用于你自己创建的函数和方法。在下一节中，我们将介绍新的数据类型。

> 提示
> 命名参数的详细分析可以在这里找到：
> https://wiki.php.net/rfc/named_params

## 探索新的数据类型

任何初级 PHP 开发者都会学习 PHP 可用的 **数据类型** 有哪些以及如何使用它们。基本数据类型包括 `int`（整数）、`float`、`bool`（布尔）和 `string`。复杂数据类型包括 `array` 和 `object`。此外，还有其他数据类型，例如 `NULL` 和 `resource`。在本节中，我们讨论 PHP 8 中引入的几种新的数据类型，包括联合类型和 mixed 类型。

> **重要说明**
> 极其重要的是不要将 **数据类型** 与 **数据格式** 混淆。本节描述数据类型。而数据格式则是一种用于 *表示* 数据的方式。数据格式的示例包括 XML、**JavaScript 对象表示法 (JSON)** 和 **YAML 不是标记语言 (YAML)**。

### 联合类型

与 `int` 或 `string` 等其他数据类型不同，需要注意的是，并没有明确称为 `union` 的数据类型。相反，当你看到对 **联合类型** 的引用时，其含义是 PHP 8 引入了一种新的语法，允许你指定类型的联合，而不仅仅是一个。现在让我们来看一下联合类型的通用语法。

### 联合类型语法

联合类型的通用语法如下：

```
function ( type|type|type $var) {}
```

用 type 代替，你可以提供任何现有的数据类型（例如 float 或 string）。不过有一些限制，大多是完全合理的。下表总结了更重要的限制：

| 不允许 | 不允许的示例 |
|---|---|
| void 不能包含在联合类型中 | void|int |
| mixed 不能包含在联合类型中（下面会详细介绍） | mixed|int |
| 可空类型不能包含在联合类型中 | string|?array |
| 不能同时包含 TRUE 和 FALSE 在联合类型中。 | TRUE|FALSE |
| 不允许冗余类型 | int|float|int array|iterable|Traversable |

表 1.3 – 不允许的联合类型

从这个例外列表可以看出，定义联合类型主要是一个常识问题。

> 提示
> **最佳实践：** 使用联合类型时，**类型强制转换**（PHP 在内部将数据类型转换为满足函数要求的过程）如果未强制严格类型检查可能会成为问题。因此，最佳实践是在使用联合类型的任何文件顶部添加以下内容：`declare(strict_types=1);`
> 
> 有关更多信息，请参阅此处的文档参考：
> https://www.php.net/manual/en/language.types.declarations.php#language.types.declarations.strict

### 联合类型示例

为了简单说明，让我们回到本章中用作示例的 SingleChar 类。其中一个方法是 colorAlloc()。此方法从图像分配颜色，利用 imagecolorallocate() 函数。它接受代表红、绿、蓝的整数值作为参数。

假设第一个参数实际上可以是一个数组，表示三个值——每个分别代表红、绿、蓝。在这种情况下，第一个值的参数类型不能是 int，否则如果提供了数组，在开启严格类型检查时将抛出错误。

在 PHP 的早期版本中，唯一的解决方案是移除第一个参数的任何类型检查，并在关联的 DocBlock 中说明接受多种类型。以下是该方法在 PHP 7 中可能出现的样子：

```php
/**
 * Allocates a color resource
 *
 * @param array|int $r
 * @param int $g
 * @param int $b
 * @return int $color
 */
public function colorAlloc($r, $g = 0, $b = 0) {
    if (is_array($r)) {
        [$r, $g, $b] = $r;
    }
    return \imagecolorallocate($this->image, $r, $g, $b);
}
```

第一个参数 $r 的数据类型的唯一指示是 @param array|int $r DocBlock 注释以及该参数没有关联的数据类型提示这一事实。在 PHP 8 中，利用联合类型，请注意这里的区别：

```
#[description("Allocates a color resource")]
#[param("int|array r")]
#[int("g")]
#[int("b")]
```

```php
#[returns("int")]
public function colorAlloc(
    int|array $r, int $g = 0, int $b = 0) {
    if (is_array($r)) {
        [$r, $g, $b] = $r;
    }
    return \imagecolorallocate($this->image, $r, $g, $b);
}
```

在前面的示例中，除了存在指示第一个参数可以接受 array 或 int 类型 的属性外，在方法签名本身中，int|array 联合类型也明确了这一选择。

### mixed 类型

mixed 是 PHP 8 中引入的另一种新类型。与联合类型不同，mixed 是一个实际的数据类型，表示类型的终极联合。它用于表示接受任何和所有数据类型。在某种意义上，PHP 已经具备此功能：只需完全省略数据类型，即为隐含的 mixed 类型！

> 提示
> 你将在 PHP 文档中看到对 mixed 类型的引用。PHP 8 通过将其作为实际数据类型来正式化这种表示。

### 为什么使用 mixed 类型？

稍等一下——你可能在此时会想：为什么还要使用 mixed 类型呢？为了让你安心，这是一个很好的问题，而且没有令人信服的理由使用此类型。

然而，通过在函数或方法签名中使用 mixed，你清楚地表明了对该参数使用的意图。如果你只是将数据类型留空，后来使用或审查你代码的其他开发者可能会认为你忘记添加类型。至少，他们会对未类型化参数的性质感到不确定。

### mixed 类型对继承的影响

由于 mixed 类型代表了放宽的终极示例，它可用于在一个类扩展另一个类时放宽数据类型定义。以下是使用 mixed 类型的示例，说明了这一原则：

- 首先，我们按如下方式定义具有更严格的 object 数据类型的父类：

// /repo/ch01/php8_mixed_type.php
declare(strict_types=1);
class High {
    const LOG_FILE = __DIR__ . '../data/test.log';
    protected static function logVar(object $var) {
        $item = date('Y-m-d') . ':'
            . var_export($var, TRUE);
        return error_log($item, 3, self::LOG_FILE);
    }
}
- 接下来，我们按如下方式定义扩展 High 的 Low 类：

```
class Low extends High {
    public static function logVar(mixed $var) {
        $item = date('Y-m-d') . ':'
            . var_export($var, TRUE);
        return error_log($item, 3, self::LOG_FILE);
    }
}
```

请注意在 Low 类中，logVar() 方法的数据类型已被放宽为 mixed。

- 最后，我们创建 Low 的一个实例并使用测试数据执行它。如以下代码片段所示的结果，你可以看到一切正常：

```
if (file_exists(High::LOG_FILE)) unlink(High::LOG_FILE)
$test = [
    'array' => range('A', 'F'),
    'func' => function () { return __CLASS__; },
    'anon' => new class () {
```
