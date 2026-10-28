

```java
public class Book {

    private final String author;
    private final String title;
    private final String isbn;

    // constructor omitted for brevity

    public String fetchPage(int pageNr) {...}

    // getters, equals() and hashCode() omitted for brevity
}
```

接下来，如果我们考虑到书籍通常由图书馆管理，那么我们可以将多个功能（例如添加、查找和移除书籍）封装在一个类中，如下所示：

```java
public class Library {

    private final Map<String, Book> books = new HashMap<>();

    protected void addBook(Book book) {
        books.putIfAbsent(book.getIsbn(), book);
    }

    protected boolean remove(Book book) {
        return books.remove(book.getIsbn(), book);
    }

    protected Book find(String isbn) {
        return books.get(isbn);
    }
}
```

最后，我们只需将 `Library`、`ReaderManager` 和 `Displayer` 封装在 `OnlineReaderSystem` 类中。该类如下所示：

```java
public class OnlineReaderSystem {

    private final Displayer displayer;
    private final Library library;
    private final ReaderManager readerManager;

    private Reader reader;
    private Book book;

    public OnlineReaderSystem() {
        displayer = new Displayer();
        library = new Library();
        readerManager = new ReaderManager();
    }

    public void displayReader(Reader reader) {
        this.reader = reader;
        displayer.displayReader(reader);
    }

    public void displayReader(String email) {
        this.reader = readerManager.find(email);
        if (this.reader != null) {
            displayer.displayReader(reader);
        }
    }

    public void displayBook(Book book) {
        this.book = book;
        displayer.displayBook(book);
    }

    public void displayBook(String isbn) {
        this.book = library.find(isbn);
```

```java
if (this.book != null) {
    displayer.displayBook(book);
}

public void nextPage() {
    displayer.nextPage();
}

public void previousPage() {
    displayer.previousPage();
}

public void addBook(Book book) {
    library.addBook(book);
}

public boolean deleteBook(Book book) {
    if (!book.equals(this.book)) {
        return library.remove(book);
    }

    return false;
}

public void addReader(Reader reader) {
    readerManager.addReader(reader);
}

public boolean deleteReader(Reader reader) {
    if (!reader.equals(this.reader)) {
        return readerManager.remove(reader);
    }

    return false;
}
```

当哈希函数为不同的键生成两个相同的索引时，我们就面临索引冲突。解决索引冲突最常用的技术是*线性探测*（该技术在线性表中搜索下一个空闲槽位，试图在数组中找到不包含键值对的槽位（索引））和*链地址法*（该技术将哈希表实现为链表的数组——冲突以链表节点的形式存储在同一个数组索引处）。下图是一个用于存储*姓名-电话*对的哈希表。它具有链地址法能力（查看*Marius-0838234*条目，它链到了*Karina-0727928*，因为它们的键*Marius*和*Karina*导致了相同的数组索引126）：

![](img/6709a71136a6b327c831c6c15aff2f6a_170_0.png)

**解决方案：** 首先，我们需要设计一个哈希表条目（`HashEntry`）。如前面的图所示，一个键值对包含三个主要部分：键、值，以及指向下一个键值对的链接（这样我们就实现了*链地址法*）。由于哈希表条目只能通过专用方法（如 `get()` 和 `put()`）访问，因此我们将其封装如下：

```java
public class HashTable<K, V> {

    private static final int SIZE = 10;

    private static class HashEntry<K, V> {

        K key;
        V value;
```

```java
HashEntry <K, V> next;

HashEntry(K k, V v) {
    this.key = k;
    this.value = v;
    this.next = null;
}
}
...
```

接下来，我们定义保存 `HashEntry` 的数组。出于测试目的，10 个元素的大小就足够了，而且它使我们能够轻松测试*链地址法*（较小的尺寸容易发生冲突）。实际上，这样的数组要大得多：

```java
private final HashEntry[] entries
    = new HashEntry[SIZE];
...
```

接下来，我们添加 `get()` 和 `put()` 方法。它们的代码相当直观：

```java
public void put(K key, V value) {

    int hash = getHash(key);

    final HashEntry hashEntry = new HashEntry(key, value);

    if (entries[hash] == null) {
        entries[hash] = hashEntry;
    } else { // collision => chaining
        HashEntry currentEntry = entries[hash];
        while (currentEntry.next != null) {
            currentEntry = currentEntry.next;
        }

        currentEntry.next = hashEntry;
    }
}
```

## 示例 7：文件系统

**问题：** 设计文件系统的主要类。

**需要询问：** 需要哪些功能？文件系统由哪些部分组成？

**面试官：** 你的设计应支持目录和文件的添加、删除和重命名。我们谈论的是大多数操作系统都具有的目录和文件的层次结构。

**解决方案：** 完整的应用程序名为*FileSystem*。请访问以下链接查看UML：https://github.com/PacktPublishing/The-Complete-Coding-Interview-Guide-in-Java/blob/master/Chapter06/FileSystem/FileSystemUML.png

## 示例 8：元组

**Amazon, Google**

**问题：** 设计一个元组数据结构。

**需要询问：** 元组可以有 1 到 *n* 个元素。那么，你期望的是什么样的元组？元组中应存储哪些数据类型？

**面试官：** 我期望的是一个包含两个泛型元素的元组。元组也称为*对*。

**解决方案：** 完整的应用程序名为*Tuple*。

## 示例 9：带有电影票预订系统的电影院

**Amazon, Google, Adobe, Microsoft**

**问题：** 设计一个带有电影票预订系统的电影院。

**需要询问：** 电影院的主要结构是什么？它是否有多个影厅？我们有哪几种类型的门票？如何播放电影（只在某个影厅播放，每天只播放一次）？

**面试官：** 我期望的是一个拥有多个相同影厅的电影院。一部电影可以同时在多个影厅放映，也可以在同一天在同一个影厅多次放映。根据座位类型，有三种门票：普通票、银票和金票。电影可以非常灵活地添加/移除（例如，我们可以从某些影厅的特定开始时间移除一部电影，也可以向所有影厅添加一部电影）。

**解决方案：** 完整的应用程序名为*MovieTicketBooking*。请访问以下链接查看UML：https://github.com/PacktPublishing/The-Complete-Coding-Interview-Guide-in-Java/blob/master/Chapter06/MovieTicketBooking/MovieTicketBookingUML.png

## 示例 10：循环字节缓冲区

**Amazon, Google, Adobe**

**问题：** 设计一个循环字节缓冲区。

**需要询问：** 它应该可调整大小吗？

**面试官：** 是的，它应该可调整大小。主要的是，我希望你设计所有你认为必要的方法的签名。

**解决方案：** 完整的应用程序名为*CircularByteBuffer*。

到目前为止还不错！我建议你也尝试自己设计前面这 10 个问题。不要认为所提供的解决方案是唯一正确的。尽可能多地通过改变问题的情境来练习，并挑战其他问题。

本章的源代码包以*Chapter06*的名称提供。

## 总结

本章涵盖了关于面向对象编程（OOP）基础的最热门问题，以及 10 个在面试中非常流行的设计编码挑战。在第一部分，我们从 OOP 概念（对象、类、抽象、封装、继承、多态、关联、聚合和组合）开始，接着讨论了 SOLID 原则，最后以结合 OOP 概念、SOLID 原则和设计模式知识的综合性问题结束。在第二部分，我们应对了 10 个精心设计的设计编码挑战，包括设计点唱机、自动售货机和著名的哈希表。

练习这些问题和难题将使你具备解决面试中遇到的任何 OOP 问题的能力。

在下一章中，我们将讨论大 O 表示法和时间复杂度。

