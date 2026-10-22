```
//---- <Item.h> ----------------

#include <Money.h>

class Item
{
public:
    virtual ~Item() = default;

    virtual Money price() const = 0;
    // ...

protected:
    double taxRate_;
};
```

This apparently worked well for some time, until one day, when they were asked to also take different rates of discount into account. This apparently required a lot of effort to refactor the large amount of the existing classes for their numerous different items. You can easily imagine that this was necessary because all derived classes were accessing the protected data members. “Yes, you should always design for change…”, you think to yourself.¹

They continue by admitting to their unfortunate misdesign. Of course they should have done a better job to encapsulate the tax rates in the Item base class. However, along with this realization came the understanding, that when representing price modifiers by data members in the base class, any new kind of price modifier would always be an intrusive action and would always directly affect the Item class. For that reason, they started to think about how to avoid this kind of major refactoring in the future and how to enable the easy addition of new modifiers. “That’s the way to go!” you think to yourself. Unfortunately, the first approach that came to their mind was to factor out the different kinds of price modifiers by means of an inheritance hierarchy (see Figure 9-2).