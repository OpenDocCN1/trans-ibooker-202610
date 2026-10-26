```
void bar() { std::cout << "foo::bar called\n"; }
};

int main()
{
    std::cout << "\n\n----- Function Pointers -----\n";
    foo object;

    FnPtr fn{&object.bar};

    fn();

    std::cout << "Complete.\n";
    return 0;
}
```

When we attempt to compile this program, we get the following errors:

```
/usr/bin/make all
Scanning dependencies of target Pimpl
[ 33%] Building CXX object CMakeFiles/Pimpl.dir/main.cpp.o
/home/brian/Documents/advanced/Lesson04/main.cpp: In function 'int main()':
/home/brian/Documents/advanced/Lesson04/main.cpp:92:22: error: ISO C++ forbids taking the address of a bound member function to form a pointer
    FnPtr fn{&object.bar};
/home/brian/Documents/advanced/Lesson04/main.cpp:92:25: error: cannot convert 'void (foo::*)()' to 'FnPtr {aka void (*)()}' in initialization
    FnPtr fn{&object.bar};
CMakeFiles/Pimpl.dir/build.make:62: recipe for target 'CMakeFiles/Pimpl.dir/main.cpp.o' failed
make[2]: *** [CMakeFiles/Pimpl.dir/main.cpp.o] Error 1
make[1]: *** [CMakeFiles/Pimpl.dir/all] Error 2
CMakeFiles/Makefile2:72: recipe for target 'CMakeFiles/Pimpl.dir/all' failed
Makefile:94: recipe for target 'all' failed
"/usr/bin/make all" terminated with exit code 2. Build might be incomplete.
```

Figure 4.8: Errors compiling function pointer program

The text of the first error is **ISO C++ forbids taking the address of a bound member function to form a pointer to member function. Say '&foo::bar'**. It tells us that we should use a different form to obtain the function address. The real error in this case is the second error message: **error: cannot convert 'void (foo::*)()' to 'FnPtr {aka void (*)()} ' in initialization**. The real problem is that non-static member functions take a hidden argument when they are called – the this pointer.