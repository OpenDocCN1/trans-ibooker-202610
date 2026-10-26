The output shows that we allocated memory five times and that the addresses are returned by `new`. When executing from the `main()` function when the `monitor` is destroyed, it dumps a report of memory that was allocated and released, as well as the files that were opened but never closed.

+   4. In the editor, type the following into the **Exercise3.cpp** file of the **File** class:

```
class File {
public:
    File(const char* name, const char* access) {
        m_file = fopen(name, access);
        if (m_file == nullptr) {
            throw std::ios_base::failure("failed to open file");
        }
    }

    ~File() {
        fclose(m_file);
    }

    operator FILE*() {
        return m_file;
    }
private:
    FILE* m_file{};
};
```

+   5. Click on the **Run** button to run Exercise 3 – it still leaks files and memory, but the code is correct.

+   6. Locate the **LeakFiles()** function and modify it so that it uses the new **File** class (like in the preceding code) to prevent file leaks:

```
void LeakFiles()
{
    File fh1{"HelloB1.txt", "w"};
    fprintf(fh1, "Hello B2\n");

    File fh2{"HelloB2.txt", "w"};
    fprintf(fh2, "Hello B1\n");
}
```