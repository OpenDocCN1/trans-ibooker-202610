
此外，PHP 8 在使用 `php.ini` 文件中的 `disable_functions` 指令定义任何函数时的处理方式发生了变化。让我们先来看看这个指令。

## 理解禁用函数处理的变更

网站托管公司通常提供大幅折扣的 **共享托管** 套餐。一旦客户注册，托管公司的 IT 人员就会在共享服务器上创建账户，为控制磁盘空间使用分配磁盘配额，并在 Web 服务上创建 **虚拟主机** 定义。然而，这类托管公司面临的问题是，允许 PHP 无限制访问会给共享托管公司以及同一服务器上的其他用户带来安全风险。

为解决此问题，IT 人员通常会将一个用逗号分隔的函数列表分配给 `php.ini` 指令，**disable_functions**。这样一来，列表中的任何函数都无法在该服务器上运行的 PHP 代码中使用。 通常会出现在该列表上的函数是那些允许操作系统访问的函数，例如 `system()` 或 `shell_exec()`。

只有内部 PHP 函数才能出现在该列表中。内部函数是指包含在 PHP 核心中的函数以及通过扩展提供的函数。用户定义的函数不受此指令影响。

## 检查禁用函数处理的差异

在 PHP 7 及更早版本中，禁用函数无法重新定义。在 PHP 8 中，禁用函数被视为从未存在过，这意味着可以重新定义。

> **重要提示**
> 仅仅因为你可以在 PHP 8 中重新定义禁用函数 *并不* 意味着原始功能已被恢复！

为说明这一概念，我们首先在 `php.ini` 文件中添加这一行：
`disable_functions=system`。

请注意，我们需要在 *两个* Docker 容器（PHP 7 和 PHP 8）中都添加此项以完成说明。更新 `php.ini` 文件的命令如下所示：

```
root@php8_tips_php7 [ /repo/ch06 ]#
echo "disable_functions=system">>/etc/php.ini
root@php8_tips_php8 [ /repo/ch06 ]#
echo "disable_functions=system">>/etc/php.ini
```

如果我们随后尝试使用 system() 函数，则在 PHP 7 和 PHP 8 中都会失败。这里我们展示 PHP 8 的输出：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php -r "system('ls -l');"
PHP Fatal error: Uncaught Error: Call to undefined function system() in Command line code:1
```

我们随后定义一些程序代码来重新定义被禁止的函数：

// /repo/ch06/php8_disabled_funcs_redefine.php
function system(string $cmd, string $path = NULL) {
    $output = '';
    $path = $path ?? __DIR__;
    if ($cmd === 'ls -l') {
        $iter = new RecursiveDirectoryIterator($path);
        foreach ($iter as $fn => $obj)
            $output .= $fn . "\n";
    }
    return $output;
}
echo system('ls -l');
```

正如代码示例所示，我们创建了一个函数，它模仿 ls -l Linux 系统调用的行为，但仅使用安全的 PHP 函数和类。然而，如果我们尝试在 PHP 7 中运行它，则会抛出致命错误。以下是 PHP 7 的输出：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_disabled_funcs_redefine.php
PHP Fatal error: Cannot redeclare system() in /repo/ch06/php8_disabled_funcs_redefine.php on line 17
```

然而在 PHP 8 中，我们的函数重新定义成功了，如下所示：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_disabled_funcs_redefine.php
/repo/ch06/php8_printf_vs_vprintf.php
/repo/ch06/php8_num_str_non_wf_extracted.php
/repo/ch06/php8_vprintf_bc_break.php
/repo/ch06/php7_vprintf_bc_break.php
```

```
... not all output is shown ...
/repo/ch06/php7_curly_brace_usage.php
/repo/ch06/php7_compare_num_str_valid.php
/repo/ch06/php8_compare_num_str.php
/repo/ch06/php8_disabled_funcs_redefine.php
```

你现在已经对如何处理禁用函数有了一个概念。接下来，让我们来看看重要的 crypt() 函数的变更。

## 了解 crypt() 函数的变更

crypt() 函数自 PHP 4 版本以来一直是 PHP 哈希生成的支柱。其持久存在的一个原因是因为它有许多选项。如果你直接在代码中使用 crypt()，你会很高兴地注意到，如果提供了不可用的 salt 值，长期以来被认为已损坏的数据加密标准 (DES)，不再是 PHP 8 中的后备方案！salt 有时也被称为初始化向量 (IV)。

另一个重要变更涉及 rounds 值。一次轮次就像洗牌：洗的次数越多，随机化程度越高（除非你面对的是拉斯维加斯的赌牌高手！）。在密码学中，块类似于牌。在每一轮中，密码学函数会应用于每个块。如果密码学函数很简单，哈希可以更快地生成；然而，需要更多的轮次才能完全随机化块。

SHA-1（安全哈希算法）家族使用快速但简单的算法，因此需要更多轮次。另一方面，SHA-2 家族使用更复杂的哈希函数，消耗更多资源，但轮次更少。

在使用 PHP crypt() 函数与 CRYPT_SHA256（SHA-2 家族）结合时，PHP 8 不再静默地将 rounds 参数解析为最接近的限制。相反，crypt() 将以 *0 返回值失败，与 glibc 行为一致。此外，在 PHP 8 中，第二个参数（salt）现在是强制性的。

以下示例说明了使用 crypt() 函数时 PHP 7 和 PHP 8 之间的差异：

- 1. 首先，我们定义表示不可用 salt 值和非法轮次数的变量：

// /repo/ch06/php8_crypt_sha256.php
$password = 'password';
$salt = str_repeat('+x=', CRYPT_SALT_LENGTH + 1);
$rounds = 1;
```

- 2. 然后我们使用 crypt() 函数创建两个哈希。在第一次使用中，$default 是在提供无效 salt 参数后的结果。第二次使用中，$sha256 提供了有效的 salt 值，但轮次数无效：

```
$default = crypt($password, $salt);
$sha256 = crypt($password,
'$5$rounds=' . $rounds . '$' . $salt . '$');
echo "Default : $default\n";
echo "SHA-256 : $sha256\n";
```

以下是代码示例在 PHP 7 中运行的输出：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_crypt_sha256.php
PHP Deprecated: crypt(): Supplied salt is not valid for DES. Possible bug in provided salt format. in /repo/ch06/php8_crypt_sha256.php on line 7
Default : +xj31ZMTZkVA
SHA-256 : $5$rounds=1000$+x=+x=+x=+x=+x=+
$3Si/vFn6/xmdTdyleJ17Rb9Heg6DWgkRVKS9T0Zzy/B
```

注意 PHP 7 如何静默修改原始请求。在第一种情况下，crypt() 回退到 DES（！）。在第二种情况下，PHP 7 静默地将 rounds 值从 1 修改为最接近的限制 1000。

另一方面，同一代码在 PHP 8 中运行会失败并返回 *0，如下所示：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_crypt_sha256.php
Default : *0
SHA-256 : *0
```

正如我们在本书中反复强调的，当 PHP 为你做出假设时，最终你会得到产生不一致结果的糟糕代码。在刚才显示的代码示例中，最佳实践是定义一个类方法或函数，以对参数施加更大的控制。通过这种方式，你可以验证参数，避免依赖 PHP 的假设。

接下来，我们来看看 password_hash() 函数的变更。

## 处理 password_hash() 的变更

多年来，太多开发者误用了 crypt()，PHP 核心团队决定添加一个包装函数 password_hash()。这被证明是一个巨大的成功，现在是使用最广泛的安全函数之一。以下是 password_hash() 的函数签名：

```
password_hash(string $password, mixed $algo, array $options=?)
```

当前支持的算法包括 bcrypt、Argon2i 和 Argon2id。建议你使用预定义的算法常量：PASSWORD_BCRYPT、PASSWORD_ARGON2I 和 PASSWORD_ARGON2ID。PASSWORD_DEFAULT 算法目前设置为 bcrypt。选项因算法而异。如果使用 PASSWORD_BCRYPT 或 PASSWORD_DEFAULT 算法，选项包括 cost 和 salt。

常规 wisdom 建议最好使用 password_hash() 函数创建的随机生成的 salt。在 PHP 7 中，salt 选项已被弃用，并在 PHP 8 中被忽略。这不会导致向后兼容性中断，除非你因其他原因依赖 salt。

在此代码示例中，使用了非随机 salt 值：

// /repo/ch06/php8_password_hash.php
$salt = 'xxxxxxxxxxxxxxxxxxxxxxxx';
$password = 'password';
$hash = password_hash(
    $password, PASSWORD_DEFAULT, ['salt' => $salt]);
echo $hash . "\n";
var_dump(password_get_info($hash));
```

在 PHP 7 输出中，会发出弃用 Notice：

```
root@php8_tips_php7 [ /repo/ch06 ]# php php8_password_hash.php
PHP Deprecated: password_hash(): Use of the 'salt' option to password_hash is deprecated in /repo/ch06/php8_password_hash.php on line 6
$2y$10$xxxxxxxxxxxxxxxxxxxxxxu0d9YtxiLKHM/198x//sqUV1V2XTZEZ.
/repo/ch06/php8_password_hash.php:8:
array(3) {
  'algo' => int(1)
```
