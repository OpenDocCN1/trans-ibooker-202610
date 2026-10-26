13. All that is left is to call our functions in `main()` and print the output:

```
```cpp
int main()
{
        Input();
        BuildGraph();
        GetShortestPaths();

        for(auto it : grid)
        {
                cout << it << endl;
        }
        return 0;
}
```
```