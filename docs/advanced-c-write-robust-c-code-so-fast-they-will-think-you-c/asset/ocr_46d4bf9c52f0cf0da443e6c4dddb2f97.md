```
};

Dialog::Dialog() : m_pImpl(new DialogImpl)
{
}

Dialog::~Dialog()
{
    delete m_pImpl;
}

void Dialog::create(const char* message)
{
    m_pImpl->create(message);
}

bool Dialog::show()
{
    return m_pImpl->show();
}
```

We note several things from this:

+   - We define the implementation class **DialogImpl** before we define the methods required for Dialog. This is necessary because Dialog will need to exercise the methods through **m_pImpl** which means that they need to be defined first.
+   - The **Dialog** constructor and destructor are responsible for the memory management.
+   - We include all the necessary header files required for the implementation, in the implementation file only. This reduces the coupling by minimizing the number of headers included in the **Dialog.hpp** file.