
7. Click on the `Run` button to run Exercise 3. If you have modified `LeakFiles()` correctly, then the output will be as follows:

![Figure 3.21: No file leaks](image/C14583_03_21.jpg)

Figure 3.21: No file leaks

8. Now in `Exercise3.cpp`, add the following `CharPointer` class:

```cpp
class CharPointer
{
public:
    void allocate(size_t size)
    {
        m_memory = new char[size];
    }
    operator char*() { return m_memory; }
private:
    char* m_memory{};
};
```

9. Modify `LeakPointers()` to read like so:

```cpp
void LeakPointers()
{
    CharPointer memory[5];
    for (auto i{0} ; i<5 ; i++)
    {
        memory[i].allocate(20);
        std::cout << "allocated 20 bytes @ " << (void*)memory[i] << "\n";
    }
}
```

10. Click on the `Run` button to run Exercise 3 – it still has memory leaks, but the code is correct.

11. Now, add the following destructor to `CharPointer`. Note that the `delete` operator uses the array `[]` syntax:

```cpp
~CharPointer()
{
    delete[] m_memory;
}
```

12. Click on the `Run` button again to run Exercise 3 – this time, you should see that the monitor reports no leaks:

![Figure 3.22: No leaks – memory or files](image/C14583_03_22.jpg)

Figure 3.22: No leaks – memory or files

The implementation of `File` and `CharPointer` deliver on the `RAII` design approach, but there are other considerations when designing these. For example, do we want the copy constructor or copy assignment functions? In both cases, it could be a problem to just copy the resource from one object to another as this could result in two attempts to close a file handle or delete the memory. Typically, this would lead to undefined behavior. Next, we will re-visit the special member functions in light of implementing a resource management object such as `File` or `CharPointer`.

### Special Coding Techniques

The code for `Exercise 3`, `Implementing RAII for Memory and File Handles`, has been specially written so that we can monitor the usage of the memory and the file handles and report any leaks on exit. Visit the `monitor.h` and `monitor.cpp` files and examine the two techniques that were used to make the monitor possible:

*   `Preprocessor macros`: This is the special use of a preprocessor macro to demonstrate the leaks and should not be used in production code, that is, replacing a function by text substitution.

    If you program to the Windows API, you may occasionally be caught with your method names clashing with the macros Microsoft uses for its API methods. For example, do not call any of your methods `SendMessage` if you include `windows.h`. If you do, then depending on whether you are building ASCII mode or Unicode mode, it will be substituted with `SendMessageA` or `SendMessageW` respectively.

*   `Defining our own new handler`: This is an advanced technique and unless you write embedded code, it's unlikely you will need it.

### C++ doesn't Need finally

Other languages (`C#`, `Java`, and `Visual Basic.NET`) that support an exception throwing mechanism have a `try/catch/finally` paradigm where the code in the `finally` block is called on exit from the try block – either normally or by exception. C++ has no `finally` block because it has access to a better mechanism that ensures that we cannot forget to release a resource – `RAII`. Since the resource is represented by a local object, the local object's destructor will free the resource.

The added advantage of this design pattern is that if a lot of resources are being managed, then the `finally` block is proportionally large. `RAII` removes the need for finally and leads to code that is easier to maintain.

### RAII and the STL

The Standard Template Library (`STL`) utilizes `RAII` in many of its templates and classes. For example, the smart pointers that were introduced in C++11, that is `std::unique_ptr` and `std::shared_ptr`, help avoid many problems by ensuring that memory is deallocated when it has been finished with, or by ensuring that the memory is not deallocated if it is used elsewhere. Other examples in the `STL` include `std::string` (memory), `std::vector` (memory), and `std::fstream` (file handles).

### Who Owns This Object?

With the preceding implementations of `File` and `CharPointer`, we have tested Resource Management with `RAII`. Let's explore it further. Firstly, we will define a class that has more than just one resource:

```cpp
class BufferedWriter
{
public:
    BufferedWriter(const char* filename);
    ~BufferedWriter();
    bool write(const char* data, size_t length);
private:
    const size_t BufferSize{4096};
    FILE* m_file{nullptr};
    size_t m_writePos{0};
    char* m_buffer{new char[BufferSize]};
};
```

The class is used for buffering the writing to the file.

#### Note

This is not usually necessary when using `iostream` derived classes as they already provide buffering.

Each call to the `write()` function will add data into the allocated buffer until it reaches the `BufferSize` at which point the data is actually written to the file and the buffer is reset.

But what if we wanted to assign this instance of `BufferedWriter` to another instance or copy it? What is the right behavior?

If we just let the default copy constructor/copy assignment do its their thing we get a member-wise copy of the items. This would mean that we have two instances of `BufferedWriter` that hold the same handle to the file and the pointer to the buffer. When the first instance of the object is destroyed, being the good programmers that we are, we will clean up the file by closing it and the memory by deleting it. The second instance now has a file handle that is defunct and a pointer to memory that we have told the operating system to recover for the next user. Any attempt to use these resources, including destroying them, will result in undefined behavior and mostly likely a program crash. The default copy constructor/copy-assignment operators execute what is known as a `shallow copy` – that is, it copies all the members as they are, bit by bit (but not what they refer to).

The two resources that we have can be treated differently. Firstly, there should only be one class that owns the `m_buffer`. There are two options in dealing with this:

*   Prevent the copy of the class and hence the memory
*   Perform a `deep copy` where the buffer in the second instance has been allocated by the constructor and the contents of the first buffer is copied

Secondly, there should only be one class that owns the file handle (`m_file`). There are two options in dealing with this:

*   Prevent the copy of the class and hence the file handle
*   Transfer the `ownership` from the original instance to the second instance and mark the original as invalid or empty (whatever that means)

It is easy enough to implement a deep copy, but how do we transfer ownership of a resource? To answer this question, we need to look at temporary objects and value categories again.

### Temporary Objects

A temporary object is created to store the intermediate results of an expression before the result is deposited into a variable (or just forgotten). An expression is any code that returns a value, including passing by value to a function, returning a value from a function, implicit conversion, literals, and binary operators. Temporary objects are `rvalue expressions` and they have memory, temporarily allocated for them as a location, to place the expression result. It is this creation of temporaries and copying data between them that caused some performance issues prior to C++11. To address this issue, C++11 introduced `rvalue references` to enable what is known as `move semantics`.

### Move Semantics
