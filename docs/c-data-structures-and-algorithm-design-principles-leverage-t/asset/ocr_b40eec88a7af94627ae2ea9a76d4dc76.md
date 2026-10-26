6. The following function implements the testing logic for our sorting functions:

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