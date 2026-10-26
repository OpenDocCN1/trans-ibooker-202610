

```cpp
return;
}

if(found->is_dir)
{
    for(auto child: found->children)
    {
        std::cout << (child->is_dir ? "d " : "- ") << child->name << std::endl;
    }
}
else
{
    std::cout << "- " << found->name << std::endl;
}
};
};
```

9. 编写一个 `main` 函数，以便我们可以使用上述函数：

```cpp
int main()
{
    file_system fs;
    fs.add("usr", true);  // Add directory usr in "/"
    fs.add("etc", true);  // Add directory etc in "/"
    fs.add("var", true);  // Add directory var in "/"
    fs.add("tmp_file", false);  // Add file tmp_file in "/"

    std::cout << "Files/Directories under \"/\"" << std::endl;
    fs.show_path("/");  // List files/directories in "/"

    std::cout << std::endl;
    fs.change_dir("usr");
    fs.add("Packt", true);
    fs.add("Packt/Downloads", true);
    fs.add("Packt/Downloads/newFile.cpp", false);

    std::cout << "Let's see the contents of dir usr: " << std::endl;
    fs.show_path("usr");  // This will not print the path successfully, since we're already inside the dir usr. And there's no directory named usr inside it.

    std::cout << "Let's see the contents of \"/usr\"" << std::endl;
    fs.show_path("/usr");

    std::cout << "Let's see the contents of \"/usr/Packt/Downloads\"" <<
    std::endl;
    fs.show_path("/usr/Packt/Downloads");
}
```

上述代码的输出如下：

```
Files/Directories under "/"
d usr
d etc
d var
- tmp_file
Current working directory changed to usr.
Let's try to print the contents of usr:
No such path: usr.
Let's see the contents of "/usr"
d Packt
Contents of "/usr/Packt/Downloads"
- newFile.cpp
```

### 活动5：使用堆进行K路归并

在此活动中，我们将多个有序数组合并为一个有序数组。以下步骤将帮助你完成此活动：

1. 从所需头文件开始：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
```

2. 现在，实现归并的主要算法。它将接收一个由多个 int 向量组成的向量作为输入，其中包含所有已排序的向量。然后，它将返回归并后的 int 向量。首先，让我们构建堆节点：

```cpp
struct node
{
    int data;
    int listPosition;
    int dataPosition;
};

std::vector<int> merge(const std::vector<std::vector<int>>& input)
{
    auto comparator = [] (const node& left, const node& right)
    {
        if(left.data == right.data)
            return left.listPosition > right.listPosition;
        return left.data > right.data;
    };
```

正如我们所见，堆节点将包含三部分内容——数据、该列表在输入中的位置，以及该数据项在该列表中的位置。

3. 让我们构建堆。思路是建立一个包含所有列表中最小元素的最小堆。这样，当我们从堆中弹出元素时，可以保证得到最小的元素。移除该元素后，如果同一个列表中还有下一个元素，我们需要将其插入堆中：

```cpp
std::vector<node> heap;
for(int i = 0; i < input.size(); i++)
{
    heap.push_back({input[i][0], i, 0});
    std::push_heap(heap.begin(), heap.end(), comparator);
}
```

4. 现在，我们将构建结果向量。我们只需从堆中移除元素，直到堆为空，并且如果该元素所属的列表中还有下一个元素，则用下一个元素替换它：

```cpp
std::vector<int> result;
while(!heap.empty())
{
    std::pop_heap(heap.begin(), heap.end(), comparator);
    auto min = heap.back();
    heap.pop_back();

    result.push_back(min.data);
    int nextIndex = min.dataPosition + 1;
    if(nextIndex < input[min.listPosition].size())
    {
        heap.push_back({input[min.listPosition][nextIndex], min.listPosition, nextIndex});
        std::push_heap(heap.begin(), heap.end(), comparator);
    }
}

return result;
}
```

5. 让我们编写一个 **main** 函数，以便我们可以使用上述函数：

```cpp
int main()
{
    std::vector<int> v1 = {1, 3, 8, 15, 105};
    std::vector<int> v2 = {2, 3, 10, 11, 16, 20, 25};
    std::vector<int> v3 = {-2, 100, 1000};
    std::vector<int> v4 = {-1, 0, 14, 18};
    auto result = merge({v1, v2, v3, v4});
    for(auto i : result)
        std::cout << i << ' ';
    return 0;
}
```

你应该会看到以下输出：

```
-2 -1 0 1 2 3 3 8 10 11 14 15 16 18 20 25 100 105 1000
```

## 第3章：哈希表与布隆过滤器

### 活动6：将长URL映射为短URL

在此活动中，我们将创建一个程序，将较短的URL映射到对应的较长URL。请按照以下步骤完成此活动：

1. 让我们引入所需的头文件：

```cpp
#include <iostream>
#include <unordered_map>
```

2. 让我们编写一个名为 `URLService` 的结构体，它将为所需服务提供接口：

```cpp
struct URLService
{
    using ActualURL = std::string;
    using TinyURL = std::string;

private:
    std::unordered_map<TinyURL, ActualURL> data;
```

正如我们所见，我们创建了一个从短URL到原始URL的映射。这是因为我们使用短URL进行查找，并希望将其转换为原始URL。正如我们之前看到的，映射可以根据键快速查找。因此，我们将较短的URL作为映射的键，将原始URL作为映射的值。我们还创建了别名，以避免混淆我们讨论的是哪个字符串。

3. 让我们添加一个 **lookup**（查找）函数：

```cpp
public:

    std::pair<bool, ActualURL> lookup(const TinyURL& url) const
    {
        auto it = data.find(url);
        if(it == data.end())  // If small URL is not registered.
        {
            return std::make_pair(false, std::string());
        }
        else
        {
            return std::make_pair(true, it->second);
        }
    }
```

6. 现在，让我们编写一个函数，用于打印所有映射以进行日志记录：

```cpp
void printURLs() const
{
    for(const auto& entry: data)
    {
        std::cout << entry.first << " -> " << entry.second << std::endl;
    }
    std::cout << std::endl;
};
```

7. 现在，编写 **main** 函数，以便我们可以使用此服务：

```cpp
int main()
{
    URLService service;

    if(service.registerURL("https://www.packtpub.com/eu/big-data-and-business-intelligence/machine-learning-r-third-edition", "https://ml-r-v3"))
    {
        std::cout << "Registered https://ml-r-v3" << std::endl;
    }
    else
    {
        std::cout << "Couldn't register https://ml-r-v3" << std::endl;
    }

    if(service.registerURL("https://www.packtpub.com/eu/virtualization-and-cloud/hands-aws-penetration-testing-kali-linux", "https://aws-test-kali"))
    {
        std::cout << "Registered https://aws-test-kali" << std::endl;
    }
    else
    {
        std::cout << "Couldn't register https://aws-test-kali" << std::endl;
    }

    if(service.registerURL("https://www.packtpub.com/eu/application-development/hands-qt-python-developers", "https://qt-python"))
    {
        std::cout << "Registered https://qt-python" << std::endl;
    }
    else
    {
        std::cout << "Couldn't register https://qt-python" << std::endl;
    }

    auto findMLBook = service.lookup("https://ml-r-v3");
    if(findMLBook.first)
    {
        std::cout << "Actual URL: " << findMLBook.second << std::endl;
    }
    else
    {
        std::cout << "Couldn't find URL for book for ML." << std::endl;
    }

    auto findReactBook = service.lookup("https://react-cookbook");
    if(findReactBook.first)
    {
        std::cout << "Actual URL: " << findReactBook.second << std::endl;
    }
    else
    {
        std::cout << "Couldn't find URL for book for React." << std::endl;
    }

    if(service.deregisterURL("https://qt-python"))
    {
        std::cout << "Deregistered qt python link" << std::endl;
    }
}
```

### 活动7：电子邮件地址验证器

在此活动中，我们将创建一个验证器，用于检查用户请求的电子邮件地址是否已被占用。请按照以下步骤完成此活动：

1. 让我们引入所需的头文件：

```cpp
#include <iostream>
#include <vector>

#include <openssl/md5.h>
```

2. 让我们为布隆过滤器添加一个类：

```cpp
class BloomFilter
{
    int nHashes;
    std::vector<bool> bits;

    static constexpr int hashSize = 128/8;

    unsigned char hashValue[hashSize];
};
```

3. 让我们为其添加构造函数：

```cpp
BloomFilter(int size, int hashes) : bits(size), nHashes(hashes)
{
    if(nHashes > hashSize)
    {
        throw ("Number of hash functions too high");
    }
    if(size > 255)
    {
        throw ("Size of bloom filter can't be >255");
    }
}
```

由于我们将使用哈希值缓冲区中的每个字节作为不同的哈希函数值，而哈希值缓冲区的大小为16字节（128位），因此哈希函数的数量不能超过该值。由于每个哈希值只有1个字节，其可能的取值范围是0到255。因此，布隆过滤器的大小不能超过255。所以，我们在构造函数本身中就抛出了错误。

