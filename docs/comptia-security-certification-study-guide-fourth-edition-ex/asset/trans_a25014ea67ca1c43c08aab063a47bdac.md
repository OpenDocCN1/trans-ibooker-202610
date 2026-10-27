

- 3. 在屏幕左侧展开你的服务器，然后右键单击数据库文件夹并选择新建数据库。

- 4. 输入数据库名称 SecPlus_McgrawHill，然后单击确定。

- 5. 单击工具栏上的新建查询按钮并输入以下代码：

```
--Switch to the database
Use SecPlus_McgrawHill

-- Create a customer table to store customer data

CREATE TABLE [dbo].[Customer] (
    [CustomerID] [int] IDENTITY(1,1) NOT NULL,
    [LastName] [nvarchar](50) NULL,
    [FirstName] [nvarchar](50) NULL,
    [Email] [nvarchar](50) NULL,
    [EncryptedCreditCardNumber] [varbinary](128) NULL,
    CONSTRAINT [PK_Customer] PRIMARY KEY CLUSTERED (
    [CustomerID] ASC
    )
)
```

- 6. 要加密表中的数据，你必须首先创建数据库主密钥（如果尚未存在），然后创建证书以保护对称密钥。输入并执行以下代码以创建主密钥：

```
--Create a database master key for encryption of keys
use SecPlus_McgrawHill
if not exists
    (select * from sys.symmetric_keys where symmetric_key_id = 101)
    create master key encryption by password='Pa$$w0rd'

--Then create a certificate to encrypt the symmetric key
create certificate CustomerCreditCardCert
with subject = 'Customer Credit Card Number Encryption Key'
GO
--Create a symmetric key that uses AES
create symmetric key CustomerCreditCardKeyAES with algorithm=AES_256
encryption by certificate CustomerCreditCardCert;
GO
```

- 7. 现在你已准备好在 Customer 表中存储数据。在开始插入记录之前，你需要打开对称密钥以加密信用卡号码。在查询窗口中输入并执行以下命令：

```
--Open key so that we can encrypt data with it
open symmetric key CustomerCreditCardKeyAES
    decryption by certificate CustomerCreditCardCert;

--insert records
insert into Customer (
    Lastname,
    Firstname,
    Email,
    EncryptedCreditCardNumber
) Values (
    'Clarke',
    'Glen',
    'gleneclarke@hotmail.com',
    encryptbykey(key_guid('CustomerCreditCardKeyAES'),'3695-3445-9227',
    1,HASHBYTES('SHA1',convert(varbinary,9899))) -- function to encrypt data
)

insert into Customer (
    Lastname,
    Firstname,
    Email,
    EncryptedCreditCardNumber
) Values (
    'Dixon',
    'Sean',
    'sdixon@email.com',
    encryptbykey(key_guid('CustomerCreditCardKeyAES'),'5622-3498-3467',
    1,HASHBYTES('SHA1',convert(varbinary,9899)))--function to encrypt data
)
-- close encryption key now that data is written
close symmetric key CustomerCreditCardKeyAES
```

- 8. 为了证明数据已加密，对 Customer 表执行选择操作：

```
-- To view customer data. Note the credit card is encrypted
select * from customer;
```

![](img/3e3af33e2c753e920c09ecd40b5a457a_532_0.png)

- 9. 要读取信用卡信息，你必须打开密钥然后对 EncryptedCreditCardNumber 字段使用 decryptbykey 函数。输入以下代码以查看信用卡信息：

```
-- To decrypt credit card data and view customer data
open symmetric key CustomerCreditCardKeyAES
    decryption by certificate CustomerCreditCardCert;

select customerid, lastname, firstname, email,
    convert(varchar,decryptbykey(encryptedcreditcardnumber,1,HASHBYTES('SHA1',
convert(varbinary,9899)))) as 'CCNumber' from customer

Close symmetric key CustomerCreditCardKeyAES;
```

![](img/3e3af33e2c753e920c09ecd40b5a457a_533_0.png)

## 账户限制

访问控制的另一个重要部分是对用户账户设置限制。第一个重要限制是确保所有员工都有用于登录的用户账户，并且这些用户账户使用强密码。

## 账户过期

如果你是为临时员工创建的账户，你也可以为账户实施账户过期。例如，会计师事务所可能会决定在税务季节雇佣一名新的临时会计，但在税务季节结束后员工就会被解雇。在网络上创建此账户时，请务必设置账户过期日期（见图 11-4），这样在离职当天，账户将不再有效。

**FIGURE 11-4**

设置用户账户的账户过期时间

![](img/3e3af33e2c753e920c09ecd40b5a457a_534_0.png)

## 时间限制

对用户账户可以设置限制的另一个例子是账户登录的时间限制。大多数环境允许你将员工允许登录系统的时间作为用户账户的属性进行指定（见图 11-5）。这样做的好处是，你可以确保员工在非工作时间不会访问网络资源。

**FIGURE 11-5**

设置登录时间以控制用户可以登录网络的时间

![](img/3e3af33e2c753e920c09ecd40b5a457a_534_1.png)

该命令将检索 Active Directory 账户信息，例如账户名称、账户过期时间、密码信息和工作站限制。

以下代码列表显示了 Tanya 用户账户的账户详细信息：

```
C:\>net user tclarke /domain
User name                  tclarke
Full Name                  Tanya Clarke
Comment
User's comment
Country code               000 (System Default)
Account active             Yes
Account expires            Never

Password last set          2/14/2021 7:11 PM
Password expires           Never
Password changeable        2/15/2021 7:11 PM
Password required          Yes
User may change password   Yes

Workstations allowed       All
Logon script               users.bat
User profile
Home directory             \\win2003\users\tanya
Last logon                 5/20/2021 5:33 AM
Logon hours allowed        Monday 8:00 AM - 6:00 PM
                           Tuesday 8:00 AM - 6:00 PM
                           Wednesday 8:00 AM - 6:00 PM
                           Thursday 8:00 AM - 6:00 PM
                           Friday 8:00 AM - 6:00 PM

Local Group Memberships
Global Group memberships   *Domain Users          *Marketing
The command completed successfully.
C:\>
```

## 账户策略执行

你应该熟悉与访问控制相关的许多概念和最佳实践。其中一些主题在本章中已详细讨论过，但我想对在组织内实施账户策略的最佳实践进行总结。

> exam Watch
> Security+ 考试要求你了解接下来列出的不同类型的账户策略，尤其是地理围栏、地理标记和地理定位。

- **共享账户** 必须确保所有用户都有自己的账户，以便可以唯一应用权限并审计每个人的操作。
- **凭证管理** 确保你拥有中央凭证管理工具以及用户必须多久更改一次密码的策略。
- **组策略** 应使用组策略将安全设置部署到网络上的所有服务器和客户端。
- **密码复杂度** 确保对系统上的任何账户和网络访问使用复杂密码。这些密码比非复杂密码更难破解。
- **过期** 确保密码在一段时间后过期并且用户必须设置新密码。此外，请确保为创建的任何临时用户账户设置过期日期。
- **恢复** 确保你能够恢复任何被删除的账户或重置任何忘记的密码。
- **禁用** 确保禁用任何不必要的账户并在员工休假时禁用账户。
- **锁定** 配置账户锁定策略，以便如果在三次尝试后输入错误密码，账户将被锁定（无法使用），直到管理员将其解锁。
- **密码历史** 一个重要的密码设置是密码历史选项——确保你的系统通过保留之前的密码来防止用户重复使用相同的密码。
- **密码重用** 确定你的组织对重用密码的政策并确保将密码历史配置为执行该政策。
- **密码长度** 大多数组织要求密码最小长度为八个字符。
- **禁止通用账户** 与前面提到的共享账户点类似，确保没有为用户或系统共享而创建的通用账户，因为这使得审计操作变得困难。
- **网络位置** 你的公司可能希望实施访问控制策略，限制从特定网络位置访问特定资源。例如，为了访问敏感服务器，用户必须连接到安全网络。前面提到的另一个例子是，在 Azure 中你可以根据来自特定国家的 IP 地址阻止访问。此外，像 Netflix 这样的流媒体服务可以根据你所在的国家阻止对内容的访问。
- **地理围栏** 地理围栏是一项功能，允许软件管理员定义创建边界（或虚拟围栏）的 GPS 坐标。当运行该软件的设备超出边界时，会触发警报。

