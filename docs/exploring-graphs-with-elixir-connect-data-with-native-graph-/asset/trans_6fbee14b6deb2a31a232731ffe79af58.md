

收集器到每个桶中。换句话说，收集到桶中的值可以在下游经过更多的分类和收集，如我们接下来将看到的。

继续上一个示例，我们不再按年龄创建所有人员对象的映射，而是只获取按年龄分组的人员姓名。

```
compare/fpij/OlderThan20.java

Map<Integer, List<String>> nameOfPeopleByAge =
people.stream()
.collect(
groupingBy(Person::getAge, mapping(Person::getName, toList())));
System.out.println("People grouped by age: " + nameOfPeopleByAge);
```

在此版本中，groupingBy 接受两个参数：第一个是年龄，即用于分组的条件；第二个是一个 Collector，它是对 mapping 函数调用的结果。这些方法来自 Collectors 工具类，并在代码中静态导入以供使用。mapping 方法接受两个细节：要映射的属性（本例中为 name）以及要收集到的对象类型，例如列表或集合。让我们看看这段代码的输出：

```
People grouped by age: {35=[Greg], 20=[John], 21=[Sara, Jane]}
```

我们可以看到，姓名列表已按年龄分组。

让我们再看一个组合。让我们按姓名的首字符对姓名进行分组，然后获取每个组中年龄最大的人员。

```
compare/fpij/OlderThan20.java

Comparator<Person> byAge = Comparator.comparing(Person::getAge);
Map<Character, Optional<Person>> oldestPersonOfEachLetter =
people.stream()
.collect(groupingBy(person -> person.getName().charAt(0),
        reducing(BinaryOperator.maxBy(byAge))));
System.out.println("Oldest person of each letter:");
System.out.println(oldestPersonOfEachLetter);
```

我们首先根据姓名的首字母对姓名进行分组。为此，我们将 lambda 表达式作为 groupingBy 方法的第一个参数传递。在该 lambda 表达式内部，我们返回姓名的首字符以用于分组。本例中的第二个参数不是 mapping，而是执行一个归约操作。在每个组中，它将元素归约为年龄最大的人员，由 maxBy 方法决定。由于操作组合，语法有点紧凑，但它的意思是：按姓名首字符分组，并归约为年龄最大的人员。让我们看看输出，它列出了以给定字母开头的每个姓名分组中年龄最大的人员。

```
Oldest person of each letter:
{S=Optional[Sara - 21], G=Optional[Greg - 35], J=Optional[Jane - 21]}
```

我们已经领略了 collect 方法和 Collectors 的强大功能。请花几分钟时间在集成开发环境或文档中查看 Collectors 工具类，熟悉其提供的功能。接下来，我们将使用 lambda 表达式来代替一些过滤器。

## 列出目录中的所有文件

使用 File 类的 list 方法列出目录中的所有文件名非常简单。要获取所有文件而不仅仅是文件名，可以使用 listFiles 方法。这很容易，但挑战在于拿到列表后该如何处理。与其使用冗长的传统外部迭代器，不如使用优雅的函数式设施来遍历列表。为了实现这一点，我们需要调用 JDK 的 CloseableStream 接口，以及一些相关的高阶便捷函数。

下面是列出当前目录中所有文件名的代码。

```
compare/fpij/ListFiles.java

Files.list(Paths.get("."))
.forEach(System.out::println);
```

要列出不同目录中的文件，可以将 "." 替换为所需目录的完整路径。

我们首先使用 Paths 便捷类的 get 方法从字符串创建了一个 Path 实例。然后，使用 Files 工具类（在 java.nio.file 包中）的 list 方法，我们得到了一个 CloseableStream 来遍历给定路径中的文件。接着，我们在其上使用了内部迭代器 forEach 来打印文件名。

让我们看看这段代码输出的一部分：列出当前目录的文件和子目录。

```
./aSampleFiles.txt
./bin
./fpij
...
```

如果我们只想获取当前目录中的子目录，而不是列出所有文件，可以使用 filter 方法：

```
compare/fpij/ListDirs.java

Files.list(Paths.get("."))
.filter(Files::isDirectory)
.forEach(System.out::println);
```

filter 方法从文件流中仅提取了目录。我们没有传入 lambda 表达式，而是提供了对 Files 类 isDirectory 方法的方法引用。请回顾，filter 方法期望一个 Predicate，它返回布尔结果，因此此方法很合适。最后，我们使用内部迭代器打印目录名称。这段代码的输出将显示当前目录的子目录。

```
./.bin
./fpij
./output
...
```

这很简单，代码行数比旧式 Java 代码要少。接下来，让我们看看只列出匹配模式的特定文件。

## 列出目录中的特定文件

Java 长期以来一直提供 list 方法的一个变体来挑选文件名。这个版本的 list 接受一个 FilenameFilter 作为其参数。这个接口有一个名为 accept 的方法，它接受两个参数：File dir（表示目录）和 String name（表示文件名）。我们会从 accept 方法返回 true 以将给定文件名包含在列表中，否则返回 false。让我们探索实现此方法的选项。

在 Java 中，向 list 方法传递实现 FilenameFilter 接口的匿名内部类实例是一种惯用做法。例如，让我们看看如何使用这种方法仅选择 fpij 目录中的 java 文件。

```
compare/fpij/ListSelectFiles.java

final String[] files =
    new File("fpij").list(new java.io.FilenameFilter() {
        public boolean accept(final File dir, final String name) {
            return name.endsWith(".java");
        }
    });
if(files != null) {
    for(String file: files) {
        System.out.println(file);
    }
}
```

这花费了一些精力和几行代码。那段代码中有很多冗余：对象创建、函数调用、匿名内部类定义、类中的嵌入方法等等。我们不再需要忍受这种痛苦；我们可以简单地传递一个接受两个参数并返回布尔结果的 lambda 表达式。Java 编译器可以为我们处理其余部分。

虽然我们可以在前一个示例中简单地用 lambda 表达式替换匿名内部类，但我们可以做得更好。DirectoryStream 设施可以更高效地遍历大型目录结构，所以让我们探索这条路径。newDirectoryStream 方法有一个变体，它接受额外的 filter 参数。

让我们使用 lambda 表达式获取 fpij 目录中所有 java 文件的列表。

```
compare/fpij/ListSelectFiles.java

Files.newDirectoryStream(
    Paths.get("fpij"), path -> path.toString().endsWith(".java"))
    .forEach(System.out::println);
```

我们摆脱了匿名内部类，将冗长的代码变成了简洁的代码。两个版本的净效果相同。让我们打印选定的文件。

该代码将仅显示提及目录中的 .java 文件，如下面的部分输出所示：

```
fpij/Compare.java
fpij/IterateString.java
fpij/ListDirs.java
...
```

我们根据文件名选择了文件，但我们也可以轻松地根据文件属性选择文件，例如文件是否可执行、可读或可写。为此，我们需要 listFiles 方法的一个变体，它接受 FileFilter 作为其参数。再次，我们可以使用 lambda 表达式而不是创建匿名内部类。让我们看一个列出当前目录中所有隐藏文件的示例。

```
compare/fpij/ListHiddenFiles.java

final File[] files = new File(".").listFiles(file -> file.isHidden());
```

如果我们正在处理大型目录，则可以使用 DirectoryStream，而不是直接使用 File 上的方法。

我们传递给 listFiles 方法的 lambda 表达式的签名符合 FileFilter 接口 accept 方法的签名。在 lambda 表达式中，我们接收一个名为 file 的 File 实例作为参数。如果文件具有隐藏属性，则返回布尔值 true，否则返回 false。

我们可以进一步精简这里的代码；与其传递 lambda 表达式，不如使用方法引用来使代码更简洁：

```
compare/fpij/ListHiddenFiles.java

new File(".").listFiles(File::isHidden);
```

我们创建了 lambda 表达式版本，然后将其重构为更简洁的方法引用版本。在编写新代码时，采用这条路径完全可以。如果我们能一眼看出简洁的代码，那么当然可以直接输入它。在“先使其运行，再使其更好”的精神下，先让简单的东西运行起来是很好的；一旦我们理解了代码，就可以进一步重构以提高简洁性、性能等等。

我们通过一个示例过滤出目录中的特定文件。接下来，我们将看看如何探索子目录的 a

