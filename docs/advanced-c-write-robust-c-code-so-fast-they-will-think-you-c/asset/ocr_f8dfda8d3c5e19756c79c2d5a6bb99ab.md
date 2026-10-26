```
Notify(std::forward<ArgTypes>(args)...);
}
private:
    std::vector<std::function<void(ArgTypes...)>> m_delegates;
};
```

The tests should still run and pass.

19. Add two more tests – zero argument test, and a mutlipl e argument test:

```
TEST_F(DelegateTest, DualCallbacksNoArgs)
{
    Delegate delegate;

    delegate += [] () { std::cout << "CB1\n"; };
    delegate += [] () { std::cout << "CB2\n"; };
    delegate.Notify();
    std::string result = m_buffer.str();
    ASSERT_STREQ("CB1\nCB2\n", result.c_str());
}
```

```
TEST_F(DelegateTest, DualCallbacksStringAndInt)
{
    Delegate<std::string&, int> delegate;

    delegate += [] (std::string& value, int i) {
        std::cout << "1: = " << value << "," << i << "\n"; };
    delegate += [] (std::string& value, int i) {
        std::cout << "2: = " << value << "," << i << "\n"; };
    std::string hi{"hi"};
    delegate.Notify(hi, 52);
    std::string result = m_buffer.str();
    ASSERT_STREQ("1: = hi,52\n2: = hi,52\n", result.c_str());
}
```

All the tests run and pass showing that we have now implemented the desired Delegate class.