
```cpp
virtual ~ICharacter() {
```

12. Click the `Run` button to rebuild and run the exercise. The output now reads as follows:

![Figure 2B.9: Output from the full polymorphic version](image/C14583_02B_09.jpg)

*Figure 2B.9: Output from the full polymorphic version*

We have now implemented an interface to our `ICharacter` characters and used them polymorphically by simply calling the `speak()` and `act()` methods through a base class pointer stored in a container.

## Classes, Structs, and Unions Revisited

Previously, we discussed that the difference between a `class` and a `struct` was the default access modifier – `private` for `class` and `public` for `struct`. This difference goes one step further – it applies to the base class if it does not specify anything:

```cpp
class DerivedC : Base  // inherits as if "class DerivedC : private Base" was used
{
};
```

```cpp
struct DerivedS : Base // inherits as if "struct DerivedS : public Base" was used
{
};
```

It should be noted that a `union` can neither be a base class nor be derived from a base class. If there is essentially no difference between a `struct` and a `class`, then which type should we use? Essentially, it is one of convention. A `struct` is used to bundle together several related elements, while a `class` can do things and has responsibilities. An example of a `struct` is as follows:

```cpp
struct Point    // A point in 3D space
{
  double m_x;
  double m_y;
  double m_z;
};
```

In the preceding code, we can see that it groups together three coordinates so that we can reason about a point in a 3D space. This structure can be passed as a coherent dataset to methods that need points, rather than three separate arguments per point. A `class`, on the other hand, models an object that can perform actions. Take a look at the following example:

```cpp
class Matrix
{
public:
  Matrix& operator*(const Matrix& rhs)
  {
      // nitty gritty of the multiplication
  }
private:
  // Declaration of the 2D array to store matrix.
};
```

A rule of thumb is to use a `class` if there is at least one `private` member as this implies that the details of the implementation will be behind the public member functions.

## Visibility, Lifetime, and Access

We have talked about creating our own types and declaring variables and functions while mainly focusing on simple functions and a single file. We will now look at what happens when there are multiple source files (translation units) that contain classes and function definitions. Also, we'll check which variables and functions can be visible from the other parts of the source files, how long the variables live, and look at the difference between internal and external linkage. In *Chapter 1*, *Anatomy of Portable C++ Software*, we saw how the toolchain works to compile the source files and produce the object files and that the linker puts it all together to form an executable program.

When a compiler processes a source file, it generates an object file that contains the translated C++ code and enough information for the linker to resolve any references from the compiled source file to another source file. In *Chapter 1*, *Anatomy of Portable C++ Software*, `CxxTemplate.cpp` called `sum()`, which is defined in the `SumFunc.cpp` file. When the compiler constructs an object file, it creates the following segments:

*   `Code segment` (also known as text): This is the translation of the C++ functions into the target machine instructions.
*   `Data segment`: This contains all the variables and data structures that are declared in the program, not local or allocated off the heap or stack, and are initialized.
*   `BSS segment`: This contains all the variables and data structures that are declared in the program, not local or allocated off the heap or stack, and are not initialized (but will be initialized to zero).
*   `Database of exported symbols`: A list of variables and functions that are in this object file and their location.
*   `Database of referenced symbols`: A list of variables and functions this object file needs from outside itself and where they are used.

#### Note

`BSS` is used to name the uninitialized data segment and its name is historically derived from Block Started by Symbol.

The linker then collects all the code segments, data segments, and `BSS` segments together to form the program. It uses the information in the two databases (DB) to resolve all the referenced symbols to the exported symbols list and patch the code segments with this information so that they can operate correctly. Graphically, this is depicted as follows:

![Figure 2B.10: Parts of the object files and the executable file](image/C14583_02B_10.jpg)

*Figure 2B.10: Parts of the object files and the executable file*

For the purposes of the discussion that follows, `BSS` and data segments will be referred to simply as data segment (the only difference being that `BSS` is not initialized). When a program is executed, it is loaded into memory and its memory looks a bit like the executable file layout – it contains the text segment, data segment, `BSS` segment, and free memory allocated by the host system, which contains what is known as the `stack` and the `heap`. The `stack` typically starts at the top of the memory and grows down, while the `heap` starts where `BSS` finishes and grows up, toward the `stack`:

![Figure 2B.11: CxxTemplate runtime memory map](image/C14583_02B_11.jpg)

*Figure 2B.11: CxxTemplate runtime memory map*

The section of the program in which a variable or identifier is accessible is known as the `scope`. There are two broad categories of `scope`:

*   `Local scope` (also known as `block scope`): This applies to anything declared within a block that is enclosed with curly braces (`{}`). The variable can be accessed inside the curly braces. Just like blocks can be nested, so can the scope of the variable. This typically includes local variables and function parameters, and these are generally stored in the `stack`.
*   `Global / file scope`: This applies to variables declared outside a normal function or class, as well as to the normal functions. The variable can be accessed anywhere in the file and possibly from other files (global) if the linkage is correct. These variables are allocated memory by the linker in the data segment. The identifiers are placed into the global namespace, which is the default namespace.

### Namespaces

We can think of a `namespace` as a dictionary of names of variables, functions, and user-defined types. For small programs, it is ok to use the global `namespace` as there is very little chance of you creating multiple variables with the same name and getting name clashes. As programs get larger, and more third-party libraries are included, the chance of name clashes increases. Therefore, library writers will put their code into a `namespace` (that is hopefully unique). This allows the programmer to control access to the identifiers in the `namespace`. By using the standard library, we have already been using the `std` namespace. A `namespace` is declared like so:

```cpp
namespace name_of_namespace {  // put declarations in here }
```

It is usual for `name_of_namespace` to be short, and namespaces can be nested.

#### Note

A good use of namespaces can be seen in the `boost` libraries here: `https://www.boost.org/`.

A variable has another attribute, that of `lifetime`. There are three fundamental lifetimes; two that are managed by the compiler and one that the programmer chooses:

*   `Automatic lifetime`: Local variables are created when they are declared and are destroyed upon exiting the scope that they are in. These are managed by the `stack`.
