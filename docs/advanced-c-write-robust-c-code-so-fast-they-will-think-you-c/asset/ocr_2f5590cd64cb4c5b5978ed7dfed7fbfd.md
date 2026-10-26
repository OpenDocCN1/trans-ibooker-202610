+   12. In **Kitchen::Impl**, add a constructor with a `std::string` argument and the **processOrder()** method. The **Kitchen::Impl** declaration should now look like the following:

```
struct Kitchen::Impl
{
    Impl(std::string chef);
    std::string processOrder(std::string order);

    std::string searchForRecipe(std::string order);
    std::string searchForDessert(std::string order);
    std::string cookRecipe(std::string recipe);
    std::string serveDessert(std::string dessert);

    std::vector<Recipe>::iterator getRecipe(std::string recipe);
    std::vector<Dessert>::iterator getDessert(std::string recipe);

    std::string m_chef;
    std::vector<Recipe> m_recipes;
    std::vector<Dessert> m_desserts;
};
```

+   13. In **kitchen.cpp**, add the **#include <vector>, #include "recipe.hpp"** and **#include "dessert.hpp"** to the top of the file.

+   14. Click on the **Run** button to re-build the program, and this time it will fail with two undefined references – **Kitchen::Kitchen** and **Kitchen::processOrder**.

+   15. In **Kitchen.cpp**, after the **Kitchen::Impl** method definitions, add the following two methods:

```
Kitchen::Kitchen(std::string chef) : m_impl(std::make_
unique<Kitchen::Impl>(chef))
{
}

std::string Kitchen::processOrder(std::string order)
{
    return m_impl->processOrder(order);
}
```