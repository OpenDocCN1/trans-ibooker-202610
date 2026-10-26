7. Create a method named **insertEnd()** and insert elements from **0** to **N** into a container at the end. Add the following code to implement this:

```
template<class C> void insertEnd(C &l)
{
    for(int i = 0; i < N; ++i)
    {
        l.insert(l.end(), i);
    }
}
```

8. Write the following code in the **main** method:

```
int main()
{
    std::list<int> l;
    std::vector<int> v;
    // list
    {
        Timer t("list random");
        insertRandom(l);
    }

    {
        Timer t("list end");
        insertEnd(l);
    }
    {
        Timer t("list start");
        insertStart(l);
    }
    // vector
    {
        Timer t("vect random");
        insertRandom(v);
    }

    {
        Timer t("vect end");
        insertEnd(v);
    }
    {
        Timer t("vect start");
```