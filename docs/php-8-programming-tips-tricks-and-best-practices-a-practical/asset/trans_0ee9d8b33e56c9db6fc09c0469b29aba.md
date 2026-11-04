
4. 最后，我们转向迄今为止最复杂的方法，该方法用于扫描无效的魔术方法签名。主要问题是方法签名差异很大，因此我们需要构建单独的正则表达式来正确测试有效性。正则表达式存储在 BC 断裂配置文件中。如果检测到魔术方法，我们会检索其正确的签名并将其添加到 $this->messages。

5. 首先，我们通过查找匹配 function __ 的任何内容来检查是否存在魔术方法：

```
public function scanMagicSignatures() : int {
    $found  = 0;
    $matches = [];
    $result  = preg_match_all(
        '/function __(.+?)\b/',
        $this->contents, $matches);
```

6. 如果匹配数组不为空，我们遍历匹配集合并将魔术方法名赋给 $key：

```
if (!empty($matches[1])) {
    $config = $this->config[self::KEY_MAGIC] ?? NULL;
    foreach ($matches[1] as $name) {
        $key = '__' . $name;
```

7. 如果与此推定的魔术方法匹配的配置键未设置，我们假设它要么不是魔术方法，要么是配置文件中不存在的方法，因此无需担心。否则，如果存在键，我们提取一个表示方法调用的子字符串并将其赋给 $sub：

```
if (empty($config[$key])) continue;
if ($pos = strpos($this->contents, $key)) {
    $end = strpos($this->contents,
        '{', $pos);
    $sub = (empty($sub) || !is_string($sub))
        ? '' : trim($sub);
```

8. 然后我们从配置中提取正则表达式并将其与子字符串进行匹配。该模式代表该特定魔术方法的正确签名。如果 preg_match() 返回 FALSE，我们就知道实际签名不正确，并将其标记为潜在的 BC 断裂。我们检索并存储警告消息并递增 $found：

```
```php
$ptn = $config[$key]['regex'] ?? '/.*/';
if (!preg_match($ptn, $sub)) {
    $this->messages[] = sprintf(
        self::ERR_MAGIC_SIGNATURE, $key);
    $this->messages[] =
        $config[$key]['signature']
        ?? 'Check signature';
    $found++;
}}}}
if ($found === 0)
    $this->messages[] = sprintf(
        self::OK_PASSED, __FUNCTION__);
return $found;
}
```
```

这就结束了我们对 BreakScan 类的检查。现在我们将注意力转向定义运行 BreakScan 类中编程的扫描所需的调用程序。

## 构建 BreakScan 类调用程序

调用 BreakScan 类的程序的主要任务是接受路径参数并递归构建位于该路径中的 PHP 文件列表。然后我们遍历列表，逐个提取每个文件的内容并运行 BC 断裂扫描。最后，我们会根据选择的详细程度呈现简略或详细的报告。

请记住，我们即将讨论的 BreakScan 类和调用程序都设计为在 PHP 7 下运行。我们不使用 PHP 8 的原因是我们假设开发人员希望在进行 PHP 8 更新之前运行 BC 断裂扫描器：

1. 我们首先配置自动加载器，并从命令行 ($argv) 或 URL ($_GET) 获取路径和详细程度级别。此外，我们提供将结果写入 CSV 文件的选项，并接受此类文件的名称作为参数。你可能会注意到，我们还执行了一定程度的输入清理，尽管理论上 BC 断裂扫描器只会在开发服务器上由开发人员直接使用：

```
```php
// /repo/ch11/php7_bc_break_scanner.php
define('DEMO_PATH', __DIR__);
require __DIR__ . '/../src/Server/Autoload/Loader.php';
$loader = new \Server\Autoload\Loader();
use Php8\Migration\BreakScan;
// some code not shown
$path = $_GET['path'] ?? $argv[1] ?? NULL;
$show = $_GET['show'] ?? $argv[2] ?? 0;
$show = (int) $show;
$csv  = $_GET['csv'] ?? $argv[3] ?? '';
$csv  = basename($csv);
```
```

2. 接下来我们确认路径。如果未找到，我们将退出并显示使用信息（$usage 未显示）：

```
```php
if (empty($path)) {
    if (!empty($_SERVER['REQUEST_URI']))
        echo '<pre>' . $usage . '</pre>';
    else
        echo $usage;
    exit;
}
```
```

3. 然后我们获取 BC 断裂配置文件并创建 BreakScan 实例：

```
```php
$config = include __DIR__
    . '/php8_bc_break_scanner_config.php';
$scanner = new BreakScan($config);
```
```

4. 为了构建文件列表，我们使用 RecursiveDirectoryIterator，并将其包装在 RecursiveIteratorIterator 中，从给定路径开始。然后该列表由 FilterIterator 过滤，仅将扫描限制为 PHP 文件：

```
```php
$iter = new RecursiveIteratorIterator(
    new RecursiveDirectoryIterator($path));
$filter = new class ($iter) extends FilterIterator {
    public function accept() {
        $obj = $this->current();
        return ($obj->getExtension() === 'php');
    }
};
```
```

5. 如果开发人员选择 CSV 选项，则会创建 SplFileObject 实例。同时，我们写出一个标题数组。此外，我们定义一个匿名函数来写入 CSV 文件：

```
```php
if ($csv) {
    $csv_file = new SplFileObject($csv, 'w');
    $csv_file->fputcsv(
        ['Directory','File','OK','Messages']);
}
$write = function ($dir, $fn, $found, $messages)
    use ($csv_file) {
    $ok = ($found === 0) ? 1 : 0;
    $csv_file->fputcsv([$dir, $fn, $ok, $messages]);
    return TRUE;
};
```
```

6. 我们通过遍历 FilterIterator 实例提供的文件列表来启动扫描。在逐个文件扫描时，每次循环 $found 都会被清零。我们确实会维护 $total，以便在最后给出潜在 BC 断裂的总数。你可能还注意到我们区分文件和目录。如果目录发生变化，其名称将作为标题显示：

```
$dir = '';
$total = 0;
foreach ($filter as $name => $obj) {
    $found = 0;
    $scanner->clearMessages();
    if (dirname($name) !== $dir) {
        $dir = dirname($name);
        echo "Processing Directory: $name\n";
    }
}
```

7. 我们使用 SplFileObject::isDir() 来确定文件列表中的项目是否为目录。如果是，则继续处理列表中的下一个项目。然后我们将文件内容推送到 $scanner 并运行所有扫描。消息随后以字符串形式检索：

```
if ($obj->isDir()) continue;
$fn = basename($name);
$scanner->getFileContents($name);
$found = $scanner->runAllScans();
$messages = implode("\n", $scanner->getMessages());
```

8. 我们使用 switch() 块根据 $show 表示的显示级别采取行动。级别 0 仅显示发现潜在 BC 断裂的文件。级别 1 显示这些文件加上消息。级别 2 显示所有可能的输出，包括成功消息：

```
switch ($show) {
    case 2:
        echo "Processing: $fn\n";
        echo "$messages\n";
        if ($csv)
            $write($dir, $fn, $found, $messages);
        break;
    case 1:
        if (!$found) break;
```

```
echo "Processing: $fn\n";
echo BreakScan::WARN_BC_BREAKS . "\n";
printf(BreakScan::TOTAL_BREAKS, $found);
echo "$messages\n";
if ($csv)
    $write($dir, $fn, $found, $messages);
break;
case 0 :
default :
    if (!$found) break;
    echo "Processing: $fn\n";
    echo BreakScan::WARN_BC_BREAKS . "\n";
    if ($csv)
        $write($dir, $fn, $found, $messages);
}
```

9. 最后，我们累积总数并显示最终结果：

```
$total += $found;
}
echo "\n" . str_repeat('-', 40) . "\n";
echo "\nTotal number of possible BC breaks: $total\n";
```

现在你对调用方式有了一个大致的了解，让我们来看看一次测试扫描的结果。

## 扫描应用程序文件

为了演示目的，在本书附带的源代码中，我们包含了一个较旧版本的 phpLdapAdmin。你可以在 /path/to/repo/sample_data/phpldapadmin-1.2.3 找到源代码。对于本次演示，我们打开了 PHP 7 容器的 shell 并运行了以下命令：

```
root@php8_tips_php7 [ /repo ]#
php ch11/php7_bc_break_scanner.php \
    sample_data/phpldapadmin-1.2.3/ 1 |less
```
