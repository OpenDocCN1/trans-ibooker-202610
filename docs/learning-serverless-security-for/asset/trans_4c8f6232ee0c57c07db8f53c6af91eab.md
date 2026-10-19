

### 3. 在你的主目录中创建并激活一个 Python 虚拟环境，然后安装 Semgrep：

```
cd ~
    
python3 -m venv venv
source venv/bin/activate
    
pip install semgrep
```

### 4. 检查已安装的 Semgrep 版本：

```
semgrep --version
```

你应该得到以下版本（或相近的版本号）：

```
...
1.141.0
```

#### Caution

运行 Semgrep 时，你可能会看到关于已弃用 API 的警告，例如某些 Python 包如 `opentelemetry-instrumentation` 使用的 `pkg_resources`。这意味着该 API 可能在未来的 Python 或 Setuptools 版本中被移除，或在某些版本中已被移除。为了管理潜在的弃用问题，请确保在安装和运行时监控警告，并将依赖项固定到稳定版本（例如，`setuptools<81`）。

### 5. 运行以下命令克隆官方 Semgrep 规则仓库：

```
git clone https://github.com/semgrep/semgrep-rules.git
```

### 6. 列出 `semgrep-rules/javascript` 下的所有目录：

```
ls -1F semgrep-rules/javascript
```

这会给出一个目录列表，类似于以下内容：

```
ajv/
angular/
apollo/
argon2/
    
...
    
vue/
wkhtmltoimage/
wkhtmltopdf/
xml2json/
```

#### Note

到你阅读本书时，你可能会得到不同的目录集合。

### 7. 准备就绪后，使用 `javascript` 目录中的规则对 `evaluate.js` 运行 Semgrep：

```
semgrep --config=semgrep-rules/javascript \
   vulnerable-function/src/functions/evaluate.js
```

这会产生以下输出：

```
  ...
    
┌────────────────┐
│ 1 Code Finding │
└────────────────┘
    
    vulnerable-function/src/functions/evaluate.js
    ❯❱ semgrep-rules.javascript.browser.security.eval-detected
          Detected the use of eval(). eval() can be dangerous if used to
          evaluate dynamic content. ...
    
           13┆ result = eval(expression);
    
┌──────────────┐
│ Scan Summary │
└──────────────┘
Scan completed successfully.
 • Findings: 1 (1 blocking)
 • Rules run: 171
 • Targets scanned: 1
 • Parsed lines: ~100.0%
 • No ignore information available
Ran 171 rules on 1 file: 1 finding.
```

Semgrep 在 `evaluate.js` 中检测到 `eval()` 的使用，`semgrep-rules.javascript.browser.security.eval-detected` 规则将其标记为潜在的代码注入漏洞。

### 8. 查看检测不安全 `eval()` 用法的 Semgrep 规则：

```
cat semgrep-rules/javascript/browser/security/eval-detected.yaml
```

这会输出 Semgrep 规则定义，包括其 ID、消息、元数据、受影响的语言、严重程度以及用于检测不安全 `eval()` 用法的模式：

```
rules:
- id: eval-detected
  message: >-
    Detected the use of eval(). eval() can be dangerous if used to ...
  metadata:
    cwe:
    - "CWE-95: Improper Neutralization of Directives in Dynamically
       Evaluated Code ('Eval Injection')"
    
    ...
    
    likelihood: LOW
    impact: MEDIUM
    confidence: LOW
    references:
    - https://owasp.org/Top10/A03_2021-Injection
  languages:
  - javascript
  - typescript
  severity: WARNING
  patterns:
  - pattern-not: eval("...")
  - pattern: eval(...)
```

### 9. 要开始使用自定义规则，请为你的自定义 Semgrep 规则创建一个目录，然后设置空文件（`no-eval.yml` 和 `no-hardcoded-keys.yml`）用于你的自定义规则：

```
cd ~
    
mkdir -p custom-rules
    
touch custom-rules/no-eval.yml
touch custom-rules/no-hardcoded-keys.yml
```

### 10. 在 Cloud Shell 代码编辑器中打开 `custom-rules/no-eval.yml`。用以下规则定义替换文件内容：

```
rules:
  - id: no-eval
    patterns:
      - pattern: eval($EXPR)
    message: "Avoid using eval() — dangerous with user input."
    languages: [javascript, typescript]
    severity: ERROR
```

#### Caution

请确保在进入下一步之前保存你的更改。

### 11. 在 Cloud Shell 代码编辑器中打开 `custom-rules/no-hardcoded-keys.yml`。用以下规则定义替换文件内容：

```
rules:
  - id: hardcoded-secret-detection
    languages: [javascript, typescript]
    message: "Possible hardcoded secret or API key"
    severity: ERROR
    pattern: |
      const $VAR = "$SECRET"
    metadata:
      category: security
    condition:
      metavariable-regex:
        metavariable: $VAR
        regex: (?i)(key|secret|token|password)
```

### 12. 使用你的自定义规则文件对 `evaluate.js` 文件运行 Semgrep：

```
semgrep --config=custom-rules \
   vulnerable-function/src/functions/evaluate.js
```

这会产生以下输出：

```
┌─────────────────┐
│ 2 Code Findings │
└─────────────────┘
    
    vulnerable-function/src/functions/evaluate.js
   ❯❯❱ custom-rules.hardcoded-secret-detection
          Possible hardcoded secret or API key
    
            3┆ const HARDCODED_KEY = "67890ghijkl";
    
   ❯❯❱ custom-rules.no-eval
          Avoid using eval() — dangerous with user input.
    
           13┆ result = eval(expression);
    
    
    
┌──────────────┐
│ Scan Summary │
└──────────────┘
✅ Scan completed successfully.
 • Findings: 2 (2 blocking)
 • Rules run: 2
 • Targets scanned: 1
 • Parsed lines: ~100.0%
 • No ignore information available
Ran 2 rules on 1 file: 2 findings.
```

此时，你应该对如何使用 Semgrep 检测无服务器应用程序代码中的安全问题有了很好的了解。请记住，虽然 Semgrep 和其他自动化扫描工具可以捕获许多常见漏洞，但它可能无法识别代码中的每个安全问题。你应该将自动化扫描与人工代码审查相结合，以捕获自动化可能遗漏的问题。

# Securing Serverless Application Code from Code Injection

在前一节中，你学习了如何使用 Semgrep 等工具检测无服务器函数代码中 `eval()` 的不安全使用。此外，你还探索了创建自定义规则以自动检测代码中的硬编码密钥，从而最大程度地降低凭据意外泄露的风险。在本节中，你将通过用更安全的 expr-eval 库替换 `eval()` 来保护你的代码。在对函数代码进行更改后，你将重新运行 Semgrep 扫描以验证之前检测到的问题是否已解决。

让我们逐步了解更新和保护你的无服务器函数代码的步骤：

### 1. 切换回浏览器窗口和 Serverless Lab 账户的 Cloud Shell 终端。

### 2. 安装 expr-eval 包，该包提供了一种安全地解析和评估数学表达式的方法：

```
cd ~/vulnerable-function
    
npm install expr-eval
```

这会产生以下输出：

```
added 1 package, and audited 91 packages in 1s
    
16 packages are looking for funding
  run `npm fund` for details
    
found 0 vulnerabilities
```

#### Note

本章中的动手示例假设你将使用 expr-eval 库的版本 `^2.0.2`。有关更多信息，你可以访问 [官方 GitHub 仓库](https://oreil.ly/1cDim)。

### 3. 在代码编辑器中打开 `vulnerable-function/src/functions/evaluate.js`。用以下内容替换文件的内容：

