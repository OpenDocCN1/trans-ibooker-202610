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

The preceding function sorts the objects in decreasing order of their value/weight ratio and then picks all the fractions of objects that can fit in the knapsack until the knapsack is full.

+   3. Finally, to test our implementation, add the following test and driver code:

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