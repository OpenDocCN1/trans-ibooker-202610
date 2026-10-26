

## 练习 25：分数背包问题

在本练习中，我们将考虑 10 个物品，并尝试最大化背包中的价值，该背包的最大承重为 25 单位。让我们开始吧：

- 1. 首先，我们将添加所需的头文件，并定义一个 Object 结构体，该结构体将表示我们解决方案中的一个对象：

```
#include <iostream>
#include <algorithm>
#include <vector>
#include <random>
#include <numeric>

template <typename weight_type,
    typename value_type,
    typename fractional_type>
struct Object
{
    using Wtype = weight_type;
    using Vtype = value_type;
    using Ftype = fractional_type;

    Wtype weight;
    Vtype value;
    Ftype value_per_unit_weight;

    // NOTE: The following overloads are to be used for std::sort() and I/O
    inline bool operator< (const Object<Wtype,Vtype,Ftype>& obj) const
    {
        // An object is better or worse than another object only on the
        // basis of its value per unit weight
        return this->value_per_unit_weight < obj.value_per_unit_weight;
    }

    inline bool operator== (const Object<Wtype, Vtype, Ftype>& obj) const
    {
        // An object is equivalent to another object only if
        // its value per unit weight is equal
        return this->value_per_unit_weight == obj.value_per_unit_weight;
    }
```
```
```cpp
// Overloads the << operator so an object can be written directly to a
// stream
// e.g. Can be used as std::cout << obj << std::endl;

template <typename Wtype,
          typename Vtype,
          typename Ftype>
friend std::ostream& operator<<(std::ostream& os,
                              const Object<Wtype,Vtype,Ftype>& obj);
};

template <typename Wtype,
          typename Vtype,
          typename Ftype>
std::ostream& operator<<(std::ostream& os, const
Object<Wtype,Vtype,Ftype>& obj)
{
    os << "Value: " << obj.value
       << "\tWeight: " << obj.weight
       << "\tValue/Unit Weight: " << obj.value_per_unit_weight;
    return os;
}
```
```

请注意，我们重载了 `<` 和 `==` 运算符，因为我们将对对象向量使用 **std::sort()**。

2. 分数背包求解器的代码如下：

```
```cpp
template<typename weight_type,
         typename value_type,
         typename fractional_type>
auto fill_knapsack(std::vector<Object<weight_type, value_type, fractional_type>>& objects,
                  weight_type knapsack_capacity)
{
    std::vector<Object<weight_type, value_type, fractional_type>>
    knapsack_contents;
    knapsack_contents.reserve(objects.size());

    // Sort objects in the decreasing order
    std::sort(objects.begin(), objects.end());
```
```
```
std::reverse(objects.begin(), objects.end());

// Add the 'best' objects to the knapsack
auto current_object = objects.begin();
weight_type current_total_weight = 0;
while (current_total_weight <= knapsack_capacity &&
       current_object != objects.end())
{
    knapsack_contents.push_back(*current_object);

    current_total_weight += current_object->weight;
    current_object++;
}

// Since the last object overflows the knapsack, adjust weight
auto weight_of_last_obj_to_remove = current_total_weight - knapsack_capacity;

knapsack_contents.back().weight -= weight_of_last_obj_to_remove;
knapsack_contents.back().value -= knapsack_contents.back().value_per_unit_weight *
                                  weight_of_last_obj_to_remove;

return knapsack_contents;
}
```

上述函数按价值/重量比的降序对对象进行排序，然后挑选所有能装入背包的物品部分，直到背包被装满为止。

- 3. 最后，为了测试我们的实现，请添加以下测试和驱动程序代码：

```
void test_fractional_knapsack(unsigned num_objects, unsigned knapsack_capacity)
{
    using weight_type = unsigned;
    using value_type = double;
    using fractional_type = double;

    // Initialize the Random Number Generator
    std::random_device rd;
    std::mt19937 rand(rd());
    std::uniform_int_distribution<std::mt19937::result_type>
```
```
uniform_dist(1, num_objects);

// Create a vector of objects
std::vector<Object<weight_type, value_type, fractional_type>> objects;
objects.reserve(num_objects);
for (auto i = 0; i < num_objects; i++)
{
    // Every object is initialized with a random weight and value
    auto weight = uniform_dist(rand);
    auto value = uniform_dist(rand);
    auto obj = Object<weight_type, value_type, fractional_type> {
        static_cast<weight_type>(weight),
        static_cast<value_type>(value),
        static_cast<fractional_type>(value) / weight
    };

    objects.push_back(obj);
}

// Display the set of objects
std::cout << "Objects available: " << std::endl;
for (auto& o : objects)
    std::cout << o << std::endl;
std::cout << std::endl;

// Arbitrarily assuming that the total knapsack capacity is 25 units
auto solution = fill_knapsack(objects, knapsack_capacity);

// Display items selected to be in the knapsack
std::cout << "Objects selected to be in the knapsack (max capacity = "
    << knapsack_capacity<< "):" << std::endl;
for (auto& o : solution)
    std::cout << o << std::endl;
std::cout << std::endl;
}

int main(int argc, char* argv[])
{
    test_fractional_knapsack(10, 25);
}
```

上述函数创建对象，并使用 STL 随机数生成器生成的随机数据对它们进行初始化。接下来，它调用我们实现的分数背包求解器，然后显示结果。

4. 编译并运行这段代码！你的输出应如下所示：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_226_0.png)

请注意，求解器只取用了物品的一部分——也就是说，最后一个物品按重量计共有 5 个单位，却只取了其中的 4 个单位。这展示了物品在被选择放入背包之前可以被分割，这正是分数背包问题与 0-1（标准）背包问题的区别所在。

## 活动 11：区间调度问题

设想你的待办事项清单上有一系列任务（洗碗、去超市买日用品、从事一个旨在统治世界的秘密项目，以及其他类似的杂务）。每个任务都有一个 ID 标识，并且只能在特定的开始时间和结束时间之间完成。假设你希望完成尽可能多的任务。你应该选择哪个子集、按什么顺序处理这些任务，才能实现目标？假设你在任何时刻都只能处理一个任务。

3. 按如下方式实现调度函数：

- a. 按结束时间递增的顺序对任务列表进行排序。
- b. 贪心地选择完成结束时间最早的任务。
- c. 移除所有与当前所选任务重叠的任务（即在当前任务结束前开始的所有任务）。
- d. 如果列表中还有任务，则转到步骤 b。否则，返回选定的任务向量。

你的最终输出应与以下内容类似：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_228_0.png)

图 5.10：活动 11 的预期输出

## 注意

本活动的解决方案可在第 516 页找到。

## 贪心算法的要求

在上一节中，我们讨论了一些使用贪心方法可以得到最优解的问题示例。然而，一个问题只有在且仅在具有两个性质时，才能使用贪心方法得到最优解：最优子结构性质和贪心选择性质。在本节中，我们将尝试理解这些性质，并向你展示如何判断一个问题是否具备这些性质。

最优子结构：当给定问题 P 的最优解由它的子问题的最优解构成时，则称 P 具有最优子结构。

贪心选择：当给定问题 P 的最优解可以通过在每次迭代中选择局部最优解来获得时，则称 P 具有贪心选择性质。

为了理解最优子结构和贪心选择性质，我们将实现 Kruskal 的最小生成树算法。

## 最小生成树（MST）问题

最小生成树问题可以表述如下：

> “给定一个图 G = < V, E >，其中 V 是顶点集，E 是边集，每条边都有一个边权，请找出一棵生成树 T，它覆盖 V 中的所有顶点，并且总权重最小。”

最小生成树问题的一个现实应用是供水系统和交通网络的设计，因为设计者通常希望最小化所用管道的总长度或所建道路的总长度，同时确保服务覆盖所有指定用户。让我们通过以下示例来剖析这个问题。

假设给你一张地图上 12 个村庄的位置，要求你找到需要修建的道路的最小总长度，使得所有村庄之间都可以相互到达，并且这些道路不形成环。假设每条道路都可以双向通行。在该问题中，用图数据结构来表示村庄是一种自然的做法。假设下图 G 的顶点表示给定的 12 个村庄的位置，G 的边表示各顶点之间的距离：

