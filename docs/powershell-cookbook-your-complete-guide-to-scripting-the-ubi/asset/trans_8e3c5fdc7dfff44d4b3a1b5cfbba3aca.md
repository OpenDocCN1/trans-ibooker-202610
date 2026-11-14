
在接下来出现的“设置选项”页面上，选择“一个唯一实例”。在“实例名称”页面上，键入 `Test` 作为实例名称。在“端口”页面上，接受默认端口，然后在“应用程序目录分区”页面上，选择“是，创建一个应用程序目录分区”。作为分区名称，键入 `DC=Fabrikam,DC=COM`。

在后续页面中，接受默认的文件位置、服务账户和管理员。

当安装向导提供导入 LDIF 文件的选项时，导入除 `MS-AZMan.LDF` 之外的所有可用文件。单击此页面和确认页面上的“下一步”以完成实例设置。

打开一个 PowerShell 窗口，并测试你的新实例：

```
PS > [adsi] "LDAP://localhost:389/dc=Fabrikam,dc=COM"

distinguishedName
-----------------
{DC=Fabrikam,DC=COM}
```

`[adsi]` 标记是一个 `type shortcut`（类型快捷方式），就像 PowerShell 中的其他几个类型快捷方式一样。`[adsi]` 类型快捷方式提供了一种通过 Active Directory 服务接口创建和处理目录项的快速方法。

### 注意

当你第一次尝试此快捷方式时，可能会收到这个无用的错误消息：

```
format-default : The following exception occurred while retrieving
member "PSComputerName": "Unknown error (0x80005000)"
```

如果收到此错误，请确保 `LDAP://localhost` 中的 `LDAP` 已大写。

尽管针对 AD LDS 测试环境的脚本与直接针对 Active Directory 运行的脚本几乎相同，但仍存在一些细微差别。AD LDS 脚本在其绑定字符串中指定主机和端口（即 `localhost:389/`），而 Active Directory 脚本则不需要。

有关 PowerShell 中类型快捷方式的详细信息，请参阅 [“使用 .NET Framework”](#app01.xhtml#working_with_the_net_framework)。

## 另请参阅

[“使用 .NET Framework”](#app01.xhtml#working_with_the_net_framework)

# 26.2 创建组织单位

## 问题

你想要在 Active Directory 中创建一个组织单位 (OU)。

## 解决方案

要在容器中创建 OU，请使用 `[adsi]` 类型快捷方式绑定到 Active Directory 的一部分，然后调用 `Create()` 方法。

```
$domain = [adsi] "LDAP://localhost:389/dc=Fabrikam,dc=COM"
$salesOrg = $domain.Create("OrganizationalUnit", "OU=Sales")
$salesOrg.Put("Description", "Sales Headquarters, SF")
$salesOrg.Put("wwwHomePage", "http://fabrikam.com/sales")
$salesOrg.SetInfo()
```

## 讨论

解决方案展示了一个在组织根目录下创建 `Sales` OU 的示例。你可以使用相同的语法在其他 OU 下创建 OU。[示例 26-1](#ch26.xhtml#creating_east_and_west_sales_divisions) 演示了如何创建更多销售部门。

### 示例 26-1. 创建北区、东区和西区销售部门

```
$sales = [adsi] "LDAP://localhost:389/ou=Sales,dc=Fabrikam,dc=COM"

$east = $sales.Create("OrganizationalUnit", "OU=East")
$east.Put("wwwHomePage", "http://fabrikam.com/sales/east")
$east.SetInfo()

$west = $sales.Create("OrganizationalUnit", "OU=West")
$west.Put("wwwHomePage", "http://fabrikam.com/sales/west")
$west.SetInfo()

$north = $sales.Create("OrganizationalUnit", "OU=North")
$north.Put("wwwHomePage", "http://fabrikam.com/sales/north")
$north.SetInfo()
```

当你最初创建项目时，请注意你需要使用 `Put()` 方法在新项目上设置属性。创建项目后，你可以改用简单的属性访问来更改这些属性。有关更改 OU 属性的详细信息，请参阅 [配方 26.4](#ch26.xhtml#modify_properties_of_an_organizational_u)。

要检查这些 OU 是否已创建，请参阅 [配方 26.6](#ch26.xhtml#get_the_children_of_an_active_directory_)。

使用 Active Directory 模块，创建 OU 的 cmdlet 是 `New-ADOrganizationalUnit`。有关如何通过 Active Directory 模块完成这些任务的详细信息，请参阅 [该模块的在线帮助文档](https://oreil.ly/JiH6q)。

## 另请参阅

[配方 26.4，“修改组织单位的属性”](#ch26.xhtml#modify_properties_of_an_organizational_u)

[配方 26.6，“获取 Active Directory 容器的子项”](#ch26.xhtml#get_the_children_of_an_active_directory_)

# 26.3 获取组织单位的属性

## 问题

你想要获取并列出特定 OU 的属性。

## 解决方案

要列出 OU 的属性，请使用 `[adsi]` 类型快捷方式绑定到 Active Directory 中的 OU，然后将 OU 传递给 `Format-List` cmdlet：

```
$organizationalUnit =
  [adsi] "LDAP://localhost:389/ou=West,ou=Sales,dc=Fabrikam,dc=COM"

$organizationalUnit | Format-List *
```

## 讨论

解决方案检索 `Sales West` OU。默认情况下，`Format-List` cmdlet 仅显示组的可分辨名称，因此我们输入 `Format-List *` 以显示所有属性。

如果你知道想要获取值的属性，可以按名称指定它：

```
PS > $organizationalUnit.wWWHomePage
http://fabrikam.com/sales/west
```

如果你在获取已知存在的属性时遇到麻烦，也可以使用容器上的 `Get()` 方法来检索该属性。虽然 `name` 属性可以使用常规属性语法访问，但以下示例演示了替代方法：

```
PS > $organizationalUnit.Get("name")
West
```

使用 Active Directory 模块，获取组织单位属性的 cmdlet 是 `Get-ADOrganizationalUnit`。有关如何通过 Active Directory 模块完成这些任务的详细信息，请参阅 [该模块的在线帮助文档](https://oreil.ly/JiH6q)。

# 26.4 修改组织单位的属性

## 问题

你想要修改特定 OU 的属性。

## 解决方案

要修改 OU 的属性，请使用 `[adsi]` 类型快捷方式绑定到 Active Directory 中的 OU。如果该属性已设置，你可以像更改任何其他 PowerShell 对象的属性值一样更改它。如果你是首次设置属性，请使用 `Put()` 方法。最后，调用 `SetInfo()` 方法以应用更改。

```
$organizationalUnit =
  [adsi] "LDAP://localhost:389/ou=West,ou=Sales,dc=Fabrikam,dc=COM"

$organizationalUnit.Put("Description", "Sales West Organization")
$organizationalUnit.wwwHomePage = "http://fabrikam.com/sales/west/fy2012"
$organizationalUnit.SetInfo()
```

## 讨论

解决方案检索 `Sales West` OU。然后将说明设置为 `Sales West Organization`，更新主页，并将这些更改应用到 Active Directory。

使用 Active Directory 模块，修改 OU 属性的 cmdlet 是 `Set-ADOrganizationalUnit`。有关如何通过 Active Directory 模块完成这些任务的详细信息，请参阅 [该模块的在线帮助文档](https://oreil.ly/JiH6q)。

# 26.5 删除组织单位

## 问题

你想要删除特定的 OU。

## 解决方案

要删除 OU，请使用 `[adsi]` 类型快捷方式绑定到 Active Directory 中的 OU。最后，调用其 `DeleteTree()` 方法以应用更改。

```
$organizationalUnit =
  [adsi] "LDAP://localhost:389/ou=North,ou=Sales,dc=Fabrikam,dc=COM"
$organizationalUnit.DeleteTree()
```

## 讨论

解决方案检索 `Sales North` OU。然后调用 `DeleteTree()` 方法永久删除该 OU 及其所有子项。

使用 Active Directory 模块，删除 OU 的 cmdlet 是 `Remove-ADOrganizationalUnit`。有关如何通过 Active Directory 模块完成这些任务的详细信息，请参阅 [该模块的在线帮助文档](https://oreil.ly/JiH6q)。

# 26.6 获取 Active Directory 容器的子项

## 问题

你想要列出 Active Directory 容器的所有子项。

## 解决方案

要列出容器中的项目，请使用 `[adsi]` 类型快捷方式绑定到 Active Directory 中的 OU，然后访问该容器的 `Children` 属性：
