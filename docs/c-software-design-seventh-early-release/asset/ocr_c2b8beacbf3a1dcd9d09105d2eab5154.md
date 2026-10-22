+   - Function parameters in a subtype must be contravariant: in a member function, the subtype can accept a super type of the function parameter in the according member function of the super type. This property does not have direct language support in the C++ language.

```
struct Base { /*...some virtual functions, including destructor...*/ };
struct Derived : public Base { /*...*/ };
```

```
struct X
{
    virtual ~X() = default;
    virtual void f( Derived* );
};
```

```
struct Y : public X
{
    void f( Base* ) override;  // Contravariant function
parameter; Not
                        // supported in C++. Therefore the
function
                        // does not override, but fails to
compile.
};
```

+   - Invariants of the super type must be preserved in a subtype: any expectation about the state of a super type must always be valid before and after all calls to any member function, including the member functions of the subtype.

```
struct X
{
    explicit X( int v = 1 )
        : value_(v)
    {
        if( v < 1 || v > 10 ) throw std::invalid_argument( /*...*/ );
    }

    virtual ~X() = default;

    int get() const { return value_; }
```