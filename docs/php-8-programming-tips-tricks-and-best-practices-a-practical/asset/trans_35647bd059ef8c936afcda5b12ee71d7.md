
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
