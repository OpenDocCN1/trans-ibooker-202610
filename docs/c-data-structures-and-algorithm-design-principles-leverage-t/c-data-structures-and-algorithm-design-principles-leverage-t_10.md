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




4. 现在，让我们编写一个哈希函数。它只是使用 MD5 函数来计算哈希值：

```
void hash(const std::string& key)
{
    MD5(reinterpret_cast<const unsigned char*>(key.data()), key.length(),
    hashValue);
}
```

5. 让我们添加一个函数，以便插入电子邮件：

```
void add(const std::string& key)
{
    hash(key);
    for(auto it = &hashValue[0]; it < &hashValue[nHashes]; it++)
    {
        bits[*it] = true;
    }
    std::cout << key << " added in bloom filter." << std::endl;
}
```

正如我们所见，我们正在从哈希值缓冲区中的第 0 个字节迭代到第 nHashes 个字节，并将每个位设置为 1。

6. 同样，让我们添加一个函数来查找电子邮件地址：

```
bool mayContain(const std::string &key)
{
    hash(key);
    for (auto it = &hashValue[0]; it < &hashValue[nHashes]; it++)
    {
        if (!bits[*it])
        {
            std::cout << key << " email can by used." << std::endl;
            return false;
        }
    }
    std::cout << key << " email is used by someone else." << std::endl;
    return true;
};
```

```
bool search_result = needs_vaccination(p, people);

// Stop the clock
std::chrono::steady_clock::time_point end = std::chrono::steady_clock::now();

std::cout << "Time taken to search = " <<
    std::chrono::duration_cast<std::chrono::microseconds>
    (end - begin).count() << " microseconds" << std::endl;

if (search_result)
    std::cout << "Student (" << p.get_name().first
<< " " << p.get_name().second << ") "
        << "needs vaccination." << std::endl;
else
    std::cout << "Student (" << p.get_name().first
<< " " << p.get_name().second << ") "
        << "does not need vaccination." << std::endl;
}
```

5. 下面的函数实现了我们关于是否需要接种疫苗的逻辑：

```
bool needs_vaccination(Student P, std::vector<Student>& people)
{
    auto first = people.begin();
    auto last = people.end();

    while (true)
    {
        auto range_length = std::distance(first, last);
        auto mid_element_index = std::floor(range_length / 2);
        auto mid_element = *(first + mid_element_index);

        // Return true if the Student is found in the sequence and
        // he/she's not vaccinated
        if (mid_element == P && mid_element.is_vaccinated() == false)
            return true;
```

```
```cpp
else if (mid_element == P && mid_element.is_vaccinated() == true)
    return false;
else if (mid_element > P)
    std::advance(last, -mid_element_index);
if (mid_element < P)
    std::advance(first, mid_element_index);

// Student not found in the sequence and therefore should be
vaccinated
    if (range_length == 1)
        return true;
}
}
```
```

6. 最后，驱动程序代码如下：

```
```cpp
int main()
{
    // Generate a Student to search
    auto p = generate_random_Student(1000);

    search_test(1000, p);
    search_test(10000, p);
    search_test(100000, p);

    return 0;
}
```
```

> > **注意**
> >
> > 由于我们在第 3 步中随机化了值，你的输出可能会与本活动所示的预期输出有所不同。

## 活动 9：部分排序

部分快速排序只是对练习 20（快速排序）中演示的原始快速排序算法稍作修改。与该练习相比，只有第 4 步不同。下面是一个参考实现：

- 1. 添加以下头文件：
-   
- ```cpp
#include <iostream>
#include <vector>
#include <chrono>
#include <random>
#include <algorithm>
```

- 2. 接下来，我们将实现分区操作，如下所示：

```
```cpp
template <typename T>
auto partition(typename std::vector<T>::iterator begin,
               typename std::vector<T>::iterator end)
{
    auto pivot_val = *begin;
    auto left_iter = begin + 1;
    auto right_iter = end;

    while (true)
    {
        // Starting from the first element of vector,
        // find an element that is greater than pivot.
        while (*left_iter <= pivot_val && std::distance(left_iter, right_iter) > 0)
            left_iter++;

        // Starting from the end of vector moving to the beginning,
        // find an element that is lesser than the pivot.
        while (*right_iter > pivot_val && std::distance(left_iter, right_iter) > 0)
            right_iter--;

        // If left and right iterators meet, there are no elements left to swap.
        // Else, swap the elements pointed to by the left and right iterators
```

6. 下面的函数实现了我们排序函数的测试逻辑：

```
// Sort the first K elements of a random vector of a given 'size'
template <typename T>
void test_partial_quicksort(size_t size, size_t k)
{
    // Create two copies of a random vector to use for the two algorithms
    auto random_vec = generate_random_vector<T>(size);
    auto random_vec_copy(random_vec);

    std::cout << "Original vector: "<<std::endl;
    print_vector<T>(random_vec);

    // Measure the time taken by partial quick sort
    std::chrono::steady_clock::time_point
    begin_qsort = std::chrono::steady_clock::now();
    partial_quick_sort<T>(random_vec.begin(), random_vec.end()-1, k);
    std::chrono::steady_clock::time_point
    end_qsort = std::chrono::steady_clock::now();

    std::cout << std::endl << "Time taken by partial quick sort = "
        << 'std::chrono::duration_cast<std::chrono::microseconds>'
        (end_qsort - begin_qsort).count()
        << " microseconds" << std::endl;

    std::cout << "Partially sorted vector (only first "<< k <<" elements):";
    print_vector<T>(random_vec);

    // Measure the time taken by partial quick sort
    begin_qsort = std::chrono::steady_clock::now();
    quick_sort<T>(random_vec_copy.begin(), random_vec_copy.end()-1);
    end_qsort = std::chrono::steady_clock::now();

    std::cout << std::endl <<"Time taken by full quick sort = "
        << std::chrono::duration_cast<std::chrono::microseconds>
        (end_qsort - begin_qsort).count()
        << " microseconds" << std::endl;
```

```
std::cout << "Fully sorted vector: ";
print_vector<T>(random_vec_copy);
}
```

- 7. 最后，添加驱动程序代码，如下所示：

```
int main()
{
    test_partial_quicksort<unsigned>(100, 10);
    return 0;
}
```

## 活动 10：在 MapReduce 中实现 WordCount

在本活动中，我们将实现 MapReduce 模型来解决 WordCount 问题。以下是本活动的解决方案：

- 1. 实现 map 任务，如下所示：

```
struct map_task : public mapreduce::map_task<
    std::string,                              // MapKey (filename)
    std::pair<char const*, std::uintmax_t>>  // MapValue (memory mapped file contents)
{
    template<typename Runtime>
    void operator()(Runtime& runtime, key_type const& key, value_type& value) const
    {
        bool in_word = false;
        char const* ptr = value.first;
        char const* end = ptr + value.second;
        char const* word = ptr;
        // Iterate over the contents of the file, extract words and emit a <word,1> pair.
        for (; ptr != end; ++ptr)
        {
            // Convert the character to upper case.
            char const ch = std::toupper(*ptr, std::locale::classic());
            if (in_word)
            {
                if ((ch < 'A' || ch > 'Z') && ch != '\'')
                {
                    runtime.emit_intermediate(std::pair<char const*,
                        std::uintmax_t>(word, ptr - word), 1);

```

```
in_word = false;
}
}
else if (ch >= 'A' && ch <= 'Z')
{
    word = ptr;
    in_word = true;
}
}

// Handle the last word.
if (in_word)
{
    assert(ptr > word);
    runtime.emit_intermediate(std::pair<char const*,
        std::uintmax_t>(word, ptr - word), 1);
}
}
};
```

上述 map 函数分别应用于输入目录中的每个文件。输入文件的内容作为value中的字符指针被接收。然后，内层循环遍历文件的内容，提取不同的单词，并发出 < key, value > 键值对，其中 key 是一个单词，value 设置为 1。

- 2. 实现 reduce 任务，如下所示：

```
template<typename KeyType>
struct reduce_task : public mapreduce::reduce_task<KeyType, unsigned>
{
    using typename mapreduce::reduce_task<KeyType, unsigned>::key_type;

    template<typename Runtime, typename It>
    void operator()(Runtime& runtime, key_type const& key, It it, It const ite) const
    {
        runtime.emit(key, std::accumulate(it, ite, 0));
    }
};
```

然后，reduce 操作可以应用于 map 函数发出的所有 < key, value > 键值对。由于上一步将 value 设置为 1，我们现在可以使用 std::accumulate() 来获取某个 key 在 reduce 操作的输入对中出现的总次数。



