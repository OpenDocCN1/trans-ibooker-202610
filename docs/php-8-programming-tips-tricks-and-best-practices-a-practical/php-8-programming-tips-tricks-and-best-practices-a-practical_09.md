## 9 Mastering PHP 8 Best Practices

在本章中，您将了解当前在 PHP 8 中强制执行的最佳实践。我们将介绍几个重要的方法签名更改以及它们的新用法如何延续 PHP 的总体趋势，帮助您生成更好的代码。我们还将看看私有方法、接口、trait 和匿名类用法的变化。最后，我们将讨论命名空间解析中的重要更改。

掌握本章将介绍的最佳实践不仅会让您朝着编写更好的代码迈进，还会帮助您避免因未能掌握这些新实践而可能出现的代码中断。此外，本章中讨论的技术将帮助您编写比过去更高效的代码。

在本章中，我们将介绍以下主题：

- Discovering method signature changes
- Dealing with private methods
- Working with interfaces and traits
- Controlling anonymous class usage
- Understanding changes in namespaces

## Technical requirements

要检查和运行本章提供的代码示例，推荐的最低硬件配置如下：

- X86_64-based desktop PC or laptop
- 1 GB free disk space
- 4 GB of RAM
- 500 Kbps or faster internet connection

此外，您需要安装以下软件：

- Docker
- Docker Compose

请参阅第 1 章《Introducing New PHP 8 OOP Features》的 Technical requirements 部分，了解有关如何安装 Docker 和 Docker Compose 以及如何构建本书中用于演示代码的 Docker 容器的更多信息。在本书中，我们将您恢复示例代码的目录称为 /repo。

本章的源代码位于此处：https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices。我们将首先检查重要的[content]
## Dealing with changes to backtraces

令人惊讶的是，在 PHP 8 之前，可以通过回溯更改函数参数。这之所以可能，是因为由 debug_backtrace() 或 Exception::getTrace() 生成的跟踪通过引用提供了对函数参数的访问。

这是一种极其糟糕的做法，因为它允许程序在可能处于错误状态的情况下继续运行。此外，在审查此类代码时，参数数据是如何提供的并不清楚。因此，在 PHP 8 中，不再允许这种做法。debug_backtrace() 或 Exception::getTrace() 仍然像以前一样运行。唯一的区别是它们不再通过引用传递参数变量。

现在让我们来看看 PDO 错误处理的更改。

## PDO error-handling mode default changed

多年来，当使用 PDO 扩展的数据库应用程序未能产生结果时，初级 PHP 开发者感到困惑。在许多情况下，此问题的原因是未报告的简单 SQL 语法错误。这是由于在 PHP 8 之前的版本中，默认的 PDO 错误模式为 PDO::ERRMODE_SILENT。

SQL 错误不是 PHP 错误。因此，此类错误不会被正常的 PHP 错误处理捕获。相反，PHP 开发者必须专门将 PDO 错误模式设置为 PDO::ERRMODE_WARNING 或 PDO::ERRMODE_EXCEPTION。PHP 开发者现在可以松一口气了，因为从 PHP 8 开始，PDO 默认错误处理模式现在是 PDO::ERRMODE_EXCEPTION。

在下面的示例中，PHP 7 允许不正确的 SQL 语句静默失败：

```php
// /repo/ch08/php7_pdo_err_mode.php
$dsn = 'mysql:host=localhost;dbname=php8_tips';
$pdo = new PDO($dsn, 'php8', 'password');
$sql = 'SELECT propertyKey, hotelName FROM hotels '
    ."WHERE country = 'CA'";
$stmt = $pdo->query($sql);
if ($stmt)
    while($hotel = $stmt->fetch(PDO::FETCH_OBJ))
        echo $hotel->hotelName . ' ' . $hotel->propertyKey . "\n";
else
    echo "No Results\n";
```

在 PHP 7 中，唯一输出是 No Results，这既具有误导性又没有帮助。它可能会导致开发者认为没有结果，而实际上问题是 SQL 语法错误。

此处显示的在 PHP 8 中运行的输出更有帮助：

```
root@php8_tips_php8 [ /repo/ch08 ]# php php7_pdo_err_mode.php
PHP Fatal error: Uncaught PDOException: SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'SELECT propertyKey, hotelName FROM hotels WHERE country = 'CA'' at line 1 in /repo/ch08/php7_pdo_err_mode.php:10
```

从前面的 PHP 8 输出中可以看到，实际问题已得到明确识别。

> TIP
> 有关此更改的更多信息，请参阅此 RFC：
> https://wiki.php.net/rfc/pdo_default_errmode

接下来我们检查 track_errors php.ini 指令。

## Examining the track_errors php.ini setting

从 PHP 8 开始，track_errors php.ini 指令已被移除。这意味着自动创建的变量 $php_errormsg 不再可用。在大多数情况下，PHP 8 之前导致错误的东西现在已被转换为抛出 Error 消息。但是，对于 PHP 8 之前的版本，您仍然可以使用 error_get_last() 函数代替。

在下面的简单代码示例中，我们首先打开 track_errors 指令。然后我们在没有任何参数的情况下调用 strpos()，故意导致错误。然后我们依赖 $php_errormsg 来揭示真正的错误：

```
// /repo/ch08/php7_track_errors.php
ini_set('track_errors', 1);
@strpos();
echo $php_errormsg . "\n";
echo "OK\n";
```

以下是 PHP 7 中的输出：

```
root@php8_tips_php7 [ /repo/ch08 ]# php php7_track_errors.php
strpos() expects at least 2 parameters, 0 given
OK
```

从前面的输出可以看到，$php_errormsg 揭示了错误，并且代码块被允许继续。在 PHP 8 中，当然我们不允许在没有任何参数的情况下调用 strpos()。以下是输出：

```
root@php8_tips_php8 [ /repo/ch08 ]# php php7_track_errors.php
PHP Fatal error: Uncaught ArgumentCountError: strpos() expects at least 2 arguments, 0 given in /repo/ch08/php7_track_errors.php:5
```

正如您所见，PHP 8 抛出 Error 消息。最佳做法是使用 try/catch 块并捕获可能抛出的任何 Error 消息。您也可以使用 error_get_last() 函数。以下是重写后的示例，可在 PHP 7 和 PHP 8 中运行（未显示输出）：

```
// /repo/ch08/php8_track_errors.php
try {
    strpos();
    echo error_get_last()['message'];
    echo "\nOK\n";
} catch (Error $e) {
    echo $e->getMessage() . "\n";
}
```

您现在对 PHP 8 中已弃用或移除的功能有了一些了解。本章到此结束。

## Summary

在本章中，您了解了已弃用和移除的 PHP 功能。本章的第一部分涉及已移除的核心功能。更改的理由已得到解释，您了解到本章中描述的功能被移除的主要原因不仅是为了让您转向遵循最佳实践的代码，而且是为了让您使用更快、更高效的 PHP 8 功能。

在下一部分，您了解了已弃用的功能。本节的关键主题是强调已弃用的函数、类和方法如何导致不良实践和漏洞百出的代码。您还获得了有关在许多关键 PHP 8 扩展中已被移除或弃用的功能的指导。

您学习了如何定位和重写已弃用的代码，以及如何为已移除的功能开发变通方法。本章中学习的另一项技能包括如何重构使用涉及扩展的已移除功能的代码，最后但并非最不重要的是，您学习了通过重写依赖于已移除函数的代码来提高应用程序安全性。

在下一章中，您将通过掌握最佳实践来学习如何在 PHP 8 代码中获得更高的效率和性能。

## Section 3: PHP 8 Best Practices

在本节中，您将了解 PHP 8 最佳实践。您将了解 PHP 8 如何引入了许多额外的控制来强制执行某些最佳实践。此外，还涵盖了在 PHP 8 中编写代码的最佳方式。

本节包含以下章节：

- *第 9 章，掌握 PHP 8 最佳实践*
- *第 10 章，改进性能*
- *第 11 章，将现有 PHP 应用迁移到 PHP 8*
- *第 12 章，使用异步编程创建 PHP 8 应用程序*

## 9 Mastering PHP 8 Best Practices

在本章中，您将了解当前在 PHP 8 中强制执行的最佳实践。我们将介绍几个重要的[content]
## 处理回溯的更改

令人惊讶的是，在 PHP 8 之前，可以通过回溯更改函数参数。这之所以可能，是因为由 debug_backtrace() 或 Exception::getTrace() 生成的跟踪通过引用提供了对函数参数的访问。

这是一种极其糟糕的做法，因为它允许程序在可能处于错误状态的情况下继续运行。此外，在审查此类代码时，参数数据是如何提供的并不清楚。因此，在 PHP 8 中，不再允许这种做法。debug_backtrace() 或 Exception::getTrace() 仍然像以前一样运行。唯一的区别是它们不再通过引用传递参数变量。

现在让我们来看看 PDO 错误处理的更改。

## PDO error-handling mode default changed

多年来，当使用 PDO 扩展的数据库应用程序未能产生结果时，初级 PHP 开发者感到困惑。在许多情况下，此问题的原因是未报告的简单 SQL 语法错误。这是由于在 PHP 8 之前的版本中，默认的 PDO 错误模式为 PDO::ERRMODE_SILENT。

SQL 错误不是 PHP 错误。因此，此类错误不会被正常的 PHP 错误处理捕获。相反，PHP 开发者必须专门将 PDO 错误模式设置为 PDO::ERRMODE_WARNING 或 PDO::ERRMODE_EXCEPTION。PHP 开发者现在可以松一口气了，因为从 PHP 8 开始，PDO 默认错误处理模式现在是 PDO::ERRMODE_EXCEPTION。

在下面的示例中，PHP 7 允许不正确的 SQL 语句静默失败：

```php
// /repo/ch08/php7_pdo_err_mode.php
$dsn = 'mysql:host=localhost;dbname=php8_tips';
$pdo = new PDO($dsn, 'php8', 'password');
$sql = 'SELECT propertyKey, hotelName FROM hotels '
    ."WHERE country = 'CA'";
$stmt = $pdo->query($sql);
if ($stmt)
    while($hotel = $stmt->fetch(PDO::FETCH_OBJ))
        echo $hotel->hotelName . ' ' . $hotel->propertyKey . "\n";
else
    echo "No Results\n";
```

在 PHP 7 中，唯一输出是 No Results，这既具有误导性又没有帮助。它可能会导致开发者认为没有结果，而实际上问题是 SQL 语法错误。

此处显示的在 PHP 8 中运行的输出更有帮助：

```
root@php8_tips_php8 [ /repo/ch08 ]# php php7_pdo_err_mode.php
PHP Fatal error: Uncaught PDOException: SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'SELECT propertyKey, hotelName FROM hotels WHERE country = 'CA'' at line 1 in /repo/ch08/php7_pdo_err_mode.php:10
```

从前面的 PHP 8 输出中可以看到，实际问题已得到明确识别。

> TIP
> 有关此更改的更多信息，请参阅此 RFC：
> https://wiki.php.net/rfc/pdo_default_errmode

接下来我们检查 track_errors php.ini 指令。

## Examining the track_errors php.ini setting

从 PHP 8 开始，track_errors php.ini 指令已被移除。这意味着自动创建的变量 $php_errormsg 不再可用。在大多数情况下，PHP 8 之前导致错误的东西现在已被转换为抛出 Error 消息。但是，对于 PHP 8 之前的版本，您仍然可以使用 error_get_last() 函数代替。

在下面的简单代码示例中，我们首先打开 track_errors 指令。然后我们在没有任何参数的情况下调用 strpos()，故意导致错误。然后我们依赖 $php_errormsg 来揭示真正的错误：

```
// /repo/ch08/php7_track_errors.php
ini_set('track_errors', 1);
@strpos();
echo $php_errormsg . "\n";
echo "OK\n";
```

以下是 PHP 7 中的输出：

```
root@php8_tips_php7 [ /repo/ch08 ]# php php7_track_errors.php
strpos() expects at least 2 parameters, 0 given
OK
```

从前面的输出可以看到，$php_errormsg 揭示了错误，并且代码块被允许继续。在 PHP 8 中，当然我们不允许在没有任何参数的情况下调用 strpos()。以下是输出：

```
root@php8_tips_php8 [ /repo/ch08 ]# php php7_track_errors.php
PHP Fatal error: Uncaught ArgumentCountError: strpos() expects at least 2 arguments, 0 given in /repo/ch08/php7_track_errors.php:5
```

正如您所见，PHP 8 抛出 Error 消息。最佳做法是使用 try/catch 块并捕获可能抛出的任何 Error 消息。您也可以使用 error_get_last() 函数。以下是重写后的示例，可在 PHP 7 和 PHP 8 中运行（未显示输出）：

```
// /repo/ch08/php8_track_errors.php
try {
    strpos();
    echo error_get_last()['message'];
    echo "\nOK\n";
} catch (Error $e) {
    echo $e->getMessage() . "\n";
}
```

您现在对 PHP 8 中已弃用或移除的功能有了一些了解。本章到此结束。

## Summary

在本章中，您了解了已弃用和移除的 PHP 功能。本章的第一部分涉及已移除的核心功能。更改的理由已得到解释，您了解到本章中描述的功能被移除的主要原因不仅是为了让您转向遵循最佳实践的代码，而且是为了让您使用更快、更高效的 PHP 8 功能。

在下一部分，您了解了已弃用的功能。本节的关键主题是强调已弃用的函数、类和方法如何导致不良实践和漏洞百出的代码。您还获得了有关在许多关键 PHP 8 扩展中已被移除或弃用的功能的指导。

您学习了如何定位和重写已弃用的代码，以及如何为已移除的功能开发变通方法。本章中学习的另一项技能包括如何重构使用涉及扩展的已移除功能的代码，最后但并非最不重要的是，您学习了通过重写依赖于已移除函数的代码来提高应用程序安全性。

在下一章中，您将通过掌握最佳实践来学习如何在 PHP 8 代码中获得更高的效率和性能。

## Section 3: PHP 8 Best Practices

在本节中，您将了解 PHP 8 最佳实践。您将了解 PHP 8 如何引入了许多额外的控制来强制执行某些最佳实践。此外，还涵盖了在 PHP 8 中编写代码的最佳方式。

本节包含以下章节：

- *第 9 章，掌握 PHP 8 最佳实践*
- *第 10 章，改进性能*
- *第 11 章，将现有 PHP 应用迁移到 PHP 8*
- *第 12 章，使用异步编程创建 PHP 8 应用程序*

## 9 Mastering PHP 8 Best Practices

在本章中，您将了解当前在 PHP 8 中强制执行的最佳实践。我们将介绍几个重要的[content]
## 处理回溯的更改

令人惊讶的是，在 PHP 8 之前，可以通过回溯更改函数参数。这之所以可能，是因为由 debug_backtrace() 或 Exception::getTrace() 生成的跟踪通过引用提供了对函数参数的访问。

这是一种极其糟糕的做法，因为它允许程序在可能处于错误状态的情况下继续运行。此外，在审查此类代码时，参数数据是如何提供的并不清楚。因此，在 PHP 8 中，不再允许这种做法。debug_backtrace() 或 Exception::getTrace() 仍然像以前一样运行。唯一的区别是它们不再通过引用传递参数变量。

现在让我们来看看 PDO 错误处理的更改。

## PDO error-handling mode default changed

多年来，当使用 PDO 扩展的数据库应用程序未能产生结果时，初级 PHP 开发者感到困惑。在许多情况下，此问题的原因是未报告的简单 SQL 语法错误。这是由于在 PHP 8 之前的版本中，默认的 PDO 错误模式为 PDO::ERRMODE_SILENT。

SQL 错误不是 PHP 错误。因此，此类错误不会被正常的 PHP 错误处理捕获。相反，PHP 开发者必须专门将 PDO 错误模式设置为 PDO::ERRMODE_WARNING 或 PDO::ERRMODE_EXCEPTION。PHP 开发者现在可以松一口气了，因为从 PHP 8 开始，PDO 默认错误处理模式现在是 PDO::ERRMODE_EXCEPTION。

在下面的示例中，PHP 7 允许不正确的 SQL 语句静默失败：

```php
// /repo/ch08/php7_pdo_err_mode.php
$dsn = 'mysql:host=localhost;dbname=php8_tips';
$pdo = new PDO($dsn, 'php8', 'password');
$sql = 'SELECT propertyKey, hotelName FROM hotels '
    ."WHERE country = 'CA'";
$stmt = $pdo->query($sql);
if ($stmt)
    while($hotel = $stmt->fetch(PDO::FETCH_OBJ))
        echo $hotel->hotelName . ' ' . $hotel->propertyKey . "\n";
else
    echo "No Results\n";
```

在 PHP 7 中，唯一输出是 No Results，这既具有误导性又没有帮助。它可能会导致开发者认为没有结果，而实际上问题是 SQL 语法错误。

此处显示的在 PHP 8 中运行的输出更有帮助：

```
root@php8_tips_php8 [ /repo/ch08 ]# php php7_pdo_err_mode.php
PHP Fatal error: Uncaught PDOException: SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'SELECT propertyKey, hotelName FROM hotels WHERE country = 'CA'' at line 1 in /repo/ch08/php7_pdo_err_mode.php:10
```

从前面的 PHP 8 输出中可以看到，实际问题已得到明确识别。

> TIP
> 有关此更改的更多信息，请参阅此 RFC：
> https://wiki.php.net/rfc/pdo_default_errmode

接下来我们检查 track_errors php.ini 指令。

## Examining the track_errors php.ini setting

从 PHP 8 开始，track_errors php.ini 指令已被移除。这意味着自动创建的变量 $php_errormsg 不再可用。在大多数情况下，PHP 8 之前导致错误的东西现在已被转换为抛出 Error 消息。但是，对于 PHP 8 之前的版本，您仍然可以使用 error_get_last() 函数代替。

在下面的简单代码示例中，我们首先打开 track_errors 指令。然后我们在没有任何参数的情况下调用 strpos()，故意导致错误。然后我们依赖 $php_errormsg 来揭示真正的错误：

```
// /repo/ch08/php7_track_errors.php
ini_set('track_errors', 1);
@strpos();
echo $php_errormsg . "\n";
echo "OK\n";
```

以下是 PHP 7 中的输出：

```
root@php8_tips_php7 [ /repo/ch08 ]# php php7_track_errors.php
strpos() expects at least 2 parameters, 0 given
OK
```

从前面的输出可以看到，$php_errormsg 揭示了错误，并且代码块被允许继续。在 PHP 8 中，当然我们不允许在没有任何参数的情况下调用 strpos()。以下是输出：

```
root@php8_tips_php8 [ /repo/ch08 ]# php php7_track_errors.php
PHP Fatal error: Uncaught ArgumentCountError: strpos() expects at least 2 arguments, 0 given in /repo/ch08/php7_track_errors.php:5
```

正如您所见，PHP 8 抛出 Error 消息。最佳做法是使用 try/catch 块并捕获可能抛出的任何 Error 消息。您也可以使用 error_get_last() 函数。以下是重写后的示例，可在 PHP 7 和 PHP 8 中运行（未显示输出）：

```
// /repo/ch08/php8_track_errors.php
try {
    strpos();
    echo error_get_last()['message'];
    echo "\nOK\n";
} catch (Error $e) {
    echo $e->getMessage() . "\n";
}
```

您现在对 PHP 8 中已弃用或移除的功能有了一些了解。本章到此结束。

## Summary

在本章中，您了解了已弃用和移除的 PHP 功能。本章的第一部分涉及已移除的核心功能。更改的理由已得到解释，您了解到本章中描述的功能被移除的主要原因不仅是为了让您转向遵循最佳实践的代码，而且是为了让您使用更快、更高效的 PHP 8 功能。

在下一部分，您了解了已弃用的功能。本节的关键主题是强调已弃用的函数、类和方法如何导致不良实践和漏洞百出的代码。您还获得了有关在许多关键 PHP 8 扩展中已被移除或弃用的功能的指导。

您学习了如何定位和重写已弃用的代码，以及如何为已移除的功能开发变通方法。本章中学习的另一项技能包括如何重构使用涉及扩展的已移除功能的代码，最后但并非最不重要的是，您学习了通过重写依赖于已移除函数的代码来提高应用程序安全性。

在下一章中，您将通过掌握最佳实践来学习如何在 PHP 8 代码中获得更高的效率和性能。

## Section 3: PHP 8 Best Practices

在本节中，您将了解 PHP 8 最佳实践。您将了解 PHP 8 如何引入了许多额外的控制来强制执行某些最佳实践。此外，还涵盖了在 PHP 8 中编写代码的最佳方式。

本节包含以下章节：

- *第 9 章，掌握 PHP 8 最佳实践*
- *第 10 章，改进性能*
- *第 11 章，将现有 PHP 应用迁移到 PHP 8*
- *第 12 章，使用异步编程创建 PHP 8 应用程序*

## 9 Mastering PHP 8 Best Practices

在本章中，您将了解当前在 PHP 8 中强制执行的最佳实践。我们将介绍几个重要的方法签名更改以及它们的新用法如何延续 PHP 的总体趋势，帮助您生成更好的代码。我们还将看看私有方法、接口、trait 和匿名类用法的变化。最后，我们将讨论命名空间解析中的重要更改。

掌握本章将介绍的最佳实践不仅会让您朝着编写更好的代码迈进，还会帮助您避免因未能掌握这些新实践而可能出现的代码中断。此外，本章中讨论的技术将帮助您编写比过去更高效的代码。

在本章中，我们将介绍以下主题：

- 发现方法签名更改
- 处理私有方法
- 使用接口和 trait
- 控制匿名类用法
- 理解命名空间中的更改

## Technical requirements

要检查和运行本章提供的代码示例，推荐的最低硬件配置如下：

- 基于 X86_64 的台式电脑或笔记本电脑
- 1 GB 可用磁盘空间
- 4 GB RAM
- 500 Kbps 或更快的互联网连接

此外，您需要安装以下软件：

- Docker
- Docker Compose

请参阅第 1 章《Introducing New PHP 8 OOP Features》的 Technical requirements 部分，了解有关如何安装 Docker 和 Docker Compose 以及如何构建本书中用于演示代码的 Docker 容器的更多信息。在本书中，我们将您恢复示例代码的目录称为 /repo。

本章的源代码位于此处：https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices。我们将首先检查重要的方法签名更改。

## Discovering method signature changes

在 PHP 8 中引入了若干方法签名更改。如果您的代码扩展了本节中描述的任何类或实现了任何方法，了解这些签名更改非常重要。只要您了解这些更改，您的代码将正常运行，从而减少错误。

在 PHP 8 中引入的签名更改反映了更新后的最佳实践。因此，如果您编写使用正确方法签名的代码，您就是遵循这些最佳实践。我们将通过回顾 PHP 8 对魔术方法签名的更改开始讨论。

## Managing magic method signatures

在 PHP 8 中，魔术方法的定义和使用已朝着标准化迈出了重要一步。这是通过引入以严格参数和返回数据类型形式存在的精确魔术方法签名来实现的。与 PHP 8 中看到的大多数改进一样，此更新是为了防止魔术方法的误用。总体结果是代码更好，错误更少。



此增强功能的缺点是，如果你的代码提供了不正确的参数或返回值类型，则会抛出 Error。另一方面，如果你的代码确实提供了正确参数数据类型和返回值类型，或者你的代码根本不使用参数或返回值数据类型，则此增强功能不会产生任何不利影响。

以下代码块总结了 PHP 8 及以上版本中魔术方法的新的参数和返回值数据类型：

```
__call(string $name, array $arguments): mixed;
__callStatic(string $name, array $arguments): mixed;
__clone(): void;
__debugInfo(): ?array;
__get(string $name): mixed;
__invoke(mixed $arguments): mixed;
__isset(string $name): bool;
__serialize(): array;
__set(string $name, mixed $value): void;
__set_state(array $properties): object;
__sleep(): array;
__unserialize(array $data): void;
__unset(string $name): void;
__wakeup(): void;
```

现在，让我们来看三个简单的示例，说明魔术方法签名更改的影响：

- 1. 第一个示例涉及 NoTypes 类，该类定义了 __call()，但未定义任何数据类型：

```
// /repo/ch09/php8_bc_break_magic.php
class NoTypes {
    public function __call($name, $args) {
        return "Attempt made to call '$name' "
            . "with these arguments: '"
            . implode(',', $args) . "'\n";
    }
}
$no = new NoTypes();
echo $no->doesNotExist('A','B','C');
```

- 2. 以下示例（与前一个示例在同一文件中）是 MixedTypes 类，该类定义了 __invoke()，但使用的是 array 数据类型而不是 mixed：

```
class MixedTypes {
    public function __invoke(array $args) : string {
        return "Arguments: "
            . implode(',', $args) . "\n";
    }
}
$mixed= new MixedTypes();
echo $mixed(['A','B','C']);
```

以下是前面步骤中所示代码示例的 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

而在 PHP 8 下运行相同的代码示例：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

正如你所见，两个输出集是相同的。所示的第一个类 NoTypes 可以工作，因为没有定义数据类型提示。有趣的是，MixedTypes 类在 PHP 8 及以下版本中都能工作，因为新的 mixed 数据类型实际上是所有类型的联合。因此，你可以安全地使用任何特定数据类型代替 mixed。

- 3. 在我们的最终示例中，我们将定义 WrongType 类。在此类中，我们将使用与 PHP 8 要求不匹配的返回数据类型定义一个名为 __isset() 的魔术方法。在这里，我们使用的是 string，而在 PHP 8 中，其返回类型需要是 bool：

```
// /repo/ch09/php8_bc_break_magic_wrong.php
class WrongType {
    public function __isset($var) : string {
        return (isset($this->$var)) ? 'Y' : '';
    }
}
$wrong = new WrongType();
echo (isset($wrong->nothing)) ? 'Set' : 'Not Set';
```

此示例在 PHP 7 中有效，因为它依赖于如果变量未设置则返回空字符串的事实，然后将其插值为空 FALSE 布尔值。以下是 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
Not Set
```

然而在 PHP 8 中，由于魔术方法签名现在已标准化，示例失败，如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
PHP Fatal error: WrongType::__isset(): Return type must be bool when declared in /repo/ch09/php8_bc_break_magic_wrong.php on line 6
```

正如你所见，PHP 8 严格执行其魔术方法签名。

> 提示
> 最佳实践：修改使用魔术方法的任何代码，以遵循新的严格方法签名。有关严格魔术方法签名的更多信息，请访问 https://wiki.php.net/rfc/magic-methods-signature.

你现在已经有了需要查找的内容以及如何纠正涉及魔术方法的潜在代码破坏的思路。现在，让我们来看 Reflection 扩展方法签名更改。

## 检查 Reflection 方法签名更改

向后兼容的代码破坏可能会发生，如果你的应用程序使用 invoke() 或 newInstance() Reflection 扩展方法。在 PHP 7 及以下版本中，接下来的三个方法都接受无限数量的参数。然而，在方法签名中，只列出了一个参数，如下所示：

- `ReflectionClass::newInstance($args)`
- `ReflectionFunction::invoke($args)`
- `ReflectionMethod::invoke($args)`

在 PHP 8 中，方法签名准确地反映了实际情况，即 $args 前面加上了可变参数运算符。以下是新的方法签名：

- `ReflectionClass::newInstance(...$args)`
- `ReflectionFunction::invoke(...$args)`
- `ReflectionMethod::invoke($object, ...$args)`

此更改仅在你有一个自定义类扩展了这三个类中的任何一个，并且该自定义类还重写了前面项目符号列表中列出的三个方法中的任何一个时才会破坏你的代码。

最后，isBuiltin() 方法已从 ReflectionType 移动到 ReflectionNamedType。如果使用 ReflectionType::isBuiltin()，则会带来潜在的代码破坏。

现在，让我们来看 PDO 扩展中的方法签名更改。

## 处理 PDO 扩展签名更改

PDO 扩展有两个重要的[content]
此增强功能的缺点是，如果你的代码提供了不正确的参数或返回值类型，则会抛出 Error。另一方面，如果你的代码确实提供了正确参数数据类型和返回值类型，或者你的代码根本不使用参数或返回值数据类型，则此增强功能不会产生任何不利影响。

以下代码块总结了 PHP 8 及以上版本中魔术方法的新的参数和返回值数据类型：

```
__call(string $name, array $arguments): mixed;
__callStatic(string $name, array $arguments): mixed;
__clone(): void;
__debugInfo(): ?array;
__get(string $name): mixed;
__invoke(mixed $arguments): mixed;
__isset(string $name): bool;
__serialize(): array;
__set(string $name, mixed $value): void;
__set_state(array $properties): object;
__sleep(): array;
__unserialize(array $data): void;
__unset(string $name): void;
__wakeup(): void;
```

现在，让我们来看三个简单的示例，说明魔术方法签名更改的影响：

- 1. 第一个示例涉及 NoTypes 类，该类定义了 __call()，但未定义任何数据类型：

```
// /repo/ch09/php8_bc_break_magic.php
class NoTypes {
    public function __call($name, $args) {
        return "Attempt made to call '$name' "
            . "with these arguments: '"
            . implode(',', $args) . "'\n";
    }
}
$no = new NoTypes();
echo $no->doesNotExist('A','B','C');
```

- 2. 以下示例（与前一个示例在同一文件中）是 MixedTypes 类，该类定义了 __invoke()，但使用的是 array 数据类型而不是 mixed：

```
class MixedTypes {
    public function __invoke(array $args) : string {
        return "Arguments: "
            . implode(',', $args) . "\n";
    }
}
$mixed= new MixedTypes();
echo $mixed(['A','B','C']);
```

以下是前面步骤中所示代码示例的 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

而在 PHP 8 下运行相同的代码示例：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

正如你所见，两个输出集是相同的。所示的第一个类 NoTypes 可以工作，因为没有定义数据类型提示。有趣的是，MixedTypes 类在 PHP 8 及以下版本中都能工作，因为新的 mixed 数据类型实际上是所有类型的联合。因此，你可以安全地使用任何特定数据类型代替 mixed。

- 3. 在我们的最终示例中，我们将定义 WrongType 类。在此类中，我们将使用与 PHP 8 要求不匹配的返回数据类型定义一个名为 __isset() 的魔术方法。在这里，我们使用的是 string，而在 PHP 8 中，其返回类型需要是 bool：

```
// /repo/ch09/php8_bc_break_magic_wrong.php
class WrongType {
    public function __isset($var) : string {
        return (isset($this->$var)) ? 'Y' : '';
    }
}
$wrong = new WrongType();
echo (isset($wrong->nothing)) ? 'Set' : 'Not Set';
```

此示例在 PHP 7 中有效，因为它依赖于如果变量未设置则返回空字符串的事实，然后将其插值为空 FALSE 布尔值。以下是 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
Not Set
```

然而在 PHP 8 中，由于魔术方法签名现在已标准化，示例失败，如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
PHP Fatal error: WrongType::__isset(): Return type must be bool when declared in /repo/ch09/php8_bc_break_magic_wrong.php on line 6
```

正如你所见，PHP 8 严格执行其魔术方法签名。

> 提示
> 最佳实践：修改使用魔术方法的任何代码，以遵循新的严格方法签名。有关严格魔术方法签名的更多信息，请访问 https://wiki.php.net/rfc/magic-methods-signature.

你现在已经有了需要查找的内容以及如何纠正涉及魔术方法的潜在代码破坏的思路。现在，让我们来看 Reflection 扩展方法签名更改。

## 检查 Reflection 方法签名更改

向后兼容的代码破坏可能会发生，如果你的应用程序使用 invoke() 或 newInstance() Reflection 扩展方法。在 PHP 7 及以下版本中，接下来的三个方法都接受无限数量的参数。然而，在方法签名中，只列出了一个参数，如下所示：

- `ReflectionClass::newInstance($args)`
- `ReflectionFunction::invoke($args)`
- `ReflectionMethod::invoke($args)`

在 PHP 8 中，方法签名准确地反映了实际情况，即 $args 前面加上了可变参数运算符。以下是新的方法签名：

- `ReflectionClass::newInstance(...$args)`
- `ReflectionFunction::invoke(...$args)`
- `ReflectionMethod::invoke($object, ...$args)`

此更改仅在你有一个自定义类扩展了这三个类中的任何一个，并且该自定义类还重写了前面项目符号列表中列出的三个方法中的任何一个时才会破坏你的代码。

最后，isBuiltin() 方法已从 ReflectionType 移动到 ReflectionNamedType。如果使用 ReflectionType::isBuiltin()，则会带来潜在的代码破坏。

现在，让我们来看 PDO 扩展中的方法签名更改。

## 处理 PDO 扩展签名更改

PDO 扩展有两个重要的[content]
此增强功能的缺点是，如果你的代码提供了不正确的参数或返回值类型，则会抛出 Error。另一方面，如果你的代码确实提供了正确参数数据类型和返回值类型，或者你的代码根本不使用参数或返回值数据类型，则此增强功能不会产生任何不利影响。

以下代码块总结了 PHP 8 及以上版本中魔术方法的新的参数和返回值数据类型：

```
__call(string $name, array $arguments): mixed;
__callStatic(string $name, array $arguments): mixed;
__clone(): void;
__debugInfo(): ?array;
__get(string $name): mixed;
__invoke(mixed $arguments): mixed;
__isset(string $name): bool;
__serialize(): array;
__set(string $name, mixed $value): void;
__set_state(array $properties): object;
__sleep(): array;
__unserialize(array $data): void;
__unset(string $name): void;
__wakeup(): void;
```

现在，让我们来看三个简单的示例，说明魔术方法签名更改的影响：

- 1. 第一个示例涉及 NoTypes 类，该类定义了 __call()，但未定义任何数据类型：

```
// /repo/ch09/php8_bc_break_magic.php
class NoTypes {
    public function __call($name, $args) {
        return "Attempt made to call '$name' "
            . "with these arguments: '"
            . implode(',', $args) . "'\n";
    }
}
$no = new NoTypes();
echo $no->doesNotExist('A','B','C');
```

- 2. 以下示例（与前一个示例在同一文件中）是 MixedTypes 类，该类定义了 __invoke()，但使用的是 array 数据类型而不是 mixed：

```
class MixedTypes {
    public function __invoke(array $args) : string {
        return "Arguments: "
            . implode(',', $args) . "\n";
    }
}
$mixed= new MixedTypes();
echo $mixed(['A','B','C']);
```

以下是前面步骤中所示代码示例的 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

而在 PHP 8 下运行相同的代码示例：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

正如你所见，两个输出集是相同的。所示的第一个类 NoTypes 可以工作，因为没有定义数据类型提示。有趣的是，MixedTypes 类在 PHP 8 及以下版本中都能工作，因为新的 mixed 数据类型实际上是所有类型的联合。因此，你可以安全地使用任何特定数据类型代替 mixed。

- 3. 在我们的最终示例中，我们将定义 WrongType 类。在此类中，我们将使用与 PHP 8 要求不匹配的返回数据类型定义一个名为 __isset() 的魔术方法。在这里，我们使用的是 string，而在 PHP 8 中，其返回类型需要是 bool：

```
// /repo/ch09/php8_bc_break_magic_wrong.php
class WrongType {
    public function __isset($var) : string {
        return (isset($this->$var)) ? 'Y' : '';
    }
}
$wrong = new WrongType();
echo (isset($wrong->nothing)) ? 'Set' : 'Not Set';
```

此示例在 PHP 7 中有效，因为它依赖于如果变量未设置则返回空字符串的事实，然后将其插值为空 FALSE 布尔值。以下是 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
Not Set
```

然而在 PHP 8 中，由于魔术方法签名现在已标准化，示例失败，如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
PHP Fatal error: WrongType::__isset(): Return type must be bool when declared in /repo/ch09/php8_bc_break_magic_wrong.php on line 6
```

正如你所见，PHP 8 严格执行其魔术方法签名。

> 提示
> 最佳实践：修改使用魔术方法的任何代码，以遵循新的严格方法签名。有关严格魔术方法签名的更多信息，请访问 https://wiki.php.net/rfc/magic-methods-signature.

你现在已经有了需要查找的内容以及如何纠正涉及魔术方法的潜在代码破坏的思路。现在，让我们来看 Reflection 扩展方法签名更改。

## 检查 Reflection 方法签名更改

向后兼容的代码破坏可能会发生，如果你的应用程序使用 invoke() 或 newInstance() Reflection 扩展方法。在 PHP 7 及以下版本中，接下来的三个方法都接受无限数量的参数。然而，在方法签名中，只列出了一个参数，如下所示：

- `ReflectionClass::newInstance($args)`
- `ReflectionFunction::invoke($args)`
- `ReflectionMethod::invoke($args)`

在 PHP 8 中，方法签名准确地反映了实际情况，即 $args 前面加上了可变参数运算符。以下是新的方法签名：

- `ReflectionClass::newInstance(...$args)`
- `ReflectionFunction::invoke(...$args)`
- `ReflectionMethod::invoke($object, ...$args)`

此更改仅在你有一个自定义类扩展了这三个类中的任何一个，并且该自定义类还重写了前面项目符号列表中列出的三个方法中的任何一个时才会破坏你的代码。

最后，isBuiltin() 方法已从 ReflectionType 移动到 ReflectionNamedType。如果使用 ReflectionType::isBuiltin()，则会带来潜在的代码破坏。

现在，让我们来看 PDO 扩展中的方法签名更改。

## 处理 PDO 扩展签名更改

PDO 扩展有两个重要的[content]
此增强功能的缺点是，如果你的代码提供了不正确的参数或返回值类型，则会抛出 Error。另一方面，如果你的代码确实提供了正确参数数据类型和返回值类型，或者你的代码根本不使用参数或返回值数据类型，则此增强功能不会产生任何不利影响。

以下代码块总结了 PHP 8 及以上版本中魔术方法的新的参数和返回值数据类型：

```
__call(string $name, array $arguments): mixed;
__callStatic(string $name, array $arguments): mixed;
__clone(): void;
__debugInfo(): ?array;
__get(string $name): mixed;
__invoke(mixed $arguments): mixed;
__isset(string $name): bool;
__serialize(): array;
__set(string $name, mixed $value): void;
__set_state(array $properties): object;
__sleep(): array;
__unserialize(array $data): void;
__unset(string $name): void;
__wakeup(): void;
```

现在，让我们来看三个简单的示例，说明魔术方法签名更改的影响：

- 1. 第一个示例涉及 NoTypes 类，该类定义了 __call()，但未定义任何数据类型：

```
// /repo/ch09/php8_bc_break_magic.php
class NoTypes {
    public function __call($name, $args) {
        return "Attempt made to call '$name' "
            . "with these arguments: '"
            . implode(',', $args) . "'\n";
    }
}
$no = new NoTypes();
echo $no->doesNotExist('A','B','C');
```

- 2. 以下示例（与前一个示例在同一文件中）是 MixedTypes 类，该类定义了 __invoke()，但使用的是 array 数据类型而不是 mixed：

```
class MixedTypes {
    public function __invoke(array $args) : string {
        return "Arguments: "
            . implode(',', $args) . "\n";
    }
}
$mixed= new MixedTypes();
echo $mixed(['A','B','C']);
```

以下是前面步骤中所示代码示例的 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

而在 PHP 8 下运行相同的代码示例：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

正如你所见，两个输出集是相同的。所示的第一个类 NoTypes 可以工作，因为没有定义数据类型提示。有趣的是，MixedTypes 类在 PHP 8 及以下版本中都能工作，因为新的 mixed 数据类型实际上是所有类型的联合。因此，你可以安全地使用任何特定数据类型代替 mixed。

- 3. 在我们的最终示例中，我们将定义 WrongType 类。在此类中，我们将使用与 PHP 8 要求不匹配的返回数据类型定义一个名为 __isset() 的魔术方法。在这里，我们使用的是 string，而在 PHP 8 中，其返回类型需要是 bool：

```
// /repo/ch09/php8_bc_break_magic_wrong.php
class WrongType {
    public function __isset($var) : string {
        return (isset($this->$var)) ? 'Y' : '';
    }
}
$wrong = new WrongType();
echo (isset($wrong->nothing)) ? 'Set' : 'Not Set';
```

此示例在 PHP 7 中有效，因为它依赖于如果变量未设置则返回空字符串的事实，然后将其插值为空 FALSE 布尔值。以下是 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
Not Set
```

然而在 PHP 8 中，由于魔术方法签名现在已标准化，示例失败，如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
PHP Fatal error: WrongType::__isset(): Return type must be bool when declared in /repo/ch09/php8_bc_break_magic_wrong.php on line 6
```

正如你所见，PHP 8 严格执行其魔术方法签名。

> 提示
> 最佳实践：修改使用魔术方法的任何代码，以遵循新的严格方法签名。有关严格魔术方法签名的更多信息，请访问 https://wiki.php.net/rfc/magic-methods-signature.

你现在已经有了需要查找的内容以及如何纠正涉及魔术方法的潜在代码破坏的思路。现在，让我们来看 Reflection 扩展方法签名更改。

## 检查 Reflection 方法签名更改

向后兼容的代码破坏可能会发生，如果你的应用程序使用 invoke() 或 newInstance() Reflection 扩展方法。在 PHP 7 及以下版本中，接下来的三个方法都接受无限数量的参数。然而，在方法签名中，只列出了一个参数，如下所示：

- `ReflectionClass::newInstance($args)`
- `ReflectionFunction::invoke($args)`
- `ReflectionMethod::invoke($args)`

在 PHP 8 中，方法签名准确地反映了实际情况，即 $args 前面加上了可变参数运算符。以下是新的方法签名：

- `ReflectionClass::newInstance(...$args)`
- `ReflectionFunction::invoke(...$args)`
- `ReflectionMethod::invoke($object, ...$args)`

此更改仅在你有一个自定义类扩展了这三个类中的任何一个，并且该自定义类还重写了前面项目符号列表中列出的三个方法中的任何一个时才会破坏你的代码。

最后，isBuiltin() 方法已从 ReflectionType 移动到 ReflectionNamedType。如果使用 ReflectionType::isBuiltin()，则会带来潜在的代码破坏。

现在，让我们来看 PDO 扩展中的方法签名更改。

## 处理 PDO 扩展签名更改

PDO 扩展有两个重要的[content]
此增强功能的缺点是，如果你的代码提供了不正确的参数或返回值类型，则会抛出 Error。另一方面，如果你的代码确实提供了正确参数数据类型和返回值类型，或者你的代码根本不使用参数或返回值数据类型，则此增强功能不会产生任何不利影响。

以下代码块总结了 PHP 8 及以上版本中魔术方法的新的参数和返回值数据类型：

```
__call(string $name, array $arguments): mixed;
__callStatic(string $name, array $arguments): mixed;
__clone(): void;
__debugInfo(): ?array;
__get(string $name): mixed;
__invoke(mixed $arguments): mixed;
__isset(string $name): bool;
__serialize(): array;
__set(string $name, mixed $value): void;
__set_state(array $properties): object;
__sleep(): array;
__unserialize(array $data): void;
__unset(string $name): void;
__wakeup(): void;
```

现在，让我们来看三个简单的示例，说明魔术方法签名更改的影响：

- 1. 第一个示例涉及 NoTypes 类，该类定义了 __call()，但未定义任何数据类型：

```
// /repo/ch09/php8_bc_break_magic.php
class NoTypes {
    public function __call($name, $args) {
        return "Attempt made to call '$name' "
            . "with these arguments: '"
            . implode(',', $args) . "'\n";
    }
}
$no = new NoTypes();
echo $no->doesNotExist('A','B','C');
```

- 2. 以下示例（与前一个示例在同一文件中）是 MixedTypes 类，该类定义了 __invoke()，但使用的是 array 数据类型而不是 mixed：

```
class MixedTypes {
    public function __invoke(array $args) : string {
        return "Arguments: "
            . implode(',', $args) . "\n";
    }
}
$mixed= new MixedTypes();
echo $mixed(['A','B','C']);
```

以下是前面步骤中所示代码示例的 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

而在 PHP 8 下运行相同的代码示例：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic.php
Attempt made to call 'doesNotExist' with these arguments:
'A,B,C'
Arguments: 'A,B,C'
```

正如你所见，两个输出集是相同的。所示的第一个类 NoTypes 可以工作，因为没有定义数据类型提示。有趣的是，MixedTypes 类在 PHP 8 及以下版本中都能工作，因为新的 mixed 数据类型实际上是所有类型的联合。因此，你可以安全地使用任何特定数据类型代替 mixed。

- 3. 在我们的最终示例中，我们将定义 WrongType 类。在此类中，我们将使用与 PHP 8 要求不匹配的返回数据类型定义一个名为 __isset() 的魔术方法。在这里，我们使用的是 string，而在 PHP 8 中，其返回类型需要是 bool：

```
// /repo/ch09/php8_bc_break_magic_wrong.php
class WrongType {
    public function __isset($var) : string {
        return (isset($this->$var)) ? 'Y' : '';
    }
}
$wrong = new WrongType();
echo (isset($wrong->nothing)) ? 'Set' : 'Not Set';
```

此示例在 PHP 7 中有效，因为它依赖于如果变量未设置则返回空字符串的事实，然后将其插值为空 FALSE 布尔值。以下是 PHP 7 输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
Not Set
```

然而在 PHP 8 中，由于魔术方法签名现在已标准化，示例失败，如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_bc_break_magic_wrong.php
PHP Fatal error: WrongType::__isset(): Return type must be bool when declared in /repo/ch09/php8_bc_break_magic_wrong.php on line 6
```

正如你所见，PHP 8 严格执行其魔术方法签名。

> 提示
> 最佳实践：修改使用魔术方法的任何代码，以遵循新的严格方法签名。有关严格魔术方法签名的更多信息，请访问 https://wiki.php.net/rfc/magic-methods-signature.

你现在已经有了需要查找的内容以及如何纠正涉及魔术方法的潜在代码破坏的思路。现在，让我们来看 Reflection 扩展方法签名更改。

## 检查 Reflection 方法签名更改

向后兼容的代码破坏可能会发生，如果你的应用程序使用 invoke() 或 newInstance() Reflection 扩展方法。在 PHP 7 及以下版本中，接下来的三个方法都接受无限数量的参数。然而，在方法签名中，只列出了一个参数，如下所示：

- `ReflectionClass::newInstance($args)`
- `ReflectionFunction::invoke($args)`
- `ReflectionMethod::invoke($args)`

在 PHP 8 中，方法签名准确地反映了实际情况，即 $args 前面加上了可变参数运算符。以下是新的方法签名：

- `ReflectionClass::newInstance(...$args)`
- `ReflectionFunction::invoke(...$args)`
- `ReflectionMethod::invoke($object, ...$args)`

此更改仅在你有一个自定义类扩展了这三个类中的任何一个，并且该自定义类还重写了前面项目符号列表中列出的三个方法中的任何一个时才会破坏你的代码。

最后，isBuiltin() 方法已从 ReflectionType 移动到 ReflectionNamedType。如果使用 ReflectionType::isBuiltin()，则会带来潜在的代码破坏。

现在，让我们来看 PDO 扩展中的方法签名更改。

## 处理 PDO 扩展签名更改

PDO 扩展有两个重要的[content]
方法签名更改非常重要。这些更改是为了解决在应用不同的 **获取模式** 时方法调用不一致的问题。以下是 PDO::query() 的新方法签名：

```
PDO::query(string $query,
    ?int $fetchMode = null, mixed ...$fetchModeArgs)
```

这是 PDOStatement::setFetchMode() 的新签名：

```php
PDOStatement::setFetchMode(int $mode, mixed ...$args)
```

> 提示
>
> PDO::query() 方法签名更改在 PHP 7.4 到 PHP 8 迁移指南中有所提及，这里：https://www.php.net/manual/en/migration80.incompatible.php#migration80.incompatible.pdo.

两个新的方法签名比旧签名更统一，并且完全涵盖了使用不同获取模式时的语法差异。一个简单的代码示例演示了为什么需要规范化方法签名，该示例使用两种不同的获取模式执行 PDO::query()：

- 1. 让我们首先包含一个包含数据库连接参数的配置文件。从中，我们将创建一个 PDO 实例：

```php
// /repo/ch09/php8_pdo_signature_change.php
$config = include __DIR__ . '../src/config/config.php';
$db_cfg = $config['db-config'];
$pdo    = new PDO($db_cfg['dsn'], $db_cfg['usr'], $db_cfg['pwd']);
```

- 2. 现在，让我们定义一个 SQL 语句并发送它以便准备：

```php
$sql    = 'SELECT hotelName, city, locality, '
    . 'country, postalCode FROM hotels '
    . 'WHERE country = ? AND city = ?';
$stmt   = $pdo->prepare($sql);
```

- 3. 接下来，我们将执行准备好的语句并将获取模式设置为 PDO::FETCH_ASSOC。请注意，当我们使用此获取模式时，只向 setFetchMode() 方法提供了一个参数：

```php
$stmt->execute(['IN', 'Budhera']);
$stmt->setFetchMode(PDO::FETCH_ASSOC);
while ($row = $stmt->fetch()) var_dump($row);
```

- 4. 最后，我们将再次执行相同的准备好的语句。这次，我们将获取模式设置为 PDO::FETCH_CLASS。请注意，当我们使用此获取模式时，向 setFetchMode() 方法提供了两个参数：

```
$stmt->execute(['IN', 'Budhera']);
$stmt->setFetchMode(
    PDO::FETCH_CLASS, ArrayObject::class);
while ($row = $stmt->fetch()) var_dump($row);
```

第一个查询的输出是关联数组。第二个查询生成一个 ArrayObject 实例。以下是输出：

```
root@php8_tips_php8 [/repo/ch09 ]#
php php8_pdo_signature_change.php
array(5) {
    ["hotelName"]=> string(10) "Rose Lodge"
    ["city"]=> string(7) "Budhera"
    ["locality"]=> string(7) "Gurgaon"
    ["country"]=> string(2) "IN"
    ["postalCode"]=> string(6) "122505"
}
object(ArrayObject)#3 (6) {
    ["hotelName"]=> string(10) "Rose Lodge"
    ["city"]=> string(7) "Budhera"
    ["locality"]=> string(7) "Gurgaon"
    ["country"]=> string(2) "IN"
    ["postalCode"]=> string(6) "122505"
    ["storage":"ArrayObject":private]=> array(0) { }
}
```

重要的是要注意，即使方法签名已经更改，你也可以保持现有代码不变：这不会导致向后兼容的代码破坏！

现在，让我们来看声明为静态的方法。

## 处理新定义的静态方法

在 PHP 8 中看到的另一个潜在重大更改是几个方法现在被声明为静态。如果已将此处描述的类和方法作为直接对象实例使用，则没有问题。



以下方法现已声明为静态：

- `tidy::repairString()`
- `tidy::repairFile()`
- `XMLReader::open()`
- `XMLReader::xml()`

如果重写前面提到的类之一，则可能出现代码中断。在这种情况下，您必须将重写的方法声明为静态。下面是一个简单的示例，说明了潜在问题：

- 1. 首先，让我们定义一个 <div> 标签不匹配的字符串：

```php
// /repo/ch08/php7_tidy_repair_str_static.php
$str = <<<EOT
<DIV>
    <Div>Some Content</div>
        <Div>Some Other Content
</div>
EOT;
```

- 2. 然后，定义一个扩展 tidy 的匿名类，修复字符串并返回所有 HTML 标签都小写的字符串：

```php
$class = new class() extends tidy {
    public function repairString($str) {
        $fixed = parent::repairString($str);
        return preg_replace_callback(
            '/<.+?>/',
            function ($item) {
                return strtolower($item); },
            $fixed);
    }
};
```

- 3. 最后，输出修复后的字符串：

```php
echo $class->repairString($str);
```

如果我们在 PHP 7 中运行此代码示例，输出如下：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_tidy_repair_str_static.php
<!DOCTYPE html>
<html>
<head>
<title></title>
</head>
<body>
<div>
<div>Some Content</div>
<div>Some Other Content</div>
</div>
</body>
</html>
```

如您所见，不匹配的 <div> 标签已被修复，并且生成了一个格式正确的 HTML 文档。您还会注意到所有标签都是小写。

然而在 PHP 8 中，会出现方法签名问题，如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_tidy_repair_str_static.php
PHP Fatal error: Cannot make static method tidy::repairString() non static in class tidy@anonymous in /repo/ch09/php7_tidy_repair_str_static.php on line 11
```

如您所见，在 PHP 8 中，repairString() 方法现已声明为静态。我们之前定义的匿名类中 repairString() 的方法签名需要重写，如下所示：

```
public static function repairString(
    string $str,
    array|string|null $config = null,
    ?string $encoding = null) { // etc.
```

重写后（未显示）的输出与前面显示的 PHP 7 输出相同。另外，请注意最后一行现在也可以写成如下形式：

```
echo $class::repairString($str);
```

现在您已经了解了新定义为静态的方法，让我们来看一个相关主题；即静态返回类型。

## 使用静态返回类型

static 关键字在 PHP 中用于多个上下文。其基本用法超出了本次讨论的范围。在本节中，我们将重点关注 static 作为返回数据类型的新的用法。

由于 static 被视为 self 的子类型，因此可以用它来扩展 self 的更窄返回类型。然而，static 关键字不能用作类型提示，因为它会违反里氏替换原则。它还会让开发者感到困惑，因为 static 已在太多其他上下文中使用。

> Tip
> 以下文章描述了 static 返回类型引入之前的背景讨论：https://wiki.php.net/rfc/static_return_type. 以下文档引用了后期静态绑定：https://www.php.net/manual/en/language.oop5.late-static-bindings.php. 里氏替换原则在第 5 章《发现潜在的 OOP 向后兼容性破坏》中讨论过，具体在《理解扩展的 PHP 8 协变支持》部分。

这种新返回数据类型的最常见用途是在使用流畅接口的类中。后者是一种技术，其中对象方法返回当前对象状态的实例，从而允许以流畅（可读）的方式使用方法调用链。在下面的示例中，请注意对象如何构建 SQL SELECT 语句：

- 1. 首先，我们必须定义一个 Where 类，它接受无限数量的参数来形成 SQL WHERE 子句。请注意 static 的返回数据类型：

```php
// /src/Php8/Sql/Where.php
namespace Php8\Sql;
class Where {
    public $where = [];
    public function where(...$args) : static {
        $this->where = array_merge($this->where, $args);
        return $this;
    }
    // not all code is shown
}
```

- 2. 现在，让我们定义主类 Select，它提供用于构建 SQL SELECT 语句各部分的方法。同样请注意，所示方法都返回当前类实例，并且返回数据类型为 static：

```php
// /src/Php8/Sql/Select.php
namespace Php8\Sql;
class Select extends Where {
    public $from = '';
    public $limit = 0;
    public $offset = 0;
    public function from(string $table) : static {
        $this->from = $table;
        return $this;
    }
    public function order(string $order) : static {
        $this->order = $order;
        return $this;
    }
    public function limit(int $num) : static {
        $this->limit = $num;
        return $this;
    }
    // not all methods and properties are shown
}
```

- 3. 最后，我们必须定义一个调用程序，提供构建 SQL 语句所需的值。请注意，echo 语句使用流畅接口，使以编程方式创建 SQL 语句更容易理解：

```php
// /repo/ch09/php8_static_return_type.php
require_once __DIR__
    . './../../src/Server/Autoload/Loader.php';
$loader = new \Server\Autoload\Loader();
use Php8\Sql\Select;
$start = "'2021-06-01'";
$end   = "'2021-12-31'";
$select = new Select();
echo $select->from('events')
    ->cols(['id', 'event_name', 'event_date'])
    ->limit(10)
    ->where('event_date', '>=', $start)
    ->where('AND', 'event_date', '<=', $end)
    ->render();
```

以下是代码示例在 PHP 8 中运行的输出：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_static_return_type.php
SELECT id,event_name,event_date FROM events WHERE event_date >= '2021-06-01' AND event_date <= '2021-12-31' LIMIT 10
```

当然，此示例在 PHP 7 中不起作用，因为 static 关键字不可用作返回数据类型。接下来，让我们看看特殊 ::class 常量的扩展用法。

## 扩展 ::class 常量的使用

特殊的 ::class 常量非常有用，因为它可以静默扩展为完整的命名空间加上类名字符串。了解它的使用方式以及在 PHP 8 中其用法的扩展，可以为您节省大量时间。它的使用还可以使代码更具可读性，尤其是在处理冗长的命名空间和类名时。

特殊的 ::class 常量是作用域解析运算符 (::) 和 class 关键字的组合。然而，与 ::parent、::self 和 ::static 不同，::class 构造可以在类定义之外使用。从某种意义上说，::class 构造是一种魔术常量，因为它会导致与其关联的类神奇地扩展为其完整的命名空间加上类名。

在了解其在 PHP 8 中的用法如何扩展之前，让我们先看看其常规用法。

## 常规 ::class 常量用法

特殊的 `::class` 常量经常在命名空间很长且希望不仅节省不必要的输入，还保留源代码可读性的情况下使用。

在此简单示例中，使用 `Php7\Image\Strategy` 命名空间，我们希望创建一个策略类列表：

- 1. 首先，让我们标识命名空间并设置自动加载器：
- 2. 在引入特殊的 `::class` 常量之前，要生成完整的命名空间类名列表，您必须将其全部写成字符串，如下所示：
- 3. 使用特殊的 `::class` 常量，您可以减少所需的输入量，还可以使代码更具可读性，如下所示：

```php
// /repo/ch09/php7_class_normal.php
namespace Php7\Image\Strategy;
require_once __DIR__
  . './../../src/Server/Autoload/Loader.php';
$autoload = new \Server\Autoload\Loader();
```

```php
$listOld = [
  'Php7\Image\Strategy\DotFill',
  'Php7\Image\Strategy\LineFill',
  'Php7\Image\Strategy\PlainFill',
  'Php7\Image\Strategy\RotateText',
  'Php7\Image\Strategy\Shadow'
];
print_r($listOld);
```

```php
$listNew = [
  DotFill::class,
  LineFill::class,
  PlainFill::class,
  RotateText::class,
  Shadow::class
];
print_r($listNew);
```



如果我们运行此代码示例，就会看到在 PHP 7 和 PHP 8 中这两个列表是相同的。以下是 PHP 7 的输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_class_normal.php
Array (
    [0] => Php7\Image\Strategy\DotFill
    [1] => Php7\Image\Strategy\LineFill
    [2] => Php7\Image\Strategy\PlainFill
    [3] => Php7\Image\Strategy\RotateText
    [4] => Php7\Image\Strategy\Shadow
)
Array (
    [0] => Php7\Image\Strategy\DotFill
    [1] => Php7\Image\Strategy\LineFill
    [2] => Php7\Image\Strategy\PlainFill
    [3] => Php7\Image\Strategy\RotateText
    [4] => Php7\Image\Strategy\Shadow
)
```

如你所见，特殊的 ::class 常量会在编译时将类名扩展为其完整的命名空间加上类名，导致两个列表包含相同的信息。

现在，让我们来看看 PHP 8 中特殊 ::class 常量的使用。

## 扩展的特殊 ::class 常量使用

随着 PHP 8 中所见到的其他语法统一性改进，现在可以在活动对象实例上使用特殊的 ::class 常量。虽然效果与使用 get_class() 相同，但作为从过程式转向面向对象编程的一般最佳实践的一部分，使用特殊的 ::class 常量是有意义的。

在此示例中，扩展的 ::class 语法用于确定抛出的错误类型：

- 1. 当抛出 Error 或 Exception 时，最佳实践是在错误日志中记录一条条目。在此示例中，该示例在 PHP 7 和 PHP 8 中均可工作，Error 或 Exception 的类名包含在日志消息中：

```php
// /repo/ch09/php7_class_and_obj.php
try {
    $pdo = new PDO();
    echo 'No problem';
} catch (Throwable $t) {
    $msg = get_class($t) . ':' . $t->getMessage();
    error_log($msg);
}
```

- 2. 在 PHP 8 中，你可以通过重写示例来实现相同的结果，如下所示：

```php
// /repo/ch09/php8_class_and_obj.php
try {
    $pdo = new PDO();
    echo 'No problem';
} catch (Throwable $t) {
    $msg = $t::class . ':' . $t->getMessage();
    error_log($msg);
}
```

从第二个代码块可以看出，语法更简洁，避免了使用过程式函数。然而，我们必须强调，在此示例中，没有性能提升。

现在你已经了解了特殊 ::class 常量使用的变化，让我们快速看一下逗号。

## 利用尾随逗号

PHP 很早就允许在定义数组时使用尾随逗号。例如，这里所示的语法并不罕见：

```
$arr = [1, 2, 3, 4, 5,];
```

然而，在函数或方法签名中做同样的事情是不允许的：

```
function xyz ($fn, $ln, $mid = '',) { /* code */ }
```

虽然这并不是什么大事，但能够在定义数组时添加尾随逗号，却不允许在函数或方法签名中享有同样的自由，这令人恼火！

PHP 8 现在允许你在函数和方法签名中都使用尾随逗号。新规则也适用于与匿名函数关联的 use () 语句。

为了说明这一变化，请考虑以下示例。在此示例中，定义了一个匿名函数来渲染全名：

// /repo/ch09/php8_trailing_comma.php
$full = function ($fn, $ln, $mid = '',) {
    $mi = ($mid) ? strtoupper($mid[0]) . '. ' : '';
    return $fn . ' ' . $mi . $ln;
};
echo $full('Fred', 'Flintstone', 'John');
```

如你所见，匿名函数的第三个参数后面有一个逗号。以下是 PHP 7 的输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_trailing_comma.php
PHP Parse error: syntax error, unexpected ')', expecting variable (T_VARIABLE) in /repo/ch09/php8_trailing_comma.php on line 4
```

在 PHP 8 中，尾随逗号是允许的，并且预期的输出出现，如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_trailing_comma.php
Fred J. Flintstone
```

虽然在函数或方法定义中拥有尾随逗号不一定是最佳实践，但它确实使 PHP 8 在处理尾随逗号的整体方式上保持一致。

现在，让我们把注意力转向那些仍然存在但不再有任何用途的方法。

## 了解不再需要的方法

主要由于 PHP 8 的资源到对象迁移，多个函数和方法不再需要。截至撰写本文时，它们尚未被弃用，但这些函数不再具有任何实际用途。

作为类比，在 PHP 8 之前的 PHP 版本中，你会使用 fopen() 打开文件句柄资源。完成对文件的操作后，你通常会使用 fclose() 关闭文件句柄资源以关闭连接。

现在，假设你使用 SplFileObject 而不是 fopen()。完成对文件的操作后，你只需 unset 该对象即可。这实现了与使用 fclose() 相同的效果，使 fclose() 变得多余。

下表总结了存在的函数，可以继续使用，但在 PHP 8 中不再具有任何实际价值。带星号的函数也已被弃用：

| Extension | Functions No Longer Useful |
|---|---|
| cURL | curl_close()
 curl_multi_close()
 curl_share_close() |
| Exif | read_exif_data() |
| GD | imagedestroy() |
| OpenSSL | openssl_x509_free() * |
| Shared Memory | shmop_close() * |
| XMLParser | xml_parser_free() |

表 9.1 – 不再有用的函数

现在你已经对 PHP 8 中所做的重大方法签名和使用变化有了一个概念，让我们来看看在处理接口和 trait 时需要考虑的最佳实践。

## 使用接口和 trait

PHP 8 的 trait 实现在多个方面得到了扩展。还有几个新的接口可能会改变你使用 DOM 和 DateTime 扩展的方式。在大多数情况下，这些更改提高了这两个扩展的能力。然而，由于某些情况下方法签名已更改，你可能会遇到潜在的代码中断。因此，密切关注本节中介绍的讨论非常重要，以确保现有和未来的 PHP 代码保持功能正常。

首先，让我们来看看新的 DOM 扩展接口。

### 发现新的 DOM 扩展接口

*生活成本* 经济统计每年由许多世界政府发布。它描绘了普通公民每年生活的成本。随着网络技术的成熟，类似的原则已被应用——首先是 HTML，现在是 DOM。**DOM Living Standard** 由 **Web Hypertext Application Technology Working Group (WHATWG)** (`https://whatwg.org/`) 维护。

这条信息对 PHP 开发者重要的原因是在 PHP 8 中，做出了将 PHP DOM 扩展迁移到 DOM Living Standard 的决定。因此，从 PHP 8 开始，将根据对 living standard 的更改，对该扩展应用一系列增量和持续的更改。

在大多数情况下，这些更改向后兼容。然而，由于某些方法签名更改以保持对标准的遵循，你可能会遇到代码中断。PHP 8 中对 DOM 扩展的最重大更改是引入了两个新的接口。让我们检查这些接口，然后讨论它们对 PHP 开发的影响。

### 检查新的 DOMParentNode 接口

两个新接口中的第一个是 **DOMParentNode**。以下类在 PHP 8 中实现了此接口：

- `DOMDocument`
- `DOMElement`
- `DOMDocumentFragment`

以下是接口定义：

```
interface DOMParentNode {
    public readonly ?DOMElement $firstElementChild;
    public readonly ?DOMElement $lastElementChild;
    public readonly int $childElementCount;
    public function append(
        ...DOMNode|string|null $nodes) : void;
    public function prepend(
        ...DOMNode|string|null $nodes) : void;
}
```

需要注意的是，PHP 开发者无法使用 readonly 属性。然而，接口规范将属性显示为只读，因为它们是内部生成的且无法更改。

> **Tip**
> 实际上在 2014 年提出过一个 PHP RFC，建议为类属性添加 readonly 属性。然而，该提案已被撤回，因为通过定义常量或简单地将属性标记为 private 即可实现相同的效果！有关此提案的更多信息，请参阅 https://wiki.php.net/rfc/readonly_properties.

下表总结了新的 DOMParentNode 接口的属性和方法：

| 方法 / 属性 | 描述 |
|---|---|
| $firstElementChild | 包含此对象中的第一个 DOMElement 实例，或 NULL。 |
| $lastElementChild | 包含此对象中的最后一个 DOMElement 实例，或 NULL。 |
| $elementChildCount | 表示此对象中子元素总数的整数值。 |
| append() | 将节点追加到当前子节点列表的末尾。 |
| prepend() | 将节点添加到当前子节点列表的开头。 |

表 9.2 – DOMParentNode 接口方法和属性



新接口所代表的功能并没有为现有的 DOM 能力增加任何新内容。其主要目的是使 PHP DOM 扩展与实时标准保持一致。

> **Tip**
> 未来对 DOM 扩展进行架构性改造还有另一个目的。在 PHP 的未来版本中，DOM 扩展将具备操作整个 DOM 树分支的能力。例如，未来在发出 `append()` 时，你将能够附加的不仅是单个节点，还包括其所有子节点。更多信息请参阅以下 RFC：https://wiki.php.net/rfc/dom_living_standard_api.

现在，让我们来看第二个新接口。

## 检查新的 DOMChildNode 接口

两个新接口中的第二个是 **DOMChildNode**。在 PHP 8 中，`DOMElement` 和 `DOMCharacterData` 类实现了此接口。

以下是接口定义：

```
interface DOMChildNode {
    public readonly ?DOMElement $previousElementSibling;
    public readonly ?DOMElement $nextElementSibling;
    public function remove() : void;
    public function before(
        ...DOMNode|string|null $nodes) : void;
    public function after(
        ...DOMNode|string|null $nodes) : void;
    public function replaceWith(
        ...DOMNode|string|null $nodes) : void;
}
```

下表总结了 DOMChildNode 的方法和属性：

| Method / Property | Description |
|---|---|
| $previousElementSibling | 包含此对象同一节点层级中的前一个 DOMElement 实例，或 NULL。 |
| $nextElementSibling | 包含此对象同一节点层级中的下一个 DOMElement 实例，或 NULL。 |
| remove() | 删除此节点的简化方式。无需先获取父节点。 |
| before() | 在此对象中于此节点之前添加同一层级的节点。 |
| after() | 在此对象中于此节点之后添加同一层级的节点。 |
| replaceWith() | 用指定的元素替换此节点。 |

表 9.3 – DOMChildNode 接口方法与属性

在这种情况下，功能与现有的 DOM 能力略有不同。最显著的差异是 DOMChildNode::remove()。在 PHP 8 之前，要删除一个节点，必须访问其父节点。假设 $topic 是一个 DOMElement 实例，PHP 7 或更早版本的代码可能如下所示：

```
$topic->parentNode->removeChild($topic);
```

在 PHP 8 中，相同的代码可以写成如下形式：

```
$topic->remove();
```

除了前面两个表中提到的新方法之外，DOM 功能保持不变。现在，让我们看看如何在 PHP 8 中重写移动子节点的操作以利用新接口。

## DOM 使用示例 – 比较 PHP 7 和 PHP 8

为了说明新接口的使用，让我们看一个代码示例。本节将展示一段使用 DOM 扩展将代表 Topic X 的节点从一个文档移动到另一个文档的代码块：

- 1. 这里是一个包含一组嵌套 <div> 标签的 HTML 片段：

```
<!DOCTYPE html>
<!-- /repo/ch09/dom_test_1.html -->
<div id="content">
<div id="A">Topic A</div>
<div id="B">Topic B</div>
<div id="C">Topic C</div>
<div id="X">Topic X</div>
</div>
```

- 2. 第二个 HTML 片段包含主题 D、E 和 F：

```
<!DOCTYPE html>
<!-- /repo/ch09/dom_test_2.html -->
<div id="content">
<div id="D">Topic D</div>
<div id="E">Topic E</div>
<div id="F">Topic F</div>
</div>
```

- 3. 要从两个片段分别创建 DOMDocument 实例，我们可以进行静态调用；即 loadHTMLFile。请注意，此用法在 PHP 7 中已弃用，并在 PHP 8 中被移除：

```
$doc1 = DomDocument::loadHTMLFile( 'dom_test_1.html');
$doc2 = DomDocument::loadHTMLFile('dom_test_2.html');
```

- 4. 然后，我们可以将 Topic X 提取到 $topic 中并将其导入到第二个文档作为 $new。接下来，检索目标节点；即 content：

```
$topic = $doc1->getElementById('X');
$new = $doc2->importNode($topic);
$new->textContent= $topic->textContent;
$main = $doc2->getElementById('content');
```

- 5. 这里是 PHP 7 和 PHP 8 开始不同的地方。在 PHP 7 中，要移动节点，代码必须如下所示：

```
// /repo/ch09/php7_dom_changes.php
$main->appendChild($new);
$topic->parentNode->removeChild($topic);
```

- 6. 然而，在 PHP 8 中使用新接口时，代码更加简洁。在 PHP 8 中删除主题时，无需引用父节点：

```php
// /repo/ch09/php8_dom_changes.php
$main->append($new);
$topic->remove();
```

- 7. 对于 PHP 7 和 PHP 8，我们都可以像这样查看结果的 HTML：

```php
echo $doc1->saveHTML();
echo $doc2->saveHTML();
```

- 8. 另一个区别是如何提取 $main 的新最后一个子元素的值。它在 PHP 7 中可能如下所示：

```php
// /repo/ch09/php7_dom_changes.php
echo $main->lastChild->textContent . "\n";
```

- 9. 而在 PHP 8 中则是这样：

```php
// /repo/ch09/php8_dom_changes.php
echo $main->lastElementChild->textContent . "\n";
```

两个代码示例的输出存在细微差异。在 PHP 7 中，你会看到弃用通知，如下所示：

```
root@php8_tips_php7 [ /repo/ch09 ]# php php7_dom_changes.php
PHP Deprecated: Non-static method DOMDocument::loadHTMLFile() should not be called statically in /repo/ch09/php7_dom_changes.php on line 6
```

如果我们尝试在 PHP 8 中运行 PHP 7 代码，将会抛出致命错误，因为 loadHTMLFile() 方法的静态用法不再允许。否则，如果我们运行纯 PHP 8 示例，输出将如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]# php php8_dom_changes.php
<!DOCTYPE html>
<html><body><div id="content">
<div id="A">Topic A</div>
<div id="B">Topic B</div>
<div id="C">Topic C</div>
</div>
</body></html>
<!DOCTYPE html>
<html><body><div id="content">
<div id="D">Topic D</div>
<div id="E">Topic E</div>
<div id="F">Topic F</div>
<div id="X">Topic X</div></div>
</body></html>
Last Topic in Doc 2: Topic X
```

如你所见，在任何一种情况下，Topic X 都已从第一个 HTML 片段移动到第二个 HTML 片段。

在 PHP 的未来版本中，预计 DOM 扩展将继续发展，同时遵循 DOM 的实时标准。此外，其使用方式将继续变得更简单，并提供更多灵活性和效率。

现在，让我们把注意力转向 DateTime 扩展中的变化。

## 使用新的 DateTime 方法

在处理日期和时间时，创建 DateTimeImmutable 实例通常很有用。DateTimeImmutable 对象与 DateTime 对象相同，只是其属性值不可更改。了解如何在 DateTime 和 DateTimeImmutable 之间切换是一种有用的技巧，可以避免许多隐藏的逻辑错误。

在讨论 PHP 8 中所做的改进之前，让我们先看看 DateTimeImmutable 解决的潜在问题。

## DateTimeImmutable 的用例

在这个简单示例中，将创建一个包含三个实例的数组，表示从今天起 30、60 和 90 天。这些将用于形成 30-60-90 天应收账款账龄报告的基础：

- 1. 首先，让我们初始化几个关键变量，表示间隔、日期格式以及用于保存最终值的数组：

```
// /repo/ch09/php7_date_time_30-60-90.php
$days = [0, 30, 60, 90];
$fmt = 'Y-m-d';
$aging = [];
```

- 2. 现在，让我们定义一个循环，将间隔添加到 DateTime 实例以生成（希望！）表示 0、30、60 和 90 天的数组。有经验的开发者很可能已经发现了问题！

```
$dti = new DateTime('now');
foreach ($days as $span) {
    $interval = new DateInterval('P' . $span . 'D');
    $item = $dti->add($interval);
    $aging[$span] = clone $item;
}
```

- 3. 接下来，显示已生成的日期集合：

```
echo "Day\tDate\n";
foreach ($aging as $key => $obj)
    echo "$key\t" . $obj->format($fmt) . "\n";
```

- 4. 输出结果如下所示，这完全是一场灾难：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_date_time_30-60-90.php
Day    Date
0      2021-11-20
30     2021-06-23
60     2021-08-22
90     2021-11-20
```

如你所见，问题在于 DateTime 类不是不可变的。因此，每次添加 DateInterval 时，原始值都会被更改，导致显示的日期不准确。

- 5. 然而，通过一个简单的修改，我们就可以纠正这个问题。不是最初创建 DateTime 实例，我们只需改为创建 DateTimeImmutable 实例即可：

```
$dti = new DateTimeImmutable('now');
```



6. 然而，要用 DateTime 实例填充数组，我们需要将 DateTimeImmutable 转换为 DateTime。在 PHP 7.3 中，引入了 DateTime::createFromImmutable() 方法。因此，当值赋给 $aging 时，修改后的代码可能如下所示：

```php
$aging[$span] = DateTime::createFromImmutable($item);
```

7. 否则，你只能创建新的 DateTime 实例，如下所示：

```php
$aging[$span] = new DateTime($item->format($fmt));
```

通过这一项更改，正确的输出将如下所示：

| Day | Date |
|---|---|
| 0 | 2021-05-24 |
| 30 | 2021-06-23 |
| 60 | 2021-07-23 |
| 90 | 2021-08-22 |

你现在已经了解 DateTimeImmutable 的使用方式，并且也对如何转换为 DateTime 有了概念。你会很高兴地知道，在 PHP 8 中，通过引入 createFromInterface() 方法，两种对象类型之间的转换变得更加容易。

## 检查 createFromInterface() 方法

在 PHP 8 中，在 DateTime 和 DateTimeImmutable 之间转换，以及反向转换变得更加容易。名为 createFromInterface() 的新方法已添加到两个类中。该方法的签名只需一个 DateTimeInterface 实例，这意味着 DateTime 或 DateTimeImmutable 的实例都是此方法的可接受参数。

以下简短的代码示例演示了在 PHP 8 中从一种类型转换为另一种类型是多么容易：

- 1. 首先，让我们定义一个 DateTimeImmutable 实例并输出其类和日期：

```php
// /repo/ch09/php8_date_time.php
$fmt = 'l, d M Y';
$dti = new DateTimeImmutable('last day of next month');
echo $dti::class . ':' . $dti->format($fmt) . "\n";
```

- 2. 然后，从 $dti 创建一个 DateTime 实例并添加 90 天的间隔，显示其类和当前日期：

```php
$dtt = DateTime::createFromInterface($dti);
$dtt->add(new DateInterval('P90D'));
echo $dtt::class . ':' . $dtt->format($fmt) . "\n";
```

- 3. 最后，从 $dtt 创建一个 DateTimeImmutable 实例并显示其类和日期：

```php
$dtx = DateTimeImmutable::createFromInterface($dtt);
echo $dtx::class . ':' . $dtx->format($fmt) . "\n";
```

以下是此代码示例在 PHP 8 中运行的输出：

```
root@php8_tips_php8 [/repo/ch09 ]# php php8_date_time.php
DateTimeImmutable:Wednesday, 30 Jun 2021
DateTime:Tuesday, 28 Sep 2021
DateTimeImmutable:Tuesday, 28 Sep 2021
```

正如你所见，我们使用了相同的 createFromInterface() 方法来创建实例。当然请记住，我们实际上并没有将类实例转换为另一个实例。相反，我们是创建了克隆的实例，但类类型不同。

你现在知道为什么你可能想要 DateTimeImmutable 而不是 DateTime。你也知道在 PHP 8 中，名为 createFromInterface() 的新方法提供了一种统一的方式从一个创建另一个的实例。接下来，我们将看看 PHP 8 中对 trait 处理的改进。

## 了解 PHP 8 trait 处理的改进

trait 的实现首次在 PHP 5.4 版本中引入。自那时以来，进行了持续不断的改进。PHP 8 通过提供一种明确识别当多个 trait 具有冲突方法时使用哪些方法的方式，继续了这一趋势。此外，除了消除可见性声明中的不一致之外，PHP 8 还解决了 trait 处理（或未处理！）抽象方法方面的问题。

作为开发者，完全掌握 trait 的使用可以让你编写更高效且更易维护的代码。Trait 可以帮助你避免产生冗余代码。它们解决了需要在不同命名空间或不同类继承结构中提供相同逻辑的问题。本节介绍的信息使你能够在 PHP 8 下运行的代码中正确使用 trait。

首先，让我们看看 PHP 8 中如何解决 trait 之间的冲突。

### 解决 trait 之间的方法冲突

可以通过简单地用逗号分隔列出 trait 名称来使用多个 trait。然而，如果两个 trait 定义了相同的方法，就可能出现潜在问题。为了解决此类冲突，PHP 提供了 as 关键字。在 PHP 7 及更早版本中，为避免两个同名方法冲突，你可以简单地重命名其中一个方法。执行重命名的代码可能如下所示：

```
use Trait1, Trait2 { <METHOD> as <NEW_NAME>; }
```

然而，这种方法的问题在于 PHP 做出了一个静默假设：METHOD 被假定来自 Trait1！在持续努力强制执行良好编码实践方面，PHP 8 不再允许这种假设。PHP 8 中的解决方案是通过使用 insteadof 而不是 as 来更具体。

这里有一个简单的示例来说明这个问题：

- 1. 首先，让我们定义两个 trait，它们定义了相同的方法 test()，但返回不同的结果：

```php
// /repo/ch09/php7_trait_conflict_as.php
trait Test1 {
    public function test() {
        return '111111';
    }
}
trait Test2 {
    public function test() {
        return '222222';
    }
}
```

- 2. 然后，定义一个使用两个 trait 的匿名类，并将 test() 指定为 otherTest() 以避免命名冲突：

```php
$main = new class () {
    use Test1, Test2 { test as otherTest; }
    public function test() { return 'TEST'; }
};
```

- 3. 接下来，定义一段代码块来输出两个方法的返回值：

```php
echo $main->test() . "\n";
echo $main->otherTest() . "\n";
```

以下是 PHP 7 中的输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_trait_conflict_as.php
TEST
111111
```

正如你所见，PHP 7 静默假设我们指的是将 Trait1::test() 重命名为 otherTest()。然而，从示例代码中，完全不清楚这是否是程序员的意图！

在 PHP 8 中运行相同的代码示例，我们会得到不同的结果：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_trait_conflict_as.php
PHP Fatal error:  An alias was defined for method test(), which exists in both Test1 and Test2. Use Test1::test or Test2::test to resolve the ambiguity in /repo/ch09/php7_trait_conflict_as.php on line 6
```

显然，PHP 8 不会做出此类静默假设，因为它们很容易导致意外行为。在此示例中，最佳实践是使用作用域解析 (::) 运算符。以下是重写的代码：

```php
$main = new class () {
    use Test1, Test2 { Test1::test as otherTest; }
    public function test() { return 'TEST'; }
};
```

如果我们在 PHP 8 中重新运行代码，输出将与为 PHP 7 显示的输出相同。作用域解析运算符确认 Trait1 是 test() 方法的源 trait，从而避免任何歧义。现在，让我们看看 PHP 8 trait 如何处理抽象方法签名。

### 使用 trait 抽象签名检查

正如 API 开发者所熟知，将方法标记为抽象是向 API 用户发出该方法为强制性但尚未定义的方式。此技术允许 API 开发者不仅指定方法名称，还指定其签名。

然而，在 PHP 7 及更早版本中，trait 中定义的抽象方法会忽略签名，这违背了使用抽象方法的初衷！当你在 PHP 8 中使用包含抽象方法的 trait 时，其签名会与使用该 trait 的类中的实现进行检查。

以下示例在 PHP 7 中有效，但在 PHP 8 中失败，因为方法签名不同：

- 1. 首先，让我们声明严格类型检查并定义一个包含抽象方法 trait；即 add()。请注意，方法签名要求全部使用整数数据类型：
- 2. 接下来，定义一个使用该 trait 并定义 add() 的匿名类。请注意，该类的所有数据类型均为 float：
- 3. 然后，输出 111.111 和 222.222 相加的结果：

```php
// /repo/ch09/php7_trait_abstract_signature.php
declare(strict_types=1);
trait Test1 {
    public abstract function add(int $a, int $b) : int;
}
```

```php
$main = new class () {
    use Test1;
    public function add(float $a, float $b) : float {
        return $a + $b;
    }
};
```

```php
echo $main->add(111.111, 222.222) . "\n";
```



这个在 PHP 7 中运行的小代码示例的结果令人惊讶：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_trait_abstract_signature.php
333.333
```

如结果所示，trait 中抽象定义的方法签名被完全忽略！然而在 PHP 8 中，结果却大不相同。以下是该代码在 PHP 8 中运行的输出：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_trait_abstract_signature.php
PHP Fatal error: Declaration of class@anonymous::add(float $a, float $b): float must be compatible with Test1::add(int $a, int $b): int in /repo/ch09/php7_trait_abstract_signature.php on line 9
```

前述 PHP 8 输出向我们展示，无论抽象方法定义的来源如何，良好的编码实践都得到了强制执行。

本节的最后一个主题将向您展示如何在 trait 中处理抽象私有方法。

## 在 trait 中处理抽象私有方法

一般来说，在 PHP 中，您无法对抽象超类中的抽象私有方法强制控制，因为它不会被继承。然而在 PHP 8 中，您可以在 trait 中定义抽象私有方法！这可以用作用例强制机制，当您进行 API 开发时，使用类需要定义指定的私有方法。

请注意，虽然您可以在 PHP 8 trait 中将抽象方法指定为私有，但使用该 trait 的类可以轻松覆盖 trait 方法的可见性。因此，本节不会展示任何代码示例，因为私有抽象 trait 方法的效果与在其他可见性级别使用抽象 trait 方法完全相同。

> **提示**
> 
> 有关 PHP 8 中 trait 抽象方法处理的更多信息，请查看这个 RFC：https://wiki.php.net/rfc/abstract_trait_method_validation.

现在，让我们来看看私有方法中的一般用法变化。

## 处理私有方法

开发人员创建超类的原因之一是为了对子类的方法签名施加一定程度的控制。在解析阶段，PHP 通常会确认方法签名是否匹配。这导致其他开发人员正确使用您的代码。

同样，如果方法被标记为私有，就没有必要让 PHP 执行同样严格的方法签名检查。私有方法的目的是使其对扩展类不可见。如果您在扩展类中定义了同名的方法，您应该可以自由定义它。

为说明此问题，让我们定义一个名为 Cipher 的类，其中包含一个名为 encrypt() 的私有方法。OpenCipher 子类重新定义了此方法，导致在 PHP 7 下运行时出现致命错误：

- 首先，让我们定义一个 Cipher 类，其构造函数为 $key 和 $salt 生成随机值。它还定义了一个名为 encode() 的公共方法，该方法调用私有 encrypt() 方法：

```php
// /repo/src/Php7/Encrypt/Cipher.php
namespace Php7\Encrypt;
class Cipher {
    public $key = '';
    public $salt = 0;
    public function __construct() {
        $this->salt = rand(1,255);
        $this->key = bin2hex(random_bytes(8));
    }
    public function encode(string $plain) {
        return $this->encrypt($plain);
    }
}
```

- 接下来，让我们定义一个名为 encrypt() 的私有方法，该方法使用 str_rot13() 生成加密文本。请注意，该方法被标记为 final。虽然这没有任何意义，但为了本说明的目的，请假设这是有意为之：

```php
final private function encrypt(string $plain) {
    return base64_encode(str_rot13($plain));
}
```

- 最后，让我们定义一个简短的调用程序，创建类实例并调用已定义的方法：

```php
// /repo/ch09/php7_oop_diffs_private_method.php
include __DIR__ . '/../src/Server/Autoload/Loader.php';
$loader = new \Server\Autoload\Loader();
use Php7\Encrypt\{Cipher,OpenCipher};
$text = 'Super secret message';
$cipher1 = new Cipher();
echo $cipher1->encode($text) . "\n";
$cipher2 = new OpenCipher();
var_dump($cipher2->encode($text));
```

如果我们在 PHP 7 中运行调用程序，我们将得到以下输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_oop_diffs_private_method.php
RmhjcmUgZnJwZKJnIHpyZmZudHI=
PHP Fatal error: Cannot override final method Php7\Encrypt\Cipher::encrypt() in /repo/src/Php7/Encrypt/OpenCipher.php on line 21
```

在这里，您可以看到 Cipher 的输出已正确生成。然而，已抛出致命错误，并显示一条消息，指出我们无法覆盖 final 方法。理论上，私有方法应该对子类完全不可见。然而，正如您从输出中清楚看到的那样，情况并非如此。Cipher 超类私有方法的方法签名影响了我们在子类中重新定义相同方法的能力。

然而在 PHP 8 中，这一悖论已得到解决。以下是同一代码在 PHP 8 中运行的输出：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_oop_diffs_private_method.php
PHP Warning: Private methods cannot be final as they are never overridden by other classes in /repo/src/Php7/Encrypt/Cipher.php on line 17
RmhjcmUgZnJwZXJnIHpyZmZudHI=
array(2) {
["tag"]=> string(24) "woD6Vi73/IXLaKHFGUC3aA=="
["cipher"]=> string(28) "+vd+jWKqo8WFPd7SakSvszkoIX0="
```

如前述输出所示，应用程序成功运行，并且父类和子类的输出都显示出来了。我们还可以看到一个警告，告知我们私有方法不能标记为 final。

> **提示**
> 
> 有关私有方法签名的背景讨论的更多信息，请查看这个文档参考：
> https://wiki.php.net/rfc/inheritance_private_methods.

现在您已经了解了 PHP 8 如何防止子类看到超类中的私有方法，让我们把注意力转向 PHP 8 中匿名类的差异。

## 控制匿名类的使用

**匿名类**，根据其定义本身，没有名称。然而，为了信息目的，PHP 信息函数如 `var_dump()`、`var_export()`、`get_class()`，以及 Reflection 扩展中的其他类会将匿名类简单报告为 `class@anonymous`。然而，当匿名类扩展另一个类或实现接口时，让 PHP 信息函数反映这一事实可能会有用。

在 PHP 8 中，扩展类或实现接口的匿名类现在通过更改分配给匿名类的标签来反映这一事实，改为 Xyz@anonymous，其中 Xyz 是类或接口的名称。如果匿名类实现多个接口，则只会出现第一个接口。如果匿名类扩展类并同时实现一个或多个接口，则其标签中会出现其扩展的类名。下面的表格总结了这些可能性：

| 上下文 | 新名称 |
|---|---|
| 继承 Xyz | Xyz@anonymous |
| 实现 Abc | Abc@anonymous |
| 实现 Abc, Def | Abc@anonymous |
| 继承 Xyz 实现 Abc | Xyz@anonymous |

表 9.4 – 匿名类提升

请记住，PHP 已经可以测试匿名类以查看它是否属于某条继承线。例如，instanceof 运算符可用于此目的。以下示例说明了如何测试匿名类的继承性，以及如何查看其新名称：

- 对于此示例，我们将定义一个 DirectoryIterator 实例，从当前目录获取文件列表：
- 然后，我们将定义一个扩展 FilterIterator 的匿名类。在该类中，我们将定义 accept() 方法，该方法产生布尔结果。如果结果为 TRUE，则该项将出现在最终迭代中：

```php
// /repo/ch09/php8_oop_diff_anon_class_renaming.php
$iter = new DirectoryIterator(__DIR__);
```

```php
$anon = new class ($iter) extends FilterIterator {
    public $search = '';
    public function accept() {
        return str_contains(
            $this->current(), $this->search);
    }
};
```



3. 接下来，我们将生成一个包含 bc_break 在名称中的文件列表：

```
$anon->search = 'bc_break';
foreach ($anon as $fn) echo $fn . "\n";
```

4. 在接下来的两行中，我们将使用 instanceof 测试匿名类是否实现了 OuterInterface：

```
if ($anon instanceof OuterIterator)
    echo "This object implements OuterIterator\n";
```

5. 最后，我们将使用 var_dump() 简单地转储匿名类的内容：

```
echo var_dump($anon);
```

这里是在 PHP 8 下运行的输出！我们无法在 PHP 7 中运行此示例，因为该版本缺少 str_contains() 函数！

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_oop_diff_anon_class_renaming.php
php8_bc_break_construct.php
php8_bc_break_serialization.php
php8_bc_break_destruct.php
php8_bc_break_sleep.php
php8_bc_break_scanner.php
php8_bc_break_serializable.php
php8_bc_break_magic_wrong.php
php8_bc_break_magic.php
php8_bc_break_magic_to_string.php
This object implements OuterIterator
object(FilterIterator@anonymous)#2 (1) {
    ["search"]=> string(8) "bc_break"
}
```

正如你所见，instanceof 正确报告匿名类实现了 OuterInterface（因为它扩展了 FilterIterator，而 FilterIterator 又实现了 OuterInterface）。你也可以看到 var_dump() 将匿名类的名称报告为 FilterIterator@anonymous。

现在你对 PHP 8 中匿名类命名是如何变化的有了一个概念，让我们来看看命名空间处理的变化。

## 理解命名空间的变化

命名空间的概念在 PHP 5.3 中引入，作为隔离类层级的一种手段。不幸的是，用于解析命名空间名称的原始算法存在多个缺陷。除了过于复杂之外，命名空间和类名在内部被标记化的方式也不一致，导致出现意外的错误。

在讨论其好处和潜在的向后兼容性破坏之前，让我们先看看命名空间标记化过程是如何变化的。

## 发现标记化的差异

标记化过程是解释过程的重要组成部分，并在你的 PHP 代码执行时发生。在生成字节码的过程中，PHP 程序代码会被 PHP 解析引擎分解为标记。

下表总结了命名空间标记化的变化：

| 命名空间 + 类 | 旧标记化 | 新标记化 |
|---|---|---|
| JustClass | T_STRING | T_STRING |
| \AbsoluteClass | T_NS_SEPARATOR + T_STRING | T_NAME_FULLY_QUALIFIED |
| Some\Thing\Class | T_STRING + T_NS_SEPARATOR + T_STRING | T_NAME_QUALIFIED |
| <namespace>\Class | T_NAMESPACE + T_NS_SEPARATOR + T_STRING | T_NAME_RELATIVE |

表 9.5 – PHP 8 中命名空间标记化差异

正如你所见，PHP 8 的命名空间标记化要简单得多且更精简。

> **提示**
> 
> 有关解析器标记的更多信息，请参阅以下文档参考：https://www.php.net/manual/en/tokens.

这一变化的影响产生了极其积极的结果。首先，你现在可以在命名空间中使用保留关键字。此外，未来随着语言中引入新关键字，PHP 不会强制你更改应用程序中的命名空间。新的标记化过程也促进了 Attributes 与命名空间一起使用。

首先，让我们看看在命名空间中使用关键字。

## 在命名空间中使用保留关键字

PHP 7 中的命名空间标记化过程会生成一系列字符串（T_STRING）和反斜杠（T_NS_SEPARATOR）标记。这种方法的问题在于，如果其中一个字符串碰巧是 PHP 关键字，解析过程中会立即抛出语法错误。然而，PHP 8 仅生成一个标记，如表 9.5 中前面所示。最终，这意味着你可以将几乎任何内容放在命名空间中，而不必担心保留关键字冲突。

以下代码示例说明了 PHP 8 与早期版本之间命名空间处理的差异。在此示例中，PHP 关键字被用作命名空间的一部分。在 PHP 7 中，由于其不准确的标记化过程，List 被视为关键字而不是命名空间的一部分：

```
```php
// /repo/ch09/php8_namespace_reserved.php
namespace List\Works\Only\In\PHP8;
class Test {
    public const TEST = 'TEST';
}
echo Test::TEST . "\n";
```
```

这里是 PHP 7 的输出：

```
```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_namespace_reserved.php
PHP Parse error: syntax error, unexpected 'List' (T_LIST),
expecting '{' in /repo/ch09/php8_namespace_reserved.php on line 3
```
```

在 PHP 8 中，程序代码片段按预期工作，如下所示：

```
```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_namespace_reserved.php
TEST
```
```

现在你对标记化过程在 PHP 8 与早期 PHP 版本之间的差异以及其潜在好处有了一个概念，让我们来看看潜在的向后兼容代码破坏。

## 暴露不良的命名空间命名实践

同样的 PHP 8 标记化过程使你不必担心关键字冲突，也可能暴露不良的命名空间命名实践。任何包含空白的命名空间现在都被视为无效。然而，无论如何，在命名空间中包含空白都是不好的做法！

以下简单代码示例说明了这一原则。在此示例中，你会注意到命名空间包含空白：

// /repo/ch09/php7_namespace_bad.php
namespace Doesnt \Work \In \PHP8;
class Test {
    public const TEST = 'TEST';
}
echo Test::TEST . "\n";

如果我们在 PHP 7 中运行此代码，它可以正常工作。以下是 PHP 7 的输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_namespace_bad.php
TEST
```

然而，在 PHP 8 中，会抛出 ParseError，如下所示：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_namespace_bad.php
PHP Parse error: syntax error, unexpected fully qualified name "\Work", expecting "{" in /repo/ch09/php7_namespace_bad.php on line 3
```

空格充当解析器在标记化过程中使用的分隔符。在此代码示例中，PHP 8 假设命名空间是 Doesnt。下一个标记是 \ Work，它标记了一个完全限定的类名。然而，在此时并不期望它，这就是为什么会抛出错误的原因。

这结束了我们对 PHP 8 中命名空间处理所做的更改的讨论。你现在处于更好的位置，可以在 PHP 8 中创建命名空间名称，这些名称不仅遵循最佳实践，而且还能利用其独立于关键字命名冲突的特性。

## 摘要

PHP 8，正如你所学到的那样，在定义魔术方法方面严格得多。在本章中，你学习了方法签名更改以及如何通过正确使用魔术方法来减少潜在错误。你还学习了 Reflection 和 PDO 扩展中的方法签名更改。有了本章获得的知识，你可以在迁移到 PHP 8 时避免潜在问题。此外，你还学习了静态方法调用方式的变化，以及一种新的静态返回类型。

然后，你学习了如何最好地使用私有方法，以及如何对匿名类施加更大的控制。你还了解了一些关于新语法可能性以及由于语言更改而现在已过时的方法的提示。

你还学习了如何正确使用接口和 trait 以促进代码的高效使用。你了解了引入的新接口，以使 DOM 扩展符合新的 DOM Living Standard。此外，你还深入了解了 DateTime 扩展中引入的新方法的使用。

最后，你学习了如何清理命名空间使用并生成更紧凑的代码。你现在对命名空间标记化过程的不准确性以及它在 PHP 8 中是如何改进的有了更好的理解。

下一章将为你呈现每个开发人员都努力追求的东西：提高性能的技巧、窍门和技术。


