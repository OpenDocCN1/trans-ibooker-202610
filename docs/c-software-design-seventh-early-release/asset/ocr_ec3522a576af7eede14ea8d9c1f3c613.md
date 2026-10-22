Figure 9-6. Dependency graph for the Decorator design pattern.

# A Classic Implementation of the Decorator Design Pattern

Let us take a look at a complete, GoF style implementation of the Decorator design pattern by means of the given Item example:

```
//---- <Item.h> ----------------

#include <Money.h>

class Item
{
public:
    virtual ~Item() = default;
    virtual Money price() const = 0;
};
```

The Item base class represents the abstraction for all possible items. The only requirement is defined by the pure virtual price() function, which can be used to query for the price of the given item. The DecoratedItem class represents one possible implementation of the Item class (1):

```
//---- <DecoratedItem.h> ----------------

#include <Item.h>
#include <memory>
#include <stdexcept>
#include <utility>

class DecoratedItem : public Item 1
{
public:
    explicit DecoratedItem( std::unique_ptr<Item> item ) 3
        : item_( std::move(item) )
    {
        if( !item_ ) {
            throw std::invalid_argument( "Invalid item" );
        }
    }

```