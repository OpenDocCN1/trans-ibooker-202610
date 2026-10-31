

## 敏感数据存储和传输中的常见缺陷

作为一名渗透测试人员，在 Web 应用程序中需要重点关注的重要事项之一就是它们如何存储和传输敏感信息。如果数据以明文形式传输或以这种方式存储，应用程序的所有者可能会面临重大的安全问题。

如果敏感信息，例如密码或信用卡数据，以明文形式存储在数据库中，利用 SQL 注入漏洞或通过其他任何方式获得服务器访问权限的攻击者将能够读取此类信息并直接从中获利。

有时，开发人员会自行实现混淆或加密机制ss of applications must include the secure flag in cookies and the HTTP Strict-Transport-Security (HSTS) header to prevent SSL Strip attacks.

有关 TLS 配置的更多信息可在 https://www.owasp.org/index.php/Transport_Layer_Protection_Cheat_Sheet 找到。

密码绝不能以明文形式存储，使用加密算法来保护它们也是不明智的。相反，应使用单向加盐哈希函数。PBKDF2、bcrypt 和 SHA-512 是推荐的替代方案。不建议使用 MD5，因为现代 GPU 每秒可计算数百万个 MD5 哈希，使得使用高端计算机在数小时或几天内破解任何少于十个字符的密码成为可能。OWASP 在此主题上也有一个有用的备忘单，网址为 https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet。

对于需要可恢复的敏感信息，例如支付信息，应使用强加密算法。AES-256、Blowfish 和 Twofish 是不错的选择。如果可以使用非对称加密，例如 RSA，则应优先选择该方式（https://www.owasp.org/index.php/Cryptographic_Storage_Cheat_Sheet）。

避免使用自定义实现或创建自定义算法。依靠已被使用、测试并多次遭受攻击的方案要好得多。

