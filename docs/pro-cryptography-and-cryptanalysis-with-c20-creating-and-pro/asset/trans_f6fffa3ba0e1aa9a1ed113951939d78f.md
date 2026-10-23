

## 第21章

## 暴力破解与缓冲区溢出攻击

本章介绍两种重要攻击，*缓冲区溢出*攻击和*暴力破解*攻击，它们经常被用于针对 C++ 应用程序和程序。

一类特殊的攻击者会使用应用程序/程序或设备来发起暴力破解或缓冲区溢出攻击。它们的方法会评估确认表单的单词的不同组合。在某些情况下，攻击者会尝试通过扫描会话 ID 等过程来破坏 Web 应用程序。例如。攻击者的目标包括窃取数据、用恶意软件破坏目标机器，并索取特定金额的钱。一些攻击者出于个人选择而以物理方式执行暴力破解攻击。今天，大多数暴力破解和缓冲区溢出攻击都是由机器人执行的。

为了保护组织和企业免受此类攻击，请考虑以下建议：

- 不要使用可以在网上找到的数据或信息。
- 使用尽可能多的字符。
- 使用字母、数字和特殊字符的组合（例如符号）。
- 不要使用常见模式（例如 qwerty）。
- 为每个用户账户使用不同的密码。
- 定期更改密码（例如每两个月）。
- 使用并生成长且强密码。如果您没有想法，可以使用密码生成器（例如 KeyPass 的密钥生成）。
- 实现多因素认证 [1]。
- 如果可能，使用生物识别 [2]。

## 暴力破解攻击

暴力破解攻击代表一种复杂的攻击，其中攻击者会提交大量密码或密码短语，目标是猜出正确的密码。攻击者会逐一检查每个密码或密码短语，直到找到正确的密码。此外，攻击者可能猜测密钥。密钥通常由密码通过密钥派生函数创建。此过程称为穷举密钥搜索。

暴力破解攻击有许多类型：

- **基于彩虹表的攻击**：彩虹表是预定义和预计算的表。目标是逆转密码哈希容量的过程。
- **基于逆向暴力破解攻击的攻击**：攻击基于使用通用密码或特定密码集针对多个用户名。
- **凭据攻击**：攻击使用密码-用户名集针对各种网站。
- **混合暴力破解攻击**：攻击用于弄清楚使用何种密码变体才能成功。之后，它会继续进行处理不同变体的一般检查过程。

我们将通过以下示例说明这种攻击，以展示攻击如何在现实生活情况（算法）中被使用和部署。提供的示例有

- **对凯撒密码的暴力破解攻击**：该示例（见图 21-1 和清单 21-1）基于凯撒密码。选择该示例作为展示暴力破解攻击的第一个案例是由于其简单性。
- **暴力破解攻击的字符串生成**：该示例（见图 21-2 和清单 21-2）展示了如何进行基本字符串生成，目标是创建可用于暴力破解攻击的复杂列表和字典。

## 第21章　暴力破解与缓冲区溢出攻击

![](img/c3d9424f64735276d661e7aba725b2a2_426_0.png)

**图 21-1。** *运行暴力破解攻击*

### 清单 21-1. 对凯撒密码的暴力破解攻击

```
```cpp
#include<iostream>
using namespace std;

// the function will be used to encrypt the plaintext
// string msg - the message
// int keytValue - the key
string encrypt(string msg,int keyValue)
{
        // variable used to hold the cipher value of the plaintext
    string cipher="";
```
```

```
```cpp
for(int i=0;i<encMessage.length();i++)
{
    // check if the character is upper case
    if(isupper(encMessage[i]))
    {
        if((encMessage[i] - keyTry - 65)<0)
            plaintext += 91 + (encMessage[i] - keyTry - 65);
        else
            plaintext += (encMessage[i] - keyTry - 65)%26 + 65;
    }
    // check if the character is lower case
    else if(islower(encMessage[i]))
    {
        if((encMessage[i] - keyTry - 97) < 0)
            plaintext += 123 + (encMessage[i] - keyTry - 97);
        else
            plaintext += (encMessage[i] - keyTry - 97)%26 + 97;
    }
    else
        plaintext += encMessage[i];
}
cout << "BRUTE-FORCE ATTACK (DECRYPTION) - The clear text for key -> " << keyTry << " :- " << plaintext << endl;
}
}

int main()
{
    int encKey;
    string cleartext;
    cout << "ENCRYPTION - Enter the text for encryption -> ";
    getline(cin,cleartext);

    cout << "Enter the key for encryption the text -> ";
    cin >> encKey;
```
```

```
string encryptedMessage = encrypt(cleartext,encKey);
cout << "ENCRYPTED MESSAGE - The encrypted message is -> "
<< encryptedMessage << endl << endl;

//** brute force attack
decrypt(encryptedMessage);
}
```

![](img/c3d9424f64735276d661e7aba725b2a2_429_0.png)

图 21-2. 暴力破解攻击的基本字符串生成（生成字符串的不同状态）

### 清单 21-2. 基本字符串生成源代码

```
#include <string.h>
#include <stdio.h>
#include <stdlib.h>

// We are using a linked list data structure.
// The reasong is to avoid some of the restrictions
//      based on the generation of the string length.
// Our list has to be converted to string in
//      such way that it can be used. The current conversion
//      might be a little slower comparing with other methods
//      due to the fact that the conversion is happening with
//      each cycle.

// Another solution consists in implementing a solution based
//      on the generation of the allocation for the string with
//      a fixed size equal with 20 characters (which is more than
//      enough).

// the structure definition for holding the characters (strings)
typedef struct charactersList charlist_t;
struct charactersList
{
    // the character
    unsigned char character;
    // the next character
    charlist_t* nextCharacter;
};

// The method will return a new initialized charlist_t element.
// The element returned is charlist_t
charlist_t* new_characterList_element()
{
    charlist_t* elementFromTheList;
```

```
if ((elementFromTheList = (charlist_t*) malloc(sizeof(charlist_t))) != 0)
{
    elementFromTheList->character = 0;
    elementFromTheList->nextCharacter = NULL;
}
else
{
    perror("The allocation using malloc() has failed.");
}

    return elementFromTheList;
}

// allocation free memory by the characters list
// listOfCharacters - represents a pointer for the first element within the list
void freeAllocation_CharactersList(charlist_t* listOfCharacters)
{
    charlist_t* currentCharacter = listOfCharacters;
    charlist_t* nextCharacter;

    while (currentCharacter != NULL)
    {
        nextCharacter = currentCharacter->nextCharacter;
        free(currentCharacter);
        currentCharacter = nextCharacter;
    }
}

// the function display the current list of characters
// the function will iterate through the whole list and it will print all the characters
void showCharactersList(charlist_t* list)
{
    charlist_t* nextCharacter = list;
```

```
while (nextCharacter != NULL)
{
    printf("%d ", nextCharacter->character);
    nextCharacter = nextCharacter->nextCharacter;
}
printf("\n");
}

// the function will return the next sequence of characters.
// the characters are treated as numbers 0-255
// the function proceeds by incrementation of the character from the first
    position
void nextCharactersSequence(charlist_t* listOfCharacters)
{
    listOfCharacters->character++;
    if (listOfCharacters->character == 0)
    {
        if (listOfCharacters->nextCharacter == NULL)
        {
            listOfCharacters->nextCharacter = new_characterList_element();
        }
        else
        {
            nextCharactersSequence(listOfCharacters->nextCharacter);
        }
    }
}

int main()
{
    charlist_t* sequenceOfCharacters;
    sequenceOfCharacters = new_characterList_element();

    // this while will work for all possibles combinations
    // this has to be stopped manually
```

