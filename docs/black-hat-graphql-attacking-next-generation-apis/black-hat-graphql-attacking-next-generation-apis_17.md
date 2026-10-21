# INDEX

# 符号

& (ampersand), 208
* (asterisk), 203, 205
@ (at symbol), 54, 194
\ (backslash), 200
: (colon, for query aliasing), 50
{} (curly brackets), 47–48, 74–78, 176, 228
$ (dollar sign), 53
&& (double ampersand), 208
-- (double dash), 202
|| (double pipe), 208
" (double quotes), 197, 200
__ (double underscore), 78, 89–90, 94, 143, 150–151, 194
! (exclamation mark), 57
# (hash mark), 26
; (semicolon), 207–208
' (single quote), 197, 201
... (spread operator), 52–53
[] (square brackets), 7, 123
_ (underscore), 150

Apollo Client, 8
AppImage extension, 25
application programming interface (API), 1
  consumers, 1–2
  endpoint paths, 15–16
  pagination, 122, 133
  scanners, 16, 164
arguments, 43, 48–50, 191–193
array-based query batching, 122–127, 253–255
ash_nz, 257
Assetnote, 36
asterisk (*), 203, 205
Atlassian, 2
at symbol (@), 54, 194
attack surface, 41–42
  matrix, 98
audit logs, 47
authentication and authorization
  flaws, 69, 163
  GraphQL Modules, 169
  GraphQL Shield, 169–170
  HTTP basic authentication, 165–166
  in-band vs. out-of-band, 164
  IP-based allow listing, 171
  OAuth, 167–169
  schema directives, 170–171
authorization grant, 167
autocorrection, 147
automatic persisted queries (APQ), 99, 134–135
awk, 141

# A

abstract syntax tree (AST), 8
adjacent server, 213
Agoо, 255–256
aliases, 50–52
  overloading, 116–119
Altair, 25–28
Amazon Web Services (AWS), 143–144
ampersand (&), 208
anti-CSRF tokens, 223, 228, 233–234, 240–241
Apache, 135
Apache Skywalking, 264–265
Apache Software Foundation, 95, 264
API Security Top 10, 190

# B

backslash (\), 200
Base64, 166–167, 178

Bash
  automating detection with cURL, 80
  creating ID dictionaries, 184
  for loop, 84, 109
  formatting, 84
  merging text files, 154
  testing hosts, 109
BatchQL, 36, 126–127, 174, 232
batch requests, 99
blind SQL injection, 197
blind XSS, 213–214
Boolean-based SQL injection, 197
Boolean scalar type, 58
browser exploitation, 213
brute forcing, 173–177, 183–185
  argument brute forcing, 183
  CSRF token brute forcing, 234
  password brute forcing, 173, 176
bug bounty, 66, 74, 248, 257, 260, 274
Burp Suite, 32–33, 197–200
  BApp Store, 35
Byron, Lee, 148

C
caching, 134–135
canary GraphQL query, 85, 144, 172
central processing unit (CPU), 102, 128
cEWL, 154
circular relationships
  fragments, 109, 255–256
  introspection, 109, 251
  queries, 102, 109, 118
claims, 168
Clairvoyance, 33–34, 152–154
classic SQL injection, 196–197
Classless Inter-Domain Routing (CIDR), 193–194
code review, 210–211
colon, for query aliasing (:), 50
comma-separated values (CSV), 40, 176, 184
Commix, 37–38, 208–210
Common Weakness Enumeration (CWE), 102
consolidator API layer, 8
cookies
  authentication, 177
  exfiltration, 211
  session, 222, 240

cost analysis, 99, 128–130
cost analyzer, 128, 130
CrackQL, 40, 176–177, 183–184
credit-based rate limiting, 130
cross-site port attack (XSPA), 239
cross-site request forgery (CSRF)
  GET-based cross-site request forgery, 228–231, 267–268
  POST-based cross-site request forgery, 225–227
  prevention, 232–234
cross-site scripting (XSS), 188, 211–214
  blind, 213–214
  DOM, 214
  hunter, 214
  reflected, 211–212
  stored, 213–214
cross-site WebSocket hijacking (CSWSH), 46, 240–244
CS Money, 249
cURL, 24–25
curly brackets ({}), 47–48, 74–78, 176, 228

D
Damn Vulnerable GraphQL Application (DVGA), 29–31
  changing modes, 93
  installing, 28–29
  rolling back, 215
  security mechanisms, 93
data classification, 140
data field, 13, 74–75
datastore layer, 165
data types
  enums, 58–59
  inputs, 62–63
  interfaces, 61–62
  objects, 57–58
  scalars
    Boolean, 58
    Float, 58
    ID, 58
    Int, 58
    String, 58
  unions, 60–61
Debian, 22

debug mode, 99, 157–158
declarative query language, 6, 14
decorate a field, 54
dee_see, 38
defense in depth, 234
denial of service (DoS), 69, 102, 248–256
dirbuster wordlist, 84
directives
  overloading, 119–121
  query-level
    @defer, 56
    @include, 54–56
    @skip, 54–56
    @stream, 56
  schema-level
    @auth, 170–171, 181–182
    @computed, 56
    @cost, 129
    @deprecated, 54–56
    @hasRole, 171
    @protect, 171
    @specifiedBy, 54, 56
disabled introspection, 93, 142–144, 148
Docker, 28–30
Document Object Model (DOM), 214
dollar sign ($), 53
double ampersand (&&), 208
double dash (--), 202
double pipe (||), 208
double quotes (") , 197, 200
double underscore (_), 78, 89–90, 94, 143, 150–151, 194
Doyensec, 34
E
edge, 5
edit-distance algorithm, 146–148
Elasticsearch, 187
enumeration, 182–183, 261–262
enum keyword, 58
enums, 58–59
error-based SQL injection, 196
errors field, 16, 75
escaping, 196, 201
exclamation mark (!), 57
Exploit Database, 21
Export SDL feature, 57
extensions field, 35, 130–131, 156–157
EyeWitness, 39, 82–83
F
Facebook, 2–3, 188
Fedora, 22
field
  defined, 4, 47
  duplication, 113–116, 132–133, 252–253
  stuffing, 149–154, 183
  suggestions, 48, 99, 145–148
File Transfer Protocol (FTP), 235
fingerprinting, server, 68, 94–98
Flask, 296
Float scalar type, 58
fragment keyword, 52
fragments, 52–53, 112
freddd, 252
fuzzing, 37, 141, 155, 177, 197, 208
G
gateway layer, 165
gateway proxies, 136–137
GitHub, 2, 29, 148
GitLab, 2, 251–252, 256, 259–262, 267–268
Google Chrome, 1, 233
graphical user interface (GUI), 17–20
GraphQL
  clients
    Altair, 25–28
    Apollo Client, 8
    cURL, 24–25
    GraphQL Explorer, 14, 17, 55, 81–83, 194–195
    GraphQL Playground, 14, 17, 55, 81–83, 194–195, 265–267
  data types
    custom objects, 57
    enums, 58–59
    inputs, 62–63
    interfaces, 61–62
    scalars, 58
    unions, 60–61

GraphQL (continued)
endpoints
  integrated development
    environment, 73
  query, 73
errors, 15–16, 75–76, 85, 95, 154–157, 172–173
hacking tools
  BatchQL, 36, 126–127, 174, 232
  Burp Suite, 32–33, 197–200
  Clairvoyance, 33–34, 152–154
  Commix, 37–38, 208–210
  CrackQL, 40, 176–177, 183–184
  GraphQL Cop, 39, 127–128, 232
  graphql-path-enum, 38, 182–183
  GraphQL Threat Matrix, 66–67, 98, 132–135, 157
  Graphw00f, 35–36
  Netcat, 239, 241–244
  SQLmap, 203–204
implementations
  Agoo, 255–256
  Apollo Server, 72, 135, 142
  Ariadne, 72, 88
  AWS AppSync, 143–144
  Graphene, 72, 74, 88, 98
  graphql-go, 88
  graphql-java, 72, 88
  graphql-php, 68, 72, 88
  graphql-ruby, 72, 88, 131, 135
  Juniper, 72
  Sangria, 72, 95–96, 160
  Strawberry, 72
  Tartiflette, 72
  WPGraphQL, 253–255
  Yoga, 72
language
  aliases, 50–52
  arguments, 48–50, 191–193
  directives, 54, 56
  fragments, 52–53
  operation name, 46, 177–178, 194–195
  operation types, 43
  query anatomy, 42
  top-level fields, 43, 47
  variables, 53–54
responses, 74–76
specification, 2–3, 111
spread operator (...), 52–53
graphql-directive-auth, 170
GraphQL Foundation, 2
GraphQL Modules, 169
graphql-scalars library, 50
GraphQL Shield, 169–170
GraphQL Threat Matrix, 66–67, 98, 132–135, 157
GraphQL Voyager, 92, 106
GraphQL Working Groups, 53
GTFOBins, 211
H
H2, 264
HackerOne, 248, 258–260, 262–263
handshake, 240–241
hash-based message authentication code (HMAC), 168
hash mark (#), 26
haxta4ok00, 258
headless browser, 39, 81
hypervisor, 22–23
HyperText Markup Language (HTML)
  <a> tag, 223, 228–230
  crafted forms, 222–223, 225
  errors, 124
  <form> tag, 223, 229
  heading, 218
  <img> tag, 223, 229–231
  injection, 230
  <p> tag, 218
  <script> tag, 212, 217, 218, 229
  <title> tag, 77, 214
HyperText Transfer Protocol (HTTP)
  basic authentication, 165–166
  client, 7, 24
  GET, 3, 25, 44, 77, 79, 160, 212, 223–224, 228–233, 242, 262–263, 267
  HEAD, 94–95
  headers, 27–28, 93, 171
  methods, 15, 157
  POST, 3, 15, 79–80, 160, 225–229
  query parameters, 79
  request methods, 15
  response codes, 15–16, 172





+   我
+   ID 标量类型，58
+   带内身份验证和授权，164
+   信息泄露，69，139–140，261–262
+   通过错误消息的信息暴露，154–155
+   注入，69–70，187–188，190–195，262–265
+   输入/输出（I/O），128
+   InQL，34–35，107–108，140–141
+   不安全直接对象引用（IDOR），177
+   interface 关键字，61
+   内部服务器，189
+   内省，33，63，92，99，140，142，262
+     查询，33，142
+     系统类型
+       _Directive，63
+       _EnumValue，63
+       _Field，63，111
+       _InputValue，63
+       _Schema，63–64，142–145
+       _Type，63–64，143–144，150–151
+       _TypeKind，63，65
+   Int 标量类型，58
+   
+   J
+   JavaScript，2，8，50，81，211–219，227
+   JavaScript 对象表示法（JSON），10
+     GraphQL 响应，74–76
+       data 字段，13，74–75
+       errors 字段，16，75
+       extensions 字段，130–131，156–157
+       location 字段，155
+       message 字段，155
+       path 字段，156
+   Joaxcar，256
+   Jobert，262
+   Johnson，Donald B.，108
+   Johnson 算法，108
+   JSON Web Token（JWT），167–169，172，178–180
+   Jumbo-WJB，264
+   
+   K
+   Kali，23
+   键值存储，187
+   
+   L
+   实验室安全预防措施，22
+   轻量级目录访问协议（LDAP），235
+   限制
+     别名和基于数组的批处理，132
+     字段重复，132–133
+     查询深度，131–132
+     返回记录，133
+   负载均衡器，171
+   lowerCamelCase，150
+   Lua 语言，37
+   Lyon，Gordon（又名“Fyodor”），37
+   
+   M
+   Magento，252–253
+   恶意输入，188
+   中间人（MITM）攻击，46
+   元字段，78，89，143
+   微服务，9
+   Microsoft SQL Server，197
+   中间件，132
+   Mozilla Firefox
+     cookie 修改，87
+     开发工具，85
+   msdian7，260
+   mutation 关键字，6
+   mutations，6，44
+   mvm，250
+   MySQL，8，149，187，197，203，264
+   
+   N
+   Neo4j，187
+   Nginx，95，135，160
+   Nikto，81
+   Nmap 脚本引擎（NSE），37，76–77，79
+   节点包管理器（npm），267
+   节点和边，4
+   非空包装类型，57
+   规范化，133
+   Nuri，262

## O
+   - OAuth，167–169
+   - 对象限制覆盖，121–122
+   - 对象类型，4
+   - 攻击性安全，23
+   - 单向链接关系，5
+   - on 关键字，52
+   - OpenAPI，66
+   - OpenSearch，264
+   - 操作系统（OS）命令注入，205–210
+   - 操作名称，43，46–47，177–178，194–195
+   - 带外身份验证和授权，164
+   - 过度获取，9
+   - OWASP，70，189–190

## P
+   - 有效载荷，168
+   - 渗透测试，53，101，117–118
+   - 个人身份信息（PII），2，69，139，149，160–161，196，270
+   - PhantomJS，82
+   - 端口，78
+   - 端口扫描，37
+   - PortSwagger，32，44
+   - PostgreSQL，197，203
+   - 编程语言
+     - Golang，72，88
+     - Java，72，88，265
+     - JavaScript，2，8，50，81，211–219，227
+     - PHP，35，68，72，88
+     - Python，34，72–74，80，118，124，206，244
+     - Ruby，72，88，131，255，263
+     - Rust，72–73
+     - Scala，72–73，95，160
+     - TypeScript，8，72
+   - 受保护资源，167
+   - 公共服务器，189

## Q
+   - 查询
+     - 允许列表，133
+     - 批处理
+       - 基于别名的，50–52，173–175
+       - 基于数组的，122–127

## R
+   - 成本分析，99，128–131
+   - 深度限制，99，131–132
+   - 高度，133
+   - 关键字，6–7
+   - 解析器，3，7–8
+   - 追踪，250–251
+   - Rapid7，261
+   - React，81
+   - 侦察，71
+   - 递归查询，102，109，125
+   - Redis，187
+   - 参考实现，2，148
+   - 反射型跨站脚本（XSS），211–212
+   - 正则表达式
+     - 在线评估器，250
+     - ReDoS，50，249–250
+   - Relay，8
+   - 征求意见稿（RFC），3，15–16，165，168，178
+   - 请求伪造，211
+   - 解析器函数，3，8，210–211
+   - 资源所有者，167
+   - REST API，2，7–12，15，48，66
+   - 返回全部，192
+   - 反向代理，171
+   - Rivest-Shamir-Adleman（RSA），168
+   - 基于角色的访问控制，164
+   - 根类型，6
+   - Ruby，72，88，131，255，263

## S
+   - SameSite，232–233
+   - 清理，47，188，295
+   - scalar 关键字，58
+   - 标量类型，4，50，58，89
+     - Boolean，58
+     - Float，58
+     - ID，58
+     - Int，58
+     - String，58
+   - 模式，3–4
+     - 指令，170–171，181–182
+     - 联邦，9
+     - 拼接，9
+   - 模式定义语言（SDL），4，92–93

安全发展，148
段错误，112
选择集，47–48，74–78
Selenium，82
半盲 SSRF，235–236
分号（;），207–208
序列化，13，58
服务器消息块（SMB），235
服务器端请求伪造（SSRF），207，234–239
    预防，240
SHA-256，134
Shopify，257–258
简写查询语法，47–48
SIGINT 信号，116，120
签名，167–169，179–180
单引号（'），197，201
snake_case，150
快照，22
SOAP，70，249
SpectaQL，93
展开运算符（...），52–53
SQL 注入（SQLi），264–265
    自动化，203–205
    盲注，197
    基于布尔的，197
    经典，196–197
    基于错误的，196
    测试，197–203
    基于时间的，197，264
    联合查询，196
SQLite，202
方括号（[]），7，123
堆栈跟踪，158–160
Star Wars API（SWAPI），183
Stasinopoulos，Anastasios，37
状态更改操作，223
静态分析，128
存储型跨站脚本，213–214
存储的有效载荷，213–214
String 标量类型，58
强类型，8，193
结构化查询语言（SQL）
    引擎，201
    操作符，201
    查询，201，264
    表，201
Stupin，Nikita，34
subscription 关键字，6
订阅，6，44，231，240–244
Swagger，66
T
制表符分隔值（TSV），140
Tarjan 算法，108
TiDB，264
基于时间的 SQL 注入，197，264
超时，135
传输层安全（TLS），46，166
Truncer，Chris，39
Tsaturow，Ilya，34
Twitter，188
双向链接关系，5
type 关键字，4
TypeScript，8，72
类型填充，150–152
类型验证，8
U
Ubuntu，22
不受控资源消耗，102
获取不足，9
下划线（_），150
统一网关，9
联合查询 SQL 注入，196
union 关键字，60
联合，60–61
无序参数，49
V
验证，3，66
变量，53–54
Vazarkar，Rohan，39
VirtualBox，22–23
虚拟化，22
漏洞评估，16，94
W
Web 应用防火墙（WAFs），17，136，143–144，174
WebSocket，44–46，200，217，231，240–244，262
词表，74，81–84，152–154，176，184

WordPress，253
WPGrap hQL，253–255

X
X-Forwarded-For，171
X-Host，171
X-Originating-IP，171

X-Real-IP，171
XSS。参见 跨站脚本
XSS Hunter，214

Z
zerodivisi0n，262

*Black Hat GraphQL* 使用 New Baskerville、Futura、Dogma 和 TheSansMono Condensed 字体排版。

## RESOURCES

访问 https://nostarch.com/black-hat-graphql 获取勘误和更多信息。

来自 NO STARCH PRESS 的更多无废话书籍

## HACKING APIS
破坏 Web 应用程序编程接口
作者 COREY J. BALL
368 页，$59.99
ISBN 978-1-7185-0244-4

## BUG BOUNTY BOOTCAMP
寻找和报告 Web 漏洞指南
作者 VICKIE LI
416 页，$49.99
ISBN 978-1-7185-0154-6

## BLACK HAT GO
黑客和渗透测试人员的 Go 编程
作者 TOM STEELE，CHRIS PATTEN 和 DAN KOTTMANN
368 页，$39.95
ISBN 978-1-59327-865-6

## BLACK HAT PYTHON，2ND EDITION
黑客和渗透测试人员的 Python 编程
作者 JUSTIN SEITZ 和 TIM ARNOLD
216 页，$44.99
ISBN 978-1-7185-0112-6

## HOW TO HACK LIKE A GHOST
入侵云端
作者 SPARC FLOW
264 页，$34.99
ISBN 978-1-7185-0126-3

## ATTACKING NETWORK PROTOCOLS
捕获、分析与利用的黑客指南
作者 JAMES FORSHAW
336 页，$49.95
ISBN 978-1-59327-750-5

电话：
800.420.7240 或
415.863.9900

电子邮件：
SALES@NOSTARCH.COM

网站：
WWW.NOSTARCH.COM

![](img/2f244b00cd9f8f37cfc22f928aaa471a_312_0.png)
从未有过世界如此依赖互联网来保持联系和获取信息。这使得电子前沿基金会的使命——确保技术支持所有人的自由、正义和创新——比以往任何时候都更加紧迫。

30 多年来，EFF 通过行动主义、在法庭上以及通过开发软件来克服对隐私、安全和言论自由的障碍，为科技用户而战。这种奉献精神通过黑暗赋予我们所有人力量。在你的帮助下，我们可以朝着更光明的数字未来航行。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_312_1.png)





## GraphQL 安全权威指南

Black Hat GraphQL 适合任何有兴趣学习如何借助进攻性安全测试来攻破和保护 GraphQL API 的人。无论你是渗透测试员、安全分析师还是软件工程师，你都将学习如何攻击 GraphQL API、制定加固流程、将自动化安全测试集成到开发流程中并验证控制措施，而且无需任何 GraphQL 经验。

代码仓库：Black Hat GraphQL 代码仓库（https://github.com/dolevf/Black-Hat-GraphQL）包含代码示例、漏洞利用、查询以及对新资源覆盖。

在介绍核心概念后，你将搭建你的实验环境，探索 GraphQL 与 REST API 之间的差异，运行你的第一个查询，并学习如何创建自定义查询。

你还将学习如何：

- 使用数据收集和目标映射来了解目标
- 防御 API 免受拒绝服务攻击，并利用 GraphQL 服务器中的不安全配置来获取有关加固目标的信息
- 冒充用户并在远程服务器上执行管理员级操作
- 在服务器、数据库和客户端浏览器中发现基于注入的漏洞
- 利用跨站和服务器端请求伪造漏洞，以及跨站 WebSocket 劫持，迫使服务器代表你请求敏感信息
- 剖析漏洞披露报告并审查漏洞利用代码，以揭示漏洞如何影响大型公司

这份全面的资源提供了防御 GraphQL API 和构建安全应用程序所需的一切。把它想象成雷雨中的一把雨伞。

## 关于作者

NICK ALEKS 是 Wealthsimple 的安全高级总监、DEFCON Toronto 的联合创始人以及 ASEC.IO 的负责人。他黑过从网站到智能建筑的一切。

DOLEV FARHI 是 Palo Alto Networks 的杰出安全工程师，也是 DEFCON Toronto 的联合创始人。业余时间他研究物联网漏洞、参与 CTF 挑战，并为 Exploit-DB 做出贡献。

