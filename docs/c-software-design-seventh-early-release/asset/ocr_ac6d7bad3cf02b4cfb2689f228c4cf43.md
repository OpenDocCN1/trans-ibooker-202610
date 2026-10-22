object, which is used to guarantee that there is at most one instance of this allocator.

All C++ allocators from the pmr namespace, including the allocator returned by null_memory_resource() and the monotonic_buffer_resource, are derived from the std::pmr::memory_resource base class. The first design pattern becomes visible if you look at the memory_resource class definition:

```
namespace std::pmr {

class memory_resource
{
public:
    // ... a virtual destructor, some constructors and assignment operators

    [[nodiscard]] void* allocate(size_t bytes, size_t alignment);
    void deallocate(void* p, size_t bytes, size_t alignment);
    bool is_equal(memory_resource const& other) const noexcept;

private:
    virtual void* do_allocate(size_t bytes, size_t alignment) = 0;
    virtual void do_deallocate(void* p, size_t bytes, size_t alignment) = 0;
    virtual bool do_is_equal(memory_resource const& other) const noexcept = 0;
};

} // namespace std::pmr
```

You may notice that the three functions in the public section of the class have a virtual counterpart in the private section of the class. Whereas the public allocate(), deallocate() and is_equal() functions represent the user-facing interface of the class, the do_allocate(), do_deallocate() and do_is_equal() functions represent the interface for derived classes. This separation of concerns is an example of the Non-Virtual Interface (NVI) idiom, which itself is an example of the Template Method design pattern.²⁰