

- 3. The **transform_test()** function randomly generates a vector of a given size and applies a transformation, f(x) = x², to the vector.

### Note

**std::transform()** does not change the original vector and instead returns the result in a separate vector, while **std::for_each()** modifies the input vector. Another difference between the two is that **std::transform()** does not guarantee that the input function, f, will be applied from the first to the last element of the container; that is, the order of function application does not necessarily match the order of elements. Starting with C++17, **std::transform()** also supports native parallelization by accepting **ExecutionPolicy** as the first argument.

The reduce operation is implemented in the C++ Standard Library as **std::accumulate()** and **std::reduce()** (available only in C++17 and later):

```
void reduce_test(size_t size)
{
    std::vector<int> S;
    std::random_device rd;
    std::mt19937 rand(rd());
    std::uniform_int_distribution<std::mt19937::result_type> uniform_dist(1, size);

    // Insert random elements
    for (auto i = 0; i < size; i++)
        S.push_back(uniform_dist(rand));

    std::cout << std::endl << "Reduce test== " << std::endl << "Original array, S: ";
    for (auto i : S)
        std::cout << i << " ";
    std::cout << std::endl;
    // Accumulate
    std::cout<<"std::accumulate() = " << std::accumulate(S.begin(), S.end(), 0, [](int acc, int x) {return acc+x;});

    std::cout << std::endl;
}
```

- 4. Add the following driver code:

```
int main()
{
    transform_test(10);
    reduce_test(10);
    return 0;
}
```

- 5. Compile and run the code. Your output should look as follows:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_202_0.png)

## Integrating the Parts – Using a MapReduce Framework

To write a program using the MapReduce model, we must be able to express our desired computation in a series of two stages: Map (also referred to as Partition), where the program reads the input and creates a set of intermediate `<key,value>` pairs, and Reduce, where the intermediate `<key,value>` pairs are then combined in the required manner to generate the final result. The following diagram illustrates this idea:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_202_1.png)

The main value that frameworks such as Hadoop add to the MapReduce programming model is that they make the map and reduce operations distributed and highly scalable so that the computation runs on a cluster of machines and the total time taken is reduced.

We shall use the MapReduce framework to execute a sample task in the following exercise.

### Note

The following exercise and activity need the Boost C++ libraries to be installed on your system. Follow these links to get the Boost libraries:

- Windows: https://www.boost.org/doc/libs/1_71_0/more/getting_started/windows.html
- Linux/macOS: https://www.boost.org/doc/libs/1_71_0/more/getting_started/unix-variants.html

### Exercise 23: Checking Primes Using MapReduce

Given a positive integer, N, we wish to find out the prime numbers between 1 and N. In this exercise, we shall see how we can implement this using the MapReduce programming model and solve the problem using multiple threads. Let's get started:

- 1. Let's begin by including the required libraries and defining a function to check whether a given number is prime using prime factorization:

```
#include <iostream>
#include "mapreduce.hpp"

namespace prime_calculator {

    bool const is_prime(long const number)
    {
        if (number > 2)
        {
            if (number % 2 == 0)
                return false;

            long const n = std::abs(number);
            long const sqrt_number = static_cast<long>(std::sqrt(
                static_cast<double>(n)));

            for (long i = 3; i <= sqrt_number; i += 2)
            {
                if (n % i == 0)
                    return false;
            }
        }
        else if (number == 0 || number == 1)
            return false;

        return true;
    }
}
```

- 2. The following class is used to generate a range of numbers with a given difference between consecutive numbers (also called the step size):

```
template<typename MapTask>
class number_source : mapreduce::detail::noncopyable
{
public:
    number_source(long first, long last, long step)
        : sequence_(0), first_(first), last_(last), step_(step)
    {
    }

    bool const setup_key(typename MapTask::key_type& key)
    {
        key = sequence_++;
        return (key * step_ <= last_);
    }

    bool const get_data(typename MapTask::key_type const& key,
                        typename MapTask::value_type& value)
    {
        typename MapTask::value_type val;

        val.first = first_ + (key * step_);
        val.second = std::min(val.first + step_ - 1, last_);

        std::swap(val, value);
        return true;
    }

private:
    long sequence_;
    long const step_;
    long const last_;
    long const first_;
};
```

- 3. The following function defines the steps to be performed in the map stage:

```
struct map_task : public mapreduce::map_task<long, std::pair<long, long>>
{
    template<typename Runtime>
    void operator()(Runtime& runtime, key_type const& key, value_type const& value) const
    {
        for (key_type loop = value.first;
             loop <= value.second; loop++)
            runtime.emit_intermediate(is_prime(loop), loop);
    }
};
```

- 4. Now, let's implement the reduce stage:

```
struct reduce_task : public mapreduce::reduce_task<bool, long>
{
    template<typename Runtime, typename It>
    void operator()(Runtime& runtime, key_type const& key, It it, It ite) const
    {
        if (key)
            std::for_each(it, ite, std::bind(&Runtime::emit, &runtime, true, std::placeholders::_1));
    }
};
```

```
typedef
mapreduce::job<
    prime_calculator::map_task,
    prime_calculator::reduce_task,
    mapreduce::null_combiner,
    prime_calculator::number_source<prime_calculator::map_task>>
job;

} // namespace prime_calculator
```

The preceding namespace has three functions: first, it defines a function that checks whether a given number is prime; second, it defines a function that generates a range of numbers within given bounds; third, it defines the map and reduce tasks. The map function, as defined earlier, emits `<k, v>` pairs, where both k and v are of the long type, where k is 1 if v is a prime, and 0 if v is not a prime number. The reduce function then acts as a filter and outputs `<k, v>` pairs only where k = 1.

- 5. The following driver code then sets the relevant parameters and starts the MapReduce computation:

```
int main()
{
    mapreduce::specification spec;

    int prime_limit = 1000;

    // Set number of threads to be used
    spec.map_tasks = std::max(1U, std::thread::hardware_concurrency());
    spec.reduce_tasks = std::max(1U, std::thread::hardware_concurrency());

    // Set the source of numbers in given range
    prime_calculator::job::datasource_type datasource(0, prime_limit,
        prime_limit / spec.reduce_tasks);

    std::cout << "\nCalculating Prime Numbers in the range 0 .. " <<
              prime_limit << " ..." << std::endl;

    std::cout << std::endl << "Using "
              << std::max(1U, std::thread::hardware_concurrency()) << " CPU cores";

    // Run mapreduce
    prime_calculator::job job(datasource, spec);
```

### Activity 10: Implementing WordCount in MapReduce

In this chapter, we have seen how powerful the idea behind the divide-and-conquer technique can be as an exceedingly useful algorithm design technique, as well as in providing useful tools to handle large and complex computations. In this activity, we shall practice dividing a large problem into smaller parts, solving the smaller parts, and merging the subsequent results by using the MapReduce model that was presented in the preceding section.

Our problem definition has been taken from the original MapReduce paper, and is given as follows: given a set of files containing text, find the frequency of each word that appears in the files. For example, let’s say you are given two files with the following contents:

File 1:

```
The quick brown fox jumps over a rabbit
```

File 2:

```
The quick marathon runner won the race
```

Considering the input files, our program should output the following result:

| Word | Frequency |
| --- | ---: |
| The | 2 |
| quick | 2 |
| a | 1 |
| brown | 1 |
| fox | 1 |
| jumps | 1 |
| marathon | 1 |
| over | 1 |
| rabbit | 1 |
| race | 1 |
| runner | 1 |
| the | 1 |
| won | 1 |

Such problems often arise in indexing workloads, that is, when you are given a large corpus of text and are required to index the contents so that subsequent searches on the text can be made faster. Search engines such as Google and Bing heavily use such indexes.

