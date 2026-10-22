```
std::unique_ptr<Concept> clone() const override
{
    return std::make_unique<Model<T>>(*this);
}

    T item_;
};

    std::unique_ptr<Concept> pimpl_;
};
```

In this implementation, the Item class defines a nested Concept base class in its private section ④. As usual, the Concept base class represents the set of requirements (i.e. the expected behavior) for the wrapped types, which are expressed by the price() and clone() member functions. These requirements are implemented by the nested Model class template ⑤. Model implements the price() function by forwarding the call to the price() member function of the stored item_ data member, and the clone() function by creating a copy of the stored item.

The public section of the Item class should look familiar:

```
//---- <Item.h> ----------------

// ...

class Item
{
public:
    template< typename T >
    Item( T item ) ⑥
        : pimpl_( std::make_unique<Model<T>>( std::move(item) ) )
    {}

    Item( Item const& item ) : pimpl_( item.pimpl_->clone() ) {}

    Item& operator=( Item const& item )
    {
        pimpl_ = item.pimpl_->clone();
        return *this;
    }
```