

```
LAMBDA_SG_ID=$(aws ec2 create-security-group \
  --group-name lambda-private-sg \
  --description "SG for Lambda in private subnet" \
  --vpc-id $VPC_ID \
  --region $REGION \
  --query 'GroupId' \
  --output text)
```

此安全组将用于控制 Lambda 函数的网络流量。

### 注意

此安全组尚未附加到 Lambda 函数。你将在后续步骤中更新 Lambda 函数的配置以使用此安全组。

7. 删除 Lambda 函数安全组中的任何现有出站规则：

```
aws ec2 revoke-security-group-egress \
  --group-id $LAMBDA_SG_ID \
  --protocol -1 \
  --port all \
  --cidr 0.0.0.0/0 \
  --region $REGION || true
```

你应得到以下输出：

```
{
    "Return": true,
    "RevokedSecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-...",
            "GroupId": "sg-...",
            "IsEgress": true,
            "IpProtocol": "-1",
            "FromPort": -1,
            "ToPort": -1,
            "CidrIpv4": "0.0.0.0/0"
        }
    ]
}
```

这可确保默认情况下不允许任何出站流量。

8. 在你的 VPC 中创建一个名为 `endpoint-sg` 的安全组：

```
ENDPOINT_SG_ID=$(aws ec2 create-security-group \
  --group-name endpoint-sg \
  --description "SG for VPC Endpoint" \
  --vpc-id $VPC_ID \
  --region $REGION \
  --query 'GroupId' \
  --output text)
```

此安全组将用于控制流向 VPC 端点的流量。

### 注意

此安全组尚未附加到任何 VPC 端点，因为端点本身尚未创建。

9. 删除 VPC 端点安全组的任何现有出站规则，以便默认情况下不允许出站流量：

```
aws ec2 revoke-security-group-egress \
  --group-id $ENDPOINT_SG_ID \
  --protocol -1 \
  --port all \
  --cidr 0.0.0.0/0 \
  --region $REGION || true
```

10. 允许通过端口 443 的入站 HTTPS 流量从 Lambda 函数的安全组进入 VPC 端点安全组：

```
aws ec2 authorize-security-group-ingress \
  --group-id $ENDPOINT_SG_ID \
  --protocol tcp \
  --port 443 \
  --source-group $LAMBDA_SG_ID \
  --region $REGION
```

这将允许 Lambda 函数通过 VPC 端点在 AWS 网络内安全、私密地访问 AWS 服务。

11. 允许从 VPC 端点安全组和 Lambda 函数安全组通过端口 443 的出站 HTTPS 流量到所有 IP 地址：

```
aws ec2 authorize-security-group-egress \
  --group-id $ENDPOINT_SG_ID \
  --protocol tcp \
  --port 443 \
  --cidr 0.0.0.0/0 \
  --region $REGION

aws ec2 authorize-security-group-egress \
  --group-id $LAMBDA_SG_ID \
  --protocol tcp \
  --port 443 \
  --cidr 0.0.0.0/0 \
  --region $REGION
```

### 注意

即使这些安全组规则允许出站 HTTPS 连接，实际的出站访问仍取决于 VPC 的网络配置。由于 VPC 将配置为不包含互联网网关或 NAT 网关，Lambda 函数将无法访问外部端点，除非是配置的 VPC 端点。

12. 在你的 VPC 中创建 Secrets Manager 的接口 VPC 端点，并将其与之前步骤中创建的子网和安全组关联：

```
aws ec2 create-vpc-endpoint \
  --region "$REGION" \
  --vpc-id "$VPC_ID" \
  --vpc-endpoint-type Interface \
  --service-name "com.amazonaws.${REGION}.secretsmanager" \
  --subnet-ids "$SUBNET_ID" \
  --security-group-ids "$ENDPOINT_SG_ID" \
  --private-dns-enabled \
  --query 'VpcEndpoint.VpcEndpointId' \
  --output text
```

### 注意

有了此接口 VPC 端点，即使 VPC 没有互联网连接和 NAT 网关，Lambda 函数也可以安全地访问 Secrets Manager。

至此，你已成功设置并配置了一个没有出站访问的 VPC。请记住，VPC 配置取决于你的应用程序需求，并非所有无服务器应用程序都可以在没有出站访问的 VPC 中运行。例如，某些应用程序需要访问外部 API 或服务，这意味着你可能需要允许它们的选择性出站访问以便正常运行。在处理 VPC 中受限的出站流量时，你可以使用额外的服务和组件——例如 AWS Network Firewall 与 VPC 端点或 NAT 网关一起使用——以选择性地过滤、监控和控制出站流量，同时保持安全性。

# 将 Lambda 函数附加到具有受限出站访问的 VPC

现在 VPC 及其组件已就位，你可以配置 Lambda 函数在其中运行而无需互联网访问。在本节中，你将把 Lambda 函数附加到 VPC，配置其子网和安全组，并确认其在没有出站访问的情况下成功运行。

### 注意

本节假定你使用的是 Serverless Lab 账户，该账户托管易受攻击的无服务器实验室环境。

让我们逐步完成将 Lambda 函数连接到你刚刚创建的 VPC 的步骤：

1. 切回已打开 AWS Lambda 控制台的浏览器标签页。确保在继续下一步之前，已打开 AWS Lambda 函数 `lambda-0010` 的函数详情页面。

2. 导航到 Configuration 标签。选择 VPC 然后点击 Edit。

3. 在 Edit VPC 页面上，找到 VPC 部分。对于 VPC，选择 `NoOutboundVPC`。对于 Subnets，选择 `PrivateSubnet`。对于 Security Groups，选择 `lambda-private-sg`。然后点击 Save。

### 注意

点击 Save 按钮后，你应收到以下通知：`Updating the function lambda-0010`。等待更新完成后再继续。

4. 在 Test 标签上，点击 Test（在“Test event”下）。Lambda 函数预计会成功运行并产生以下日志集：

```
START RequestId: ... Version: $LATEST
event: {'queryStringParameters': {'statement': '1 + 1'}}
HARDCODED_KEY_01:  ABCDE
HARDCODED_KEY_02:  FGHIJ
CUSTOM_ENV_VAR_01:  KLMNO
CUSTOM_ENV_VAR_02:  PQRST
SECRETS MANAGER SECRETS:  {'secret1': 'secret-value1',
'secret2': 'secret-value2', 'secret3': 'secret-value3'}
END RequestId: ...
REPORT RequestId: ...   Duration: 2178.81 ms    Billed Duration: 2506 ms
Memory Size: 128 MB Max Memory Used: 86 MB  Init Duration: 326.40 ms
```

此时，Lambda 函数已配置为在 VPC 内运行，使用私有子网和安全组，并可通过 VPC 端点访问 AWS Secrets Manager，同时与互联网隔离。你现在可以继续创建并配置 API Gateway REST API 以供你的 Lambda 函数使用。

# 设置路由请求到 Lambda 函数的 API Gateway REST API

到目前为止，你一直在使用 Lambda 函数 URL 来调用你的函数。对于生产工作负载，你应考虑使用 Amazon API Gateway，用完全托管的 API 层替代直接的 Lambda 函数 URL。与函数 URL 相比，API Gateway 提供细粒度的访问控制，支持高级身份验证和授权机制，支持节流，支持请求验证和转换，并提供详细的日志记录和可观测性。虽然函数 URL 确实支持基于 IAM 的基本授权，但 API Gateway 为生产 API 提供了更大的灵活性和控制能力。

