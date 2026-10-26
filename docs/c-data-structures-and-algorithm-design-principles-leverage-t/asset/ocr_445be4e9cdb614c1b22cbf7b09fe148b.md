```
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

As we can see, the heap node will contain three things – data, the position of the list in the input, and the position of the data item inside that list.

3. Let's build the heap. The idea is to have a min heap with the smallest element from all the lists. So, when we pop from the heap, we are guaranteed to get the smallest element. After removing that element, we need to insert the next element from the same list, if it's available:

```
std::vector<node> heap;
for(int i = 0; i < input.size(); i++)
{
    heap.push_back({input[i][0], i, 0});
    std::push_heap(heap.begin(), heap.end(), comparator);
}
```

4. Now, we'll build the resultant vector. We'll simply remove the elements from the heap until it is empty and replace it with the next element from the same list it belongs to, if available:

```
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

```