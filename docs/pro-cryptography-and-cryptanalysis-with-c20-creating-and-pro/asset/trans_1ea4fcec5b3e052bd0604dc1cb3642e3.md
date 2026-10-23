

```
if ((special_symbols_count == 1)
    && (lowerCase_Alphabet_Count == 0
    || lowerCase_Alphabet_Count == 1
    || upperCase_Alphabet_Count == 0
    || upperCase_Alphabet_Count == 1
    || digits_And_numbers_count == 0))
    break;

key_seed = generateRandomlyInteger();
key_seed = key_seed % 6;
crypto_key = crypto_key +
    special_symbols[key_seed];
special_symbols_count++;
countingLengthCryptoKey++;
break;
}
}
cout << "\n---------------------------\n";
cout << "      Cryptography Key      \n";
cout << "---------------------------\n\n";
cout << " " << crypto_key;
cout << "\n\nPress any key to continue... \n";
getchar();
}

int main() {
    int option;
    int desired_length;

    //** designing the menu
do {
    cout << "\n-------------------------------\n";
    cout << "  Random Cryptography Key Generator   \n";
    cout << "-------------------------------\n\n";
    cout << "  1 --> Generate a Cryptography Key"
    << "\n";
```

```
printf("Enter ( 1 ) to generate cryptography\nkey and ( 2 ) to quit the program.\n");
cout << "Enter a key and try again \n";
getchar();}
break; }
} while (option != 2);
return 0;
}
```

![](img/c3d9424f64735276d661e7aba725b2a2_460_0.png)

*图 23-5.* 生成的密钥和可能的密码。我们选择三个字符以缩短处理时间

## 选择密文攻击

在选择密文攻击（CCA）中，密码分析者可以对信息执行加密和解密操作。在此攻击中（参见图 23-6），密码分析者可以选择明文、对其进行加密、观察密文的生成方式，并逆转整个过程。

## 参考文献

+   [1] 阿布·优素福·雅各布·伊本·伊沙克·萨巴赫·金迪。在线获取：www.trincoll.edu/depts/phil/philo/phils/muslim/kindi.html。
+   [2] 哲学家：雅各布·伊本·伊沙克·金迪 Kennedy-Day, K. al-Kindi, Abu Yusuf Ya‘qub ibn Ishaq (卒于约 866–73 年)。在线获取：www.muslimphilosophy.com/ip/kin.html。
+   [3] 艾哈迈德·福阿德·埃尔-赫瓦尼，“金迪”，载于 *《穆斯林哲学史》第 1 卷*（第 421-434 页）。新德里：低价出版社。1961 年。
+   [4] 伊斯梅尔·R·法鲁基和洛伊丝·拉姆娅·法鲁基，*《伊斯兰文化地图集》*（第 305-306 页）。纽约：麦克米伦出版公司。1986 年。
+   [5] *《不列颠百科全书》*（第 352 页）。不列颠百科全书股份有限公司。芝加哥：威廉·本顿。1969 年。
+   [6] J.J. 奥康纳和 E.F. 罗伯逊，*阿布·优素福·雅各布·伊本·伊沙克·萨巴赫·金迪*。1999 年。

## 索引

+   **A**
+   
+   临时技术，21  
+   美国国家标准学会（ANSI），377  
+   基于属性的加密（AE），341，354  
+   身份验证，18，19，22  
+   身份验证协议，258，350，381，384

+   **B**
+   
+   大数据加密  
+   &nbsp;&nbsp;&nbsp;&nbsp;CIA 三元组，339  
+   &nbsp;&nbsp;&nbsp;&nbsp;云架构，338  
+   &nbsp;&nbsp;&nbsp;&nbsp;云，339，340  
+   &nbsp;&nbsp;&nbsp;&nbsp;加密技术，340  
+   &nbsp;&nbsp;&nbsp;&nbsp;节点，337，338  
+   &nbsp;&nbsp;&nbsp;&nbsp;VC（参见可验证计算（VC））  
+   大整数库  
+   &nbsp;&nbsp;&nbsp;&nbsp;Bignum C++ 库，113  
+   &nbsp;&nbsp;&nbsp;&nbsp;Boost 库，113  
+   &nbsp;&nbsp;&nbsp;&nbsp;GMP 库，113  
+   &nbsp;&nbsp;&nbsp;&nbsp;L3HARRIS 地理空间解决方案，113  
+   &nbsp;&nbsp;&nbsp;&nbsp;LibBF 库，113  
+   &nbsp;&nbsp;&nbsp;&nbsp;Matt McCutchen，112  
+   &nbsp;&nbsp;&nbsp;&nbsp;ttmath:UInt<>，114  
+   大整数  
+   &nbsp;&nbsp;&nbsp;&nbsp;加 1，107，108  
+   &nbsp;&nbsp;&nbsp;&nbsp;加法算法，108，109  
+   &nbsp;&nbsp;&nbsp;&nbsp;乘法，109–111  
+   &nbsp;&nbsp;&nbsp;&nbsp;multiplyingOneDigit 函数，109

+   左移，112  
+   标准整数，106  
+   转换标准整数，106，107  
+   Bignum C++ 库，113  
+   二元组计数，440–443  
+   生日问题，69  
+   Boost 库，113  
+   可自举加密方案，262  
+   玻色子散射，261  
+   Botan，177  
+   暴力攻击，424  
+   &nbsp;&nbsp;&nbsp;&nbsp;示例，424  
+   &nbsp;&nbsp;&nbsp;&nbsp;生成字符串，428，430，432  
+   &nbsp;&nbsp;&nbsp;&nbsp;运行，425  
+   &nbsp;&nbsp;&nbsp;&nbsp;类型，424  
+   缓冲区溢出攻击，执行，433，434

+   **C**
+   
+   C++20  
+   &nbsp;&nbsp;&nbsp;&nbsp;可调用概念，130，131  
+   &nbsp;&nbsp;&nbsp;&nbsp;比较概念，130  
+   &nbsp;&nbsp;&nbsp;&nbsp;核心语言概念，128，129  
+   &nbsp;&nbsp;&nbsp;&nbsp;特性，125  
+   &nbsp;&nbsp;&nbsp;&nbsp;特性测试  
+   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;carries_dependency，125–127  
+   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;no_unique_address，127，128  
+   &nbsp;&nbsp;&nbsp;&nbsp;<compare> 头文件，131  
+   &nbsp;&nbsp;&nbsp;&nbsp;<concepts> 头文件，128  
+   &nbsp;&nbsp;&nbsp;&nbsp;<format> 头文件，132  
+   &nbsp;&nbsp;&nbsp;&nbsp;对象概念，130

## 索引

+   凯撒密码，180，182
+   C/C++ 库，151
+   CERT 编码标准
+     - 自动化检测过程，143
+     - 合规解决方案，141
+     - 异常，141
+     - 标识符，141
+     - 不合规代码，141
+     - 相关指南，143
+     - 风险评估，142，143
+     - 规则，144
+   基于混沌的加密，117
+     - 混沌映射，307，308
+     - 混沌系统 *对比* 加密算法，303
+     - 复数，309，310
+     - 解密过程，304，305
+     - 离散时间系统，304
+     - 加密过程，304，305
+     - 图像加密/解密，304，306
+     - 李雅普诺夫指数（LE），304
+     - 周期性的，304
+     - 实际实现（*参见* 实际实现）
+     - Rössler 吸引子，308，309
+     - 安全性分析，306，307
+   字符与字符串（STR），146，147
+   中国剩余定理（CRT），74
+   卡方统计量，7，435
+     - 加密文本，437，438
+     - 输出，436，437
+   选择密文攻击（CCA），459，460
+   选择明文攻击（CPA），451–454，456–459
+   密码算法，381
+   仅密文攻击（COA），450
+   最近向量问题（CVP），226

## E

## D

## F

+   - 通信过程，31，32
+   - 定义，3，18，22
+   - 数字签名，16
+   - 定义域，22
+   - 定义域/值域，29，30
+   - 电子通信，3
+   - 元素，23
+   - 加密/解密，30，31
+   - 交换密钥，15
+   - 功能图，24
+   - 难度假设，15
+   - 像，22
+   - 机制，6
+   - 模型/服务，5
+   - 网络安全，10
+   - 公钥加密方案，15
+   - RSA，15
+   - 安全通信过程，4
+   - 标准，11–13
+   - 系统，7
+   - 维吉尼亚密码实现，55
+     - 代数描述，55
+     - 源代码，56，57
+   - 密码算法，105，131，138，388
+   - 下一代加密（CNG），6
+   - CrypTool（CT），177，179，180
+   
+   - 数据完整性，5，17，19，51
+   - 声明与初始化（DCL），144，145
+   - 差分分析，307
+   - 差分密码分析，387，388
+   - 数字签名
+     - 签名过程，32，33
+     - 验证过程，33
+   
+   - 椭圆曲线加密（ECC），152，155，189
+     - 优势，189
+     - 图形内容，190–192
+     - 群定律，194
+     - 实际实现，195
+       - 布尔变量，197
+       - FFE 引擎，198，201，203，205，207，210，218，222
+       - 主程序，197
+       - 部件，195
+     - Weierstrass 方程，192，194
+   - 椭圆曲线 Diffie-Hellman（ECDH），6，155
+     - 创建密钥，156
+     - EC 参数，157
+   - 椭圆曲线数字签名算法（ECDSA），244
+   - 加密/解密，357，359
+   - 熵，70，71
+   - 欧几里得算法，91
+   - 表达式（EXP），145
+   
+   - 联邦信息处理标准（FIPS），11
+   - 有限域
+     - 基本概念，78
+     - 多项式/欧几里得算法，79
+   - FIPS 140-2/140-3，378
+   - 浮点运算
+     - 计算，117
+     - 数据类型，117
+     - 显示，118

