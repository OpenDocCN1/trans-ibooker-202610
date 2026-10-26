```
std::vector<std::function<void(int)>> m_delegates;
};
```

The tests all run and pass again.

14. We now have the basic multicast delegate class implemented. We now need to convert it to a template- based class. Update the existing tests, by changing all of the declarations of Delegate to Delegate<int> in the three tests.

15. Now update the Delegate class by adding template<class Arg> before the class to convert it to a template, and substituting the four occurrences of int with Arg:

```
template<class Arg>
class Delegate
{
public:
    Delegate() = default;

    Delegate& operator+=(const std::function<void(Arg)>& delegate)
    {
        m_delegates.push_back(delegate);
        return *this;
    }

    void Notify(Arg value) const
    {
        for(auto& delegate : m_delegates)
        {
            delegate(value);
        }
    }

    void operator()(Arg value)
    {
        Notify(value);
    }
private:
    std::vector<std::function<void(Arg)>> m_delegates;
};
```

16. All the tests now run and pass as previously, so it stills works for int arguments for the handlers.