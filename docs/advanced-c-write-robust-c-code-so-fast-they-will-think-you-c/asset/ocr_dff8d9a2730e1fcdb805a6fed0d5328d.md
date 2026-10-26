When writing libraries or factories, the author of the library will not always know how a user will want to use the objects that have been created, so the recommendation is to return **unique_ptr<T>** from your factory methods. The reason for this is that the user can easily convert a **std::unique_ptr** into a **std::shared_ptr** by assignment:

```
std::unique_ptr<MyClass> unique_obj = std::make_unique<MyClass>();
std::shared_ptr<MyClass> shared_obj = unique_obj;
```

This will transfer ownership and leave **unique_obj** empty.

> **Note**
> 
> Once a resource has been made a shared resource, it cannot be reverted into a unique object.

# std::weak_ptr

A weak pointer is a variant of a shared pointer, but it doesn't hold a reference count to the resource. So, it does not prevent it from being released when the count goes to zero. Consider the following program structure, which may occur within a normal graphical user interface (GUI):

```
#include <iostream>

#include <memory>

struct ScrollBar;

struct TextWindow;

struct Panel
{
    ~Panel() {
        std::cout << "--Panel destroyed\n";
    }

    void setScroll(const std::shared_ptr<ScrollBar> sb) {
        m_scrollbar = sb;
    }

    void setText(const std::shared_ptr<TextWindow> tw) {
        m_text = tw;
```