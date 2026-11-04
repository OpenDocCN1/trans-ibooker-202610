## 索引

### 符号

__autoload() function
  缺点 150
  在 PHP 8 中使用 150-152
@ error control operator
  处理 106
@ operator
  关于 107-109
  用法 106, 107
__sleep() magic method
  关于 166, 167
  代码中断 167-169
__toString() magic method
  使用 162-164
__wakeup() magic method
  使用，以重新打开文件句柄 169-171

### A

abstract private method
  关于 354
  在 traits 中处理 354
abstract private method, in PHP 8
  参考链接 354
access control list (ACL) 78
action code 76
addGlob()
  选项 277
addPattern()
  选项 277, 278
addr()
  FFI 数据，使用 132-134
Advanced Vector Extensions
  (AVX) instructions 374
alignof()
  FFI 数据，使用 129-131
ambiguous string offset cast 104, 105
American Standard Code for Information Interchange (ASCII) 132, 258
Amp framework
  使用实现 PHP 异步 473
  URL 473
anonymous classes
  关于 357
  用法控制 357-359
anonymous functions
  与箭头函数对比 56
anonymous functions, generating from class methods
  更改 298-300
Apache JMeter tool
  URL 441
  使用 441, 442
application programming interface (API) 118, 247, 291, 455
Argon2i 232
Argon2id 232
arithmetic operations
  差异处理 214
  非标量数据类型处理 215-217
array
  处理，花括号使用
    更改 225-227
  在 PHP 8 中处理 223
  处理，负数组偏移 223-225
  与 SplFixedArray 对比 382, 383
array handling
array, unpacking 94-96
element 92
illegal offset types 96, 97
offsets, in non-array variables 93, 94
promoted warnings 91
speeding up 381, 382
techniques 67
ArrayObject
  与 SplFixedArray 对比 382, 383
array_slice() function
  使用 69, 70
array_splice() function
  使用 67, 68
arrow functions
  关于 56
  通用语法 56
  参考链接 61
  使用 56-60
  变量继承 57
  与匿名函数对比 56
assembly language 367
assert() function
  更改处理 233
assert() function, usage
  更改处理 234, 235
assertion 445
associative array 96
async coroutine support
  使用 453
attributes
  关于 10, 300
  语法 13-15
  查看，使用 Reflection
    扩展 15-17
  使用 10
Attributes class 10-13
attributes v2
  参考链接 17
authorization callback 76
authorizer
  用法示例 77-82
  用于保护 SQLite 数据库 75

### B

backtraces changes
  检查 316
backward-compatible breaks
  检测，使用数字字符串 210, 211
BC breaks
  审查 412
  在迁移前发现 411
BC BreakScan class
  扫描应用程序文件 428-430
  开发 417
  单个扫描方法，
    检查 420-424
  基础设施方法，
    定义 417-420
scanFromCallbacks(), examining 422
scanIsResource(), examining 421
scanMagicSignatures(), examining 423
scanRemovedFunctions(),
    examining 420, 421
BC break scan configuration file
    创建 412
    魔术方法签名违规，
        检测 414, 415
    strpos() 搜索配置，
        定义 413
BC breaks, with is_resource()
    检测 413
bcrypt 232
Beginners' All-purpose Symbolic
    Instruction Code (BASIC) 112
bitwise operations
    差异处理 214
    非标量数据类型处理 215-217
black-box tests 432
blocking operations 451
    执行，使用 PHP 8.1
        纤维 478, 479
BreakScan class calling program
    构建 424-428
bubble sort
    关于 137
    参考链接 137
built-in PHP web server
    用于测试 PHP 8.1 纤维 481
bytecode 366-368

### C

cast()
    FFI 数据，使用 134-136
CentOS Linux
    PHP 8 安装 437-439
changes, to mb_ereg*() functions
    检查 260
changes, to mb_ereg_replace()
    检查 261, 262
channels
    实现 456
C language
    与 PHP 的关系 113
class constant
    常规类常量，
        使用 336, 337
    特殊类常量，使用 337, 338
    使用 335
class constructor
    关于 156
    不一致，解决 158-162
client Uniform Resource Locator
    (URL) (cURL) 239
code
    改进，使用类型化属性 33
code reduction
    使用构造函数属性
        提升 8, 9
COM extension changes
    审查 282
comment handling
    差异 300, 301
commit hooks 410
communicating sequential
    processes (CSP) 456
complex BC break detection
  处理 415, 416
complex match expressions
  示例 21-23
Component Object Model (COM) 282
Composer
  问题，使用处理 443-445
Computer Science 101 (CS101) 137
concatenation operations
  差异处理 214
concatenation operator 51
constants
  在 PHP 8 中使用 206, 207
constructor changes
  处理，在同名方法和类中 156-158
  不一致，在类构造函数中解决 158-162
  使用 156
constructor property promotion
  关于 33
  参考链接 10
  语法 7, 8
  使用 7
  用于代码减少，使用 8, 9
container class
  实现，使用 SplObjectStorage 类 398-402
contravariant parameters
  使用 180-182
conventional class constant
  使用 336, 337
core deprecations
  检查 301
  在参数顺序中 302, 303
core functionality, removed in PHP 8
  检查 287
core functions
  调用，使用命名参数 24
coroutine support 458
counter-intuitive results
  处理 52
covariant returns 177-180
createFromInterface() method
  关于 349
  检查 349, 350
create_function()
  使用 291-295
crypt() function
  更改处理 230, 231
cURL extension changes 282
curly brace usage changes
  处理 225-227

### D

data
  比较，使用外部函数接口 (FFI) 124-127
database extension changes
  操作系统库要求 274
data type checking
  mb_str*() 函数 258
data types
  探索 27
  混合类型 30
  联合类型 27
DateTimeImmutable
  用例 347-349
DateTime methods
  createFromInterface() 方法，检查 349, 350



## D
DateTimeImmutable，用例 347-349
使用 347
Debian/Ubuntu Linux
PHP 8 安装 435, 436
PHP 版本，切换 436, 437
Defense Encryption Standard (DES) 230
依赖注入 (DI) 398
已弃用功能
在 PHP 8 扩展中 310-312
已弃用的 PHP 8 扩展功能
发现 307
DOM 扩展更改 308
PostgreSQL 扩展更改 309
XML-RPC 扩展更改 307
已弃用的与安全相关的
功能，在 PHP 8 中
backtraces 更改 316
自定义错误处理更改 314, 315
检查 312
PDO 错误处理更改 316, 317
PHP 8 stream-filter 更改 312-314
track_errors php.ini 设置 317, 318
解引用 62
开发环境
检查 409, 410
disable_functions
更改，处理 228
disable_functions，处理差异
检查 228-230
docblocks 267
Doctrine ORM
参考链接 17
Document Object Model (DOM) 308
DOMChildNode 接口
关于 343
检查 343, 344
DOM 扩展更改 308
DOM 扩展接口
发现 341
DOMChildNode 接口，
检查 343, 344
DOMParentNode 接口，
检查 341-343
DOM 用法，示例 344-347
DOM Living Standard
关于 308, 341
参考链接 308
DOMParentNode 接口
关于 341
检查 341-343
DOM 用法
示例 344-347
DynASM
关于 371
参考链接 372

## E
each() 函数
使用 288-291
empty() 函数，文档
参考链接 256
empty needle-argument 处理
mb_str*() 函数 256-258
empty needle 参数
处理 197-199
工程记数法 208
环境
关于 408
组件 409
定义 409
error-level 默认值 89
error_reporting() 函数 108, 109
错误
  处理 89
事件循环
  实现 454
Expat
  参考链接 253
Expat C 库 253
昂贵对象 394
Extended Markup Language (XML) 46
Extensible Markup Language (XML) 12
扩展更改
  检查 283
扩展注意事项
  使用 274
外部 C 库
  集成到 PHP 脚本中 137-139

## F
Fedora Linux
  PHP 8 安装 437-439
fetch 模式 328
FFI 类
  比较方法 124-127
  创建方法 119
  检查 118
  基础设施方法 131
  方法 118
  参考链接 118
FFI 创建方法
  使用 119
FFI\CType 实例
  创建 119-121
  使用 120, 121
FFI 数据
  使用，addr() 132-134
  使用，alignof() 129-131
  使用，cast() 134-136
使用，free() 132-134
使用，memcpy() 132-134
使用，memset() 132-134
使用，sizeof() 129-131
使用，typeof() 128
FFI 扩展
  需要 117, 118
FFI 扩展数据
  信息，提取自 127
FFI 基础设施方法
  使用 131
FFI 实例
  创建 122, 123
  使用 122, 123
Fiber 类
  发现 476
  方法 477
文件描述符 (FD) 454
First-In, First-Out (FIFO) 188
流畅接口 333
Foreign Function Interface (FFI)
  关于 112
  添加到 PHP 中 116
  数据，比较 124-127
  使用，避免为速度 117
  使用场景 115, 116
  在应用程序中使用 137
free()
  FFI 数据，使用 132-134
函数
  受影响，由 locale
    独立性 220-223
在 PHP 8 中移除的函数
  create_function()，使用 291-295
  each() 函数，使用 288-291
  检查 287, 288
  money_format() 函数，
    使用 295-297

## G
GD 扩展更改
  关于 265, 266
  编译标志更改 264, 265
  处理 262
  资源到对象迁移 263, 264
GD 库
  参考链接 263
通用语法 56
GeoNames
  参考链接 74, 288
getters 35
Git Hooks
  参考链接 410
Golang 458
Go 语言 456, 474
Graphics Interchange Format (GIF) 262
不一致的字符串到数字比较结果
  处理 212, 213
索引 96
InfiniteIterator
  参考链接 61
无限循环 471
初始化向量 (IV) 230
Integrated Development Environment (IDE) 410
接口
  使用 341
Intermediate Representation (IR) 372
International Organization for Standardization (ISO) 20, 295
解释器 368
进程间通信 (IPC) 458
Intl 扩展更改
  使用 281, 282
控制反转容器 398
I/O 密集型应用程序
  检查 463-467
is_resource() 函数
  代码中断，涉及 240-242
IteratorAggregate 迁移
  实现，而不是 Traversable 245, 246

## H
haystack 参数
  与 needle 参数对比 195
热函数 374
HyperText Markup Language (HTML) 59, 246, 297
HyperText Transfer Protocol (HTTP) 242, 463

## I
非法偏移 96
非法排序函数
  处理 393, 394
imagesetinterpolation() 函数
  参考链接 266
implode()
  更改，检查 204, 205

## J
JavaScript Object Notation (JSON) 27, 64, 164, 459
Java Virtual Machine (JVM) 367, 442
JIT 编译器
  调试 378-380
  启用 372, 373
  PHP 程序执行，使用发现 371, 372
tracing 模式，配置 373, 374
使用 375-378
使用 366, 367
JIT 编译器设置
发现 381
JIT tracer 操作
配置 373, 374
CPU opt 标志 374
JIT 触发器 374
优化级别 374
寄存器分配 374
Just-in-Time (JIT) 编译器 7, 451

## K
键
稳定排序效果，检查 391-393

## L
Last-In, First-Out (LIFO) 188
前导数字字符串 208
libxml 扩展
更改 253
Liskov 替换原则 177
locale 独立性
关于 219
函数，受影响 220-223
操作，受影响 220-223
副作用 220
Lua 编程语言 371

## M
机器代码 366-368
机器语言 367
魔术方法
关于 324
参考链接 164
使用，控制 PHP
序列化 174-176
魔术方法签名违规
检测 414, 415
Mandelbrot 375
master 分支 432
match 表达式
示例 19, 20
通用语法 18, 19
整合到程序代码中 18
参考链接 23
match 函数 291
mb_str*() 函数
数据类型检查 258
empty needle-argument
处理 256-258
needle-argument 差异，
发现 256
mbstring 扩展更改
发现 304
mbstring 扩展函数重载
参考链接 305
使用 305-307
mbstring 扩展函数
参考链接 257
mbstring 扩展移除的别名
处理 304
mb_strrpos()
差异 259, 260
memcpy()
FFI 数据，使用 132-134
内存泄漏 140
memset()
FFI 数据，使用 132-134

## N
归并排序 137
方法和类，名称相同
  更改，处理 156-158
方法冲突
  在 traits 之间解决 351-353
方法签名 182
方法签名更改
  关于 324
  类常量，使用 335
  发现 324
  学习 340
  魔术方法签名，
    管理 324-328
PDO 扩展签名
  更改，处理 328-330
Reflection 方法签名
  更改，检查 328
静态方法，处理 330-333
静态返回类型，使用 333-335
尾随逗号，使用 339
Mezzio 框架
  URL 473
  使用，与 Swoole 473, 474
微框架 473
迁移
  执行 430-439
  测试 440
  测试和故障排除工具 440
  故障排除 440
混合类型
  关于 30
  影响，继承 31, 32
  需要 30
  参考链接 32
Model-View-Controller (MVC) 473
修改时间 277
money_format() 函数
  使用 295-297
Multi-Processing Modules (MPMs) 452
MySQL Improved (MySQLi) 274
命名参数
  关于 24, 67
  通用语法 24
  参考链接 27
  用于调用核心函数 24
命名参数，使用
  文档 25, 26
  顺序独立性 25, 26
命名空间
  关于 360
  命名实践，暴露 362, 363
  保留关键字，使用 361
  标记化过程，
    发现 360, 361
needle 参数
  更改，处理 195
  empty needle 参数 197-199
  非字符串 needle 参数 195-197
  与 haystack 参数对比 195
needle-argument 差异
  发现，在 mb_str*() 函数中 256
needle-argument 处理 256
负数组偏移
  处理 223-225
嵌套序列化调用 173
嵌套三元运算符
  使用 53-55
Node.js 框架
  URL 470
非标量数据类型
  处理，在算术
    操作 215-217
  处理，在按位操作 215-217



## O

## P
非稳定排序
  与稳定排序对比 387-390
非严格比较 208
非字符串针参数
  处理 195-197
通知到警告提升
  关于 100
  模糊字符串偏移转换 104, 105
  不存在的对象属性，
    访问处理 101, 102
  不存在的偏移处理 102, 103
  不存在的字符串偏移 105
  资源 ID，误用作
    数组偏移 103, 104
  未初始化的字符串偏移 105
空长度参数
  在 PHP 8 中使用 203, 204
空安全运算符
  关于 46
  使用 46-48
  使用以短路链 48-51
数字数组 96
数字字符串
  检测，向后兼容的
    破坏 210, 211
  检查 208-210
  参考链接 210
对象错误处理
  提升的警告 90, 91
面向对象编程
  (OOP) 44, 113, 247, 290, 451
对象属性
  处理 148, 149
对象关系映射器 (ORM) 11
偏移量 96

OOP 编码差异
  处理对象属性 148, 149
  发现 147
  静态调用，在 PHP 8 中处理 147, 148
  在 PHP 8 中工作
    自动加载 149, 150
操作码 368
开放 Web 应用安全项目 (OWASP)
  参考链接 254
操作
  受区域设置影响
    独立性 220-223
运算符优先级
  更改，检查 217-219
parallel\Channel 类
  parallel\Events 类 475
  使用 475
parallel 扩展
  安装 476
  parallel\Channel 类 475
  parallel\Events 类 475
  parallel\Future 类 475
  parallel\Runtime 类 474
  URL 474
  使用 474
parallel\Future 类
  使用 475
并行编程 453
parallel\Runtime 类
  使用 474
参数顺序
  已弃用的用法 302, 303
解析器标记
  参考链接 360

解析 369
password_hash() 函数
  更改，处理 232, 233
PCRE 扩展更改
  检查 279, 280
PDO 错误处理更改
  检查 316, 317
PECL 网站
  URL 116
个人软件包存档 (PPA) 435
PHP
  字节码 367, 368
  外部函数接口
    (FFI)，添加至 116
  机器码 367, 368
  与 C 语言的关系 113
  在没有 JIT 的情况下工作 367
PHP 8
  数组，处理 223
  比较更改 213
  常量，使用 206, 207
  在 CentOS Linux 上安装 437-439
  在 Debian/Ubuntu 上安装
    Linux 435, 436
  在 Fedora Linux 上安装 437-439
  在 Red Hat Linux 上安装 437-439
  空长度参数，
    使用 204
  Oniguruma 库 260
  移除的核心功能，
    检查 287
  移除的函数，检查 287, 288
  资源到对象迁移 239, 240
  SplFixedArray 更改，
    使用 384, 385
  SplFixedArray，使用 382
  静态调用，处理 147, 148

统一变量语法，对其影响 62
升级问题，避免 213, 214
与 PHP 7 对比 344
PHP 8.1 纤程
  关于 476
  阻塞操作，执行
    使用 478, 479
  在 ReactPHP 上检查 481
  在 Swoole 上检查 481
  示例 480, 481
  测试，使用内置 PHP
    Web 服务器 481
  使用 ReactPHP 测试 482
  使用 478
PHP 8 自动加载
  __autoload() 函数 150-152
  spl_autoload_register()
    函数，使用 153, 154
  spl_autoload_register() 函数，
    运行 154, 155
  使用 149, 150
PHP 8 错误处理
  关于 86
  错误级别默认值 89
  未定义变量，处理 87
PHP 8 扩展方差支持
  关于 177
  使用逆变参数 180-182
  使用协变返回 177-180
PHP 8 扩展
  已弃用功能 310-312
  移除功能，
    使用 303, 304
PHP 8 运算符
  使用 40
PHP 8 流过滤器更改
  检查 312-314

PHP 8 字符串到数字比较
  关于 207
  数字字符串，检查 208-210
PHP 8 trait 处理改进
  关于 350
  抽象私有方法，
    在 traits 中处理 354
  方法冲突，解决
    在 traits 之间 351-353
  trait 抽象签名检查，
    使用 353, 354
PHP 8，使用更改
  匿名函数，生成
    从类方法 298-300
  注释处理，差异 300, 301
  发现 297
  移除的类型转换 297
PHP 异步应用程序
  通道，实现 456
  创建 454
  事件循环，实现 454
  使用 Amp 实现
    框架 473
  promise，实现 455
  流，实现 455
  定时器，实现 456
PHP 异步编程模型
  关于 451, 452
  工作 453
PHP 异步解决方案
  Amp 框架 473
  Mezzio 框架 473, 474
  ReactPHP 470-472
  使用 470
PHP 回调
  实现 140-142
  用法问题 140
  使用 139

PHP 注释 10, 11
PHP 核心扩展
  访问 114
PHP DocBlocks
  考虑 11, 12
  误用，缺点 12
  URL 17
PHP 扩展社区库 (PECL)
  关于 115
  参考链接 307
PHP 扩展原型制作 117
PHP 扩展 113
php.ini 文档
  参考链接 17
PHP 魔术方法
  更改，导航 156
PHP 魔术方法，更改
  关于 156
  处理构造函数更改 156
  使用 __toString() 162, 164
PHP 非核心扩展
  检查 114, 115
  查找 115
PHP OPcache
  操作 369-371
PHP 程序执行
  关于 369
  使用 JIT 编译器发现 371, 372
PHP Reflection
  参考链接 17
PHP 脚本
  外部 C 库，集成
    至 137-139
PHP 序列化
  __sleep() 魔术方法 166, 167
  __wakeup() 魔术方法 169-171

## R

## Q
关于 164
代码中断，在 __sleep()
    方法 167-169
控制 164
控制，使用魔术
    方法 174-176
示例 165, 166
接口 171-173
接口问题，检查 173
PHP 标准建议
    15 (PSR-15) 464
PHP 同步编程代码
    开发 451, 452
PHPUnit
    关于 445
    URL 445
PHP 版本
    切换，在 Debian/之间
        Ubuntu Linux 436, 437
便携式网络图形 (PNG) 23
PostgreSQL 扩展更改 309
preg_last_error()
    参考链接 279
私有方法
    处理 355-357
生产环境
    检查 411
promise
    实现 455
PyPy VM 372
快速排序 137

ReactPHP
    PHP 8.1 纤程，检查 481
    URL 454, 470
    用于测试 PHP 8.1 纤程 482
ReactPHP 框架
    使用 470-472
Red Hat Linux
    PHP 8 安装 437-439
Red Hat 软件包管理器 (RPM) 437
Reflection*::export()
    重写代码，使用 307
Reflection 扩展
    用法 267-270
    用于查看属性 15-17
Reflection 扩展更改
    关于 273, 274
    发现 266
Reflection 扩展，类和方法
    参考链接 266
Reflection 扩展，改进
    关于 270
ReflectionParameter::*DefaultValue*
    方法 271-273
ReflectionType 修改 270
寄存器 374
正则表达式 (regex) 260, 279
Remi 的 RPM 仓库
    URL 437
移除的功能，在
    PHP 8 扩展
mbstring 扩展更改，
    发现 304
    使用 303, 304
移除的类型转换 297
表征状态转移
    (REST) 463

## S
请求评论
    (RFCs) 302, 366, 476
解析器 455
资源到对象迁移
    关于 239
    优点 242-244
    代码中断，涉及 is_
        resource() 240-242
GD 扩展 263, 264
PHP 8 扩展 239
Traversable，到 IteratorAggregate
    迁移 245, 246
RESTful API 开发 473
重写代码
    使用 Reflection*::export() 307
scanFromCallbacks()
    检查 422
scanIsResource()
    检查 421
scanMagicSignatures()
    检查 423
scanRemovedFunctions()
    检查 420, 421
科学计数法 208
作用域解析运算符 335
安全哈希算法 (SHA-1) 230
安全函数
    assert() 函数 233
    更改，处理 227
    crypt() 函数 230, 231
    password_hash() 函数 232, 233
服务定位器容器 398
设置器 35
希尔排序 137

SimpleXML 扩展
    更改，使用 248-252
sizeof()
    FFI 数据，使用 129-131
空间船运算符 124
特殊类常量
    使用 337, 338
spl_autoload_register() 函数
    使用，在 PHP 8 中 153, 154
spl_autoload_register() 函数
    运行，在 PHP 8 中 154, 155
SplDoublyLinkedList 类
    标准 PHP 库 (SPL)
        更改，在其中处理 188-191
SplFileObject 类
    标准 PHP 库 (SPL) 更改，
        检查 183-185
SplFixedArray
    与数组对比 382, 383
    与 ArrayObject 对比 382, 383
    在 PHP 8 中使用 382
SplFixedArray 更改
    在 PHP 8 中使用 384, 385
SplHeap 类
    标准 PHP 库 (SPL) 更改，
        检查 185-188
SplObjectStorage 类
    容器类，实现
        使用 398-402
SQLite
    关于 75
    参考链接 75
SQLite 数据库
    授权回调 76
    回调函数 76, 77
    无安全 75, 76
    使用授权器保护 75



稳定排序
- 关于 386, 387
- 实现 385
- 参考链接 386
- 与非稳定排序对比 387-390
稳定排序效果
- 检查，针对键 391-393
暂存环境
- 检查 410, 411
标准 PHP 库 (SPL) 59, 249
标准 PHP 库 (SPL) 更改
- 处理 183
静态调用
- 处理，在 PHP 8 中 147, 148
static 关键字 333
策略软件设计模式 242
str_contains() 函数
- 关于 72-74
- 使用 199, 200
流
- 关于 312
- 实现 455
str_ends_with() 函数 72
严格比较 208
字符串函数
- 检测 71
- str_contains() 函数 72-74
- str_ends_with() 函数 72
- str_starts_with() 函数 71
字符串处理
- 空字符串偏移 99
- 偏移 98, 99
- 提升的警告 98
字符串处理差异
- 常量，在 PHP 8 中使用 206, 207
- 处理，v*printf() 更改 200-203
- 检查，更改
  - implode() 204, 205
  - 处理，needle 参数的更改 195
  - 学习 194
  - 处理，在 PHP 8 中使用空长度参数 203, 204
字符串处理技术 67
strpos() 搜索配置
- 定义 413
str_starts_with() 函数 71
结构化查询语言 (SQL) 42, 311
超全局变量 208
switch 语句
- 示例 19, 20
Swoole
- 使用 Mezzio 框架 473, 474
- 检查 PHP 8.1 fibers 481
Swoole 扩展
- 检查 457, 458
- 安装 458-461
- URL 454
- 使用 457
- 使用，以提高应用程序性能 467-470
Swoole 扩展，安装
- 测试 461-463

## T
TensorFlow
- 关于 115
- 参考链接 116
三元运算符 53
测试和故障排除
- 工具，迁移
  - 关于 440
  - 使用 Apache JMeter 441, 442
  - 使用 Xdebug 440, 441
线程 453
定时器
- 实现 456
标记化过程 360
标记 368
track_errors php.ini 设置
- 检查 317, 318
尾随逗号
- 使用 339
trait 抽象签名检查
- 使用 353, 354
traits
- 使用 341
传输控制协议 (TCP) 458
Traversable，到 IteratorAggregate
- 迁移 245, 246
类型转换 134
类型强制转换 28
类型化属性
- 关于 33
- 缺点 35-38
- 需要 34, 35
- 用于改进代码 33
类型 juggling
- 关于 207
- 参考链接 210
typeof()
- 使用 FFI 数据 128

## U
未定义常量
- 关于 88
- 处理 88
未定义变量
- 关于 87
- 处理 87
统一资源定位符 (URL) 80, 93, 115, 480
统一变量语法
- 关于 61
- 定义 61
- 参考链接 61
统一变量语法，对 PHP 8 的影响
- 关于 62
- 类常量解引用 64, 65
- 常量，不一致的解引用 63, 64
- 解引用，定义 62
- 增强表达式 66
- 插值字符串，解引用 63
联合类型
- 关于 15, 27
- 示例 29, 30
- 泛型语法 28
联合类型文档
- 参考链接 32
单元测试
- 使用 445, 446
更新的 mbstring 扩展
- 问题，避免 255, 256
用户数据报协议 (UDP) 458
UTF-8 260

## V
可变参数运算符
- 关于 41
- 参数，收集 42-44
- 未知数量的参数 41
- 使用，作为替代 44-46
方差 177
虚拟机 (VM) 367
v*printf() 更改
- 处理 200-203

## W
WeakMap 类
- 优点，相对于 SplObjectStorage 类 402-405
- 参考链接 402
- 使用 397, 398
WeakReference 类定义
- 审查 395
弱引用
- 优点 395
- 使用 396, 397
- 使用，以提高效率 394, 395
Web 超文本应用技术工作组 (WHATWG) 341
扩展 31
Windows 服务 for Linux (WSL) 457
工作进程 452
万维网联盟 (W3C) 246
XML 外部实体 (XXE) 253
XMLParser 扩展
- 更改 254
XMLReader 扩展
- 更改 254
XML-RPC 扩展
- 更改 307
XMLWriter 扩展差异
- 检查 247, 248

## X
XDebug
- 关于 378
- URL 440
Xdebug 工具
- 使用 440, 441
XML 扩展，更改
- 关于 246, 253
- 对 SimpleXML 扩展的更改，使用 252
- libxml 扩展 253
SimpleXML 扩展更改，使用 248-252
XMLParser 扩展 254
XMLReader 扩展 254
XMLWriter 扩展差异，检查 247, 248

## Y
YAML 不是标记语言 (YAML) 27

## Z
Zend Engine 367
Zend Expressive 473
Zend Framework 473
Zend 线程安全 (ZTS) 476
Zip 扩展更改
- addGlob()，选项 277, 278
- addPattern()，选项 277, 278
- OOP 迁移，处理 275-277
- 审查 275
ZipArchive 类方法 277
ZipArchive 方法更改 278
Zip 扩展 OOP 迁移
- 处理 275-277
