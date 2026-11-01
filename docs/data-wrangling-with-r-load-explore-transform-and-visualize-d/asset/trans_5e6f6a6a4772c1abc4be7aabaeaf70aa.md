
-   **SecretNamePrefix**：包含源数据库用户名和密码的 **SecretsManager** 密钥的名称。对于我们的测试，我们将直接在连接字符串中提供它们。但对于生产部署，强烈建议使用具有密码轮换的密钥管理器以实现高效的安全性。因此，在此字段中输入 **dummysecret** 值。
-   **Spillbucket**：提供 Lambda 函数可用于存储超过其内存限制的数据的 S3 存储桶名称。此功能可有效防止较大查询结果导致的 Lambda 内存错误。
-   **DefaultConnectionString**：按此格式提供连接字符串：
-   **DisableSpillEncryption**：**False**（除非您不想在 S3 中加密溢出数据）。
-   **LambdaFunctionName**：提供与 Athena 数据源连接器相同的名称以便于识别。在我们的例子中，这是 **wranglerathena**。
-   **LambdaMemory** 和 **LambdaTimeout**：除非您想更改它们，否则保留默认最大值。
-   **PermissionsBoundaryARN**：对于我们的用例，请将其留空。
-   **SecurityGroupIds**：提供您想要与 Lambda 函数关联的安全组 ID。安全组需要具有访问 MySQL 数据库的规则。
-   **SpillPrefix**：将其保留在默认值 **athena-spill**。
-   **SubnetIds**：提供 Lambda 函数将启动以连接到 RDS MySQL 的子网 ID。提供 RDS MySQL 实例所在的子网 ID，或可访问数据库连接的子网列表。

```
mysql://jdbc:mysql://<<database endpoint>>:3306/noaa?user=<<username>>&password=<<password>>##Replace << >> values in the above string with correct values for your use case
```

应用程序创建完成后，选择上一步创建的 Lambda 函数并在 Amazon Athena 中完成数据源创建。

## 联邦数据源的工作验证

**方法 1**：在 Athena 中不使用数据源选项，通过直接按名称引用 Lambda 函数执行查询，如下所示。如果一切设置正确，您应该能够看到源表中的数据，如下所示：

```
select * from "lambda:wranglerathena".noaa.noaa_country;
```

SQL 第 71 行，第 1 列

再次运行 解释 取消 清除 创建

## 查询结果

完成

队列时间：446 ms

## 结果 (100+)

| # | countrycode | countryname | parti |
|---|-------------|-------------|-------|
| 1 | AC | Antigua and Barbuda | * |
| 2 | AE | United Arab Emirates | * |
| 3 | AF | Afghanistan | * |

Figure 7.31: Amazon Athena 通过 Lambda 函数直接从 MySQL 查询国家表

## 方法 2：使用上一步创建的 Amazon Athena 数据源执行查询。在通过此选项验证之前，我们需要在无服务器应用程序创建的 Lambda 函数中进行以下更改：

-   导航到 Lambda 控制台并选择配置部分中使用的函数名称——wranglerathena。
-   选择配置然后选择环境变量。现在，复制默认键名中的环境变量并创建另一个名为 wranglermysql_connection_string 的键，如以下屏幕截图所示。此键名应为 athenadatasource_connection_string 格式：

| Key | Value | |
|-----|-------|---|
| default | mysql://jdbc:mysql://awswrangler.ci2nx | Remove |
| disable_spill_encryption | false | Remove |
| spill_bucket | athenaresults-sankar | Remove |
| spill_prefix | athena-spill | Remove |
| wranglermysql_connection_string | mysql://jdbc:mysql://awswrangler.ci2nx | Remove |
| Add environment variable |  | |

Figure 7.32: 更新联邦 Lambda 函数中的配置

-   然后，单击 **Save** 以更新我们对 Lambda 函数所做的新的环境变量更改。

现在，导航到 Amazon Athena 控制台并选择 `wranglermysql` 作为数据源，您将能够看到新创建的表，如以下屏幕截图所示：

![](img/ec1933a597ce63afa3bcf7496364f412_244_0.png)

Figure 7.33: 在 Amazon Athena 控制台中从联邦源访问表列表

您可以使用 datasource.database.tablename 格式查询表并验证连接器的运行情况，如下所示：

![](img/ec1933a597ce63afa3bcf7496364f412_245_0.png)

Figure 7.34: 通过 Athena 控制台和已注册的数据源查询 RDS MySQL 中的国家表

现在，我们将探讨如何使用 Athena 查询将 MySQL 中的国家数据与可在 Amazon S3 中获得的 noaa_parquet 表结合起来。

## 使用数据联邦丰富数据

为了演示 Athena 联邦的好处，让我们考虑这样一种情况：您的经理要求您（作为公司的数据分析师）尽快找到本世纪南极洲和其他 10 个国家的最低温度。您作为公司的数据分析师知道您必须查询 noaa_data_parquet 表，但您不确定这些国家的国家代码值。您还知道 MySQL 中有一个包含此信息的表，并且不想等待数据工程团队将此表移至 Amazon S3 并在其上创建数据目录以便在 Amazon Athena 中查询。

让我们创建一个联邦查询，将 Amazon S3 和 RDS MySQL 中的数据连接起来，并为我们的用例提供所需的输出。我们将执行以下查询以在数据源之间无需数据迁移的情况下组合所需的数据集：

```
select min(data_value) as min_temp from AwsDataCatalog.wranglerathena.noaa_data_parquet t1
inner join wranglermysql.noaa.noaa_country t2 on t1.countrycode=t2.countrycode where
```

```
trim(t2.countryname)='Antarctica' and date_field>cast('1900-01-01' as date) and element='TMIN'
```

```
select min(data_value) as min_temp from AwsDataCatalog.wranglerathena.noaa_data_parquet t1 inner join wranglermysql.noaa.noaa_country t2 on t1.countrycode=t2.countrycode where trim(t2.countryname)='Antarctica' and date_field>cast('1900-01-01' as date) and element='TMIN'
```

![](img/ec1933a597ce63afa3bcf7496364f412_246_0.png)

Figure 7.35: 使用联邦连接的 Amazon Athena 查询以识别南极洲的最低温度

这展示了 Athena 在不实际移动数据的情况下从多个来源丰富数据的能力。这在很多时候对各个团队都非常有用。

现在我们已经了解了通过 Amazon Athena 丰富数据的各种方法，我们将在下一节中探讨如何使用 Athena 查询设置数据质量管道。

## 使用 Athena 设置无服务器数据质量管道

数据质量验证是数据整理管道中非常重要的一步，确保用于分析和可视化的数据的准确性。我们将在本节中探讨如何通过 Amazon Athena 执行数据质量验证。

## 在 Athena 中实现数据质量规则

让我们考虑我们想要在 NOAA 天气数据集中验证的规则。以下内容仅是对一些数据质量规则的高层次表示，并不是天气数据集的全面规则集：

-   1. 当国家代码为 US 时，state 列应具有两个字符值。
-   2. date 字段不应有任何未来日期值，否则会导致测量不正确。
-   3. 验证 element 列是否仅接受文档中提供的列表中的值。

我们可以有更多规则来确保更好的数据质量，但前述规则足以让我们演示使用 Amazon Athena 进行数据质量验证。我们将首先设计查询，然后在 Athena 中执行它们以确保语法和语义正确性。

## 数据质量规则 1：对于州代码列验证，使用此处给出的查询：

```
--Identify incorrect state codes when Country is US
select * from ghnd_stations where substring(ID,1,2)='US'
and length(state)<>2
```

在 Athena 中执行查询并验证我们没有任何违反数据质量规则的记录，如以下屏幕截图所示。这确保了当前数据没有任何数据错误，并且验证了我们的查询按预期正确工作：

![](img/ec1933a597ce63afa3bcf7496364f412_247_0.png)

Figure 7.36: Amazon Athena 数据质量以识别州代码不等于 2 个字符的记录

## 数据质量规则 2：为了验证我们没有任何来自未来日期的天气观测，我们将使用下一个查询。这确保我们没有任何来自未来日期的不正确观测：

```
--Identify records that have future dated values
select * from noaa_data_parquet where date_field > current_date
```

我们可以以类似的方式执行并验证之前的查询，并确保我们没有任何数据质量问题记录。

## 数据质量规则 3：我们将执行下一个给出的查询以验证观测是否仅针对有效维度进行。这是通过对 element 列进行验证来完成的。

我们通过使用 NOAA 数据集 README 文件 (https://docs.opendata.aws/noaa-ghcn-pds/readme.html) 中的数据获得了值列表。我们将值复制到 Excel 表格中，使用 = 分隔符将其拆分为两列，然后在以下查询中使用它们：

```
--Identify records with invalid values in element column
select element,s_flag,max(date_field) as latest_date,count(*) as cnt from noaa_data_parquet where countrycode='US' and element not in ('PRCP','SNOW','SNWD','TMAX','TMIN','ACMC','ACMH','ACSC','ACSH','AWDR','AWND','DAEV','DAPR
```
