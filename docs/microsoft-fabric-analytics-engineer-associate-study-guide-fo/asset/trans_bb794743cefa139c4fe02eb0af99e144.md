

我们已在图中（编号 1–5）突出显示了你在创建过程中使用的区域：

+ New button (area 1)

点击此按钮可创建一个新的动态行级安全角色。

Role name box (area 2)

默认情况下，角色名称为 Untitled，但你可以通过双击该框或从三点菜单中选择“rename”来更改它。在图中，我们已将“DynamicSalesRep”输入为角色的新名称。

Table selection box (area 3)

在这里，你可以选择要过滤的表。在我们的示例中，我们希望根据当前用户的登录信息过滤 SalesRepresentatives 表。

Switch to DAX editor button (area 4)

实现这种动态方法最简单的方式是将其作为 DAX 表达式创建，因此我们点击“Switch to DAX editor”按钮。

请注意，在默认模式下，如你在之前的方法中所见，此按钮显示为“Switch to DAX editor”。然而，当我们点击它时，文字会更改为“ Switch to default editor”。这在图 7-15 中显示，该图描绘了我们点击按钮后的屏幕。

DAX expression window (area 5)

在这里，你可以输入 DAX 表达式来过滤你的数据。

图中区域 5 中输入的 DAX 表达式使用了 Power BI 中的 UserPrincipalName() 表达式。

在 Power BI 中，我们有两种获取用户登录信息的方式：使用 UserName() 表达式或 UserPrincipalName() 表达式。这两个表达式（或函数）以两种方式返回用户的登录信息：UserName() 返回 UserID（用户的信息采用 <domain>\<username> 格式），而 UserPrincipalName() 返回用户名（用户的信息采用 <username>@<domain> 格式）。在这种情况下，我们拥有销售代表的电子邮件，因此我们可以使用 UserPrincipalName() 函数在过滤上下文中获取当前用户的信息。这给出了图中区域 5 的 DAX 表达式：

```
[SREmail] == userprincipalname()
```

要测试此角色，我们可以返回 Power BI 窗口并点击图 7-9 中的“View as”按钮，以调出图 7-16 所示的窗口。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_385_0.png)

图 7-16. Power BI Desktop 中的“View as”窗口

我们需要选中“Other user”复选框，以便输入我们想要测试报告的用户的电子邮件。我们还需要选中“DynamicSalesRep”角色的复选框以使用该角色进行测试。然后，我们可以点击 OK 生成仅包含电子邮件为 bb@dp600.com 的销售代表数据的报告（见图 7-17）。

| SalesRepresentative | Area | Sum of Sales |
|---|---|---|
| Brian Bønk | Denmark | 100 |
| Brian Bønk | The Netherlands | 400 |
| Total |  | 500 |

图 7-17. 基于当前用户的动态过滤生成的报告

在我们完成部署到工作区后，可以通过遵循图 7-15 所示的相同流程添加更多人员和用户到该角色。

你也可以修改使用 UserPrincipalName() 作为单一过滤表达式的简单方法，并将其修改以满足你的确切需求。表达式不必引用单个表。它可以是你需要的任意复杂的 DAX 表达式。

这些示例仅代表 Power BI 中行级安全的冰山一角。存在许多机制和实现方法。你可以拥有包含数十个表和关系的完整语义模型，并且设置和实现可能会迅速变得复杂，并需要对方法进行深入分析。哪种方法是“正确”的，取决于你组织的需要和要求。

## Warehouse Security

在 Microsoft Fabric 中在仓库中实现安全性与在 Power BI 中完全不同。我们使用 *security policy* 来实现，它使用 SQL analytics endpoint。安全策略是一组 T-SQL 语句，共同定义在使用 SQL analytics endpoint 时对一个或多个表的访问。方法几乎与语义模型相同，尽管场景需要更多代码，因为表之间的关系在安全函数本身中并未强制执行。开发人员需要基于自己的知识显式处理所有内容。

## Row-Level Security in a Warehouse

安全策略由 SECURITY POLICY 语句和谓词组成。*predicate* 是一个函数，根据当前登录的用户过滤特定表上的数据。我们将此函数创建为 SCHEMA bound function，并使用 SECURITY POLICY 语句设置谓词与用户之间的关系。

例如，一个 SCHEMA bound function 查看 User_Name() 来过滤行，并且该函数随后通过 SECURITY POLICY 语句绑定到表。这里的主要要点是，你需要非常明确。如果用户拥有一个角色，允许他们查看一个表中的特定数据集，并且此数据与另一个表相关，那么你必须在两个表上都实现安全性。安全性不会自动级联。

这里有一个简单的示例。首先，你需要创建将充当谓词的函数。此函数将根据当前用户名过滤行：

```
CREATE FUNCTION Security.fn_user_security_predicate(@Username AS sysname)
RETURNS TABLE WITH SCHEMABINDING AS
RETURN SELECT 1 AS result WHERE @Username = USER_NAME();
```

接下来，你将创建安全策略本身，将其绑定到谓词函数和你想要保护的表：

```
CREATE SECURITY POLICY Product.ProductSecurityPolicy
ADD FILTER PREDICATE Security.fn_user_security_predicate(
    CAST(USER_NAME() AS sysname)
)
ON Product.DimProduct;
```

通过这两个语句，你创建了一个策略，确保用户只能查看 DimProduct 表中 UserName() 与其自身匹配的行。

> **NOTE**
> 
> Fabric 仓库中的行级安全是在查询时评估的。这意味着每次用户运行查询时，安全谓词都会作为过滤器应用。这与 Power BI 语义模型中的情况不同，在语义模型中，安全角色在模型刷新和部署时应用，然后根据分配给用户的角色过滤数据。

此过程涉及创建函数，然后创建策略，最后将其应用于表。也可以将多个策略添加到单个表。这不仅仅是单个安全策略定义访问。所有作用于该表的策略的组合才定义访问。

这种方法的优点是它非常灵活，你可以用它来实现非常复杂的安全模型。缺点是它要求你对 T-SQL 和仓库本身的安全模型有深入的理解。

## Column-Level Security in a Warehouse

使用仓库，你也可以显式处理列级安全。

虽然行级安全是关于过滤用户可以看到哪些行，列级安全是关于控制他们可以访问哪些列。你通过使用标准的 SQL GRANT、REVOKE 和 DENY 语句来处理此事：GRANT 让你授予用户对列的访问权限，REVOKE 让你移除 GRANT 语句，而 DENY 让你明确拒绝用户对表中列的访问。这意味着你可以授予特定用户或用户组访问表的特定列的权限。例如，你可能希望授予一组用户访问像 CreditCardNumber 这样敏感列的权限，或根据他们是否需要查看而将其隐藏。

这是一种直接的方法，但它要求你对数据模型和用户角色进行仔细分析，因为你必须显式定义每个用户和他们应有权访问的每列。这种方法功能强大，但在你拥有大量表和用户时，管理起来可能有点负担。

以下是授予和拒绝对列访问的简单代码示例：

```
-- Grant SELECT permission on the DimProduct table to a user
GRANT SELECT ON Product.DimProduct TO [john.doe@contoso.com];
-- Deny SELECT permission on the Cost column to the same user
DENY SELECT ON OBJECT::Product.DimProduct(Cost) TO [john.doe@contoso.com];
```

此过程与处理行级安全的方式非常不同。对于列级安全，不需要函数或安全策略。你使用简单的 GRANT 或 DENY 语句。关键是要非常精确并确保

