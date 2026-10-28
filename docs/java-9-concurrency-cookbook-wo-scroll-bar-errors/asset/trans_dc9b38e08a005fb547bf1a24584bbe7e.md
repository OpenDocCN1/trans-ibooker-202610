

最后，像使用 Executor 框架一样，你应该使用 shutdown() 方法来关闭 ForkJoinPool。

下面的截图显示了此示例执行的一部分：

![](img/cc06c59cd21f558f43e9475d7f212170_230_0.png)

你可以看到任务完成其工作以及产品的价格已更新。

## 还有更多...

ForkJoinPool 类提供了其他方法来执行任务。这些方法如下：

- execute (Runnable task)：这是示例中使用的 execute() 方法的另一个版本。在这种情况下，你将一个 Runnable 任务发送给 ForkJoinPool 类。请注意，ForkJoinPool 类不会对 Runnable 对象使用工作窃取算法。它仅与 ForkJoinTask 对象一起使用。
- invoke(ForkJoinTask<T> task)：虽然 execute() 方法像示例中学习的那样对 ForkJoinPool 类进行异步调用，但 invoke() 方法对 ForkJoinPool 类进行同步调用。此调用不会在作为参数传递的任务完成执行之前返回。
- 你也可以使用 ExecutorService 接口中声明的 invokeAll() 和 invokeAny() 方法。这些方法接收 Callable 对象作为参数。ForkJoinPool 类不会对 Callable 对象使用工作窃取算法，因此最好使用 ThreadPoolExecutor 来执行它们。

ForkJoinTask 类还包括示例中使用的 invokeAll() 方法的其他版本。这些版本如下：

- invokeAll(ForkJoinTask<?>... tasks)：此方法版本使用可变参数列表。你可以向其传递任意数量的 ForkJoinTask 对象作为参数。
- invokeAll(Collection<T> tasks)：此方法版本接受一个集合（例如 ArrayList 对象、LinkedList 对象或 TreeSet 对象）作为参数，其中包含泛型类型 T 的对象。此泛型类型 T 必须是 ForkJoinTask 类或其子类。

尽管 ForkJoinPool 类设计用于执行 ForkJoinTask 对象，你也可以直接执行 Runnable 和 Callable 对象。你也可以使用 ForkJoinTask 类的 adapt() 方法，该方法接受一个 Callable 对象或一个 Runnable 对象并返回一个 ForkJoinTask 对象来执行该任务。

## 另请参阅

- 第 9 章《Testing Concurrent Applications》中的 Monitoring a fork/join pool 配方

## 合并任务的结果

fork/join 框架提供了执行返回结果的任务的能力。这种类型的任务由 RecursiveTask 类实现。此类扩展了 ForkJoinTask 类并实现了 Executor 框架提供的 Future 接口。

在任务内部，你必须使用 Java API 文档推荐的结构：

```
if (problem size > size) {
    tasks=Divide(task);
    execute(tasks);
    joinResults()
    return result;
} else {
    resolve problem;
    return result;
}
```

如果任务必须解决的问题大于预定义的大小，你将问题划分为更多子任务并使用 fork/join 框架执行这些子任务。当它们完成执行后，发起任务获取所有子任务生成的结果，对其进行分组并返回最终结果。最终，当在池中执行的发起任务完成执行时，你将获得其结果，这实际上是整个问题的最终结果。

在本配方中，你将通过开发一个在文档中查找单词的应用程序来学习如何使用 fork/join 框架进行这种问题解决。你将实现以下两种类型的任务：

- 文档任务，它将在文档的一组行中搜索单词
- 行任务，它将在文档的一部分中搜索单词

所有任务都将返回单词在它们处理的文档部分或行中出现的次数。在本配方中，我们将使用 Java Concurrency API 提供的默认 fork/join 池。

## 如何操作...

按照以下步骤实现示例：

1. 创建一个名为 DocumentMock 的类。它将生成一个字符串矩阵来模拟文档：

```
public class DocumentMock {
```

2. 创建一个包含一些单词的字符串数组。此数组将在字符串矩阵的生成中使用：

```
private String words[]={"the","hello","goodbye","packt",
    "java","thread","pool","random",
    "class","main"};
```

3. 实现 generateDocument() 方法。它接收行数、每行单词数以及示例将要查找的单词作为参数。它返回一个字符串矩阵：

```
public String[][] generateDocument(int numLines, int numWords,
    String word){
```

4. 首先，创建生成文档所需的对象——字符串矩阵和用于生成随机数的 Random 对象：

```
int counter=0;
String document[][]=new String[numLines][numWords];
Random random=new Random();
```

5. 用字符串填充数组。将每个位置存储为单词数组中随机位置的字符串，并计算程序将在生成的数组中查找的单词出现的次数。你可以使用此值来检查程序是否正确执行其工作：

```
for (int i=0; i<numLines; i++){
    for (int j=0; j<numWords; j++) {
        int index=random.nextInt(words.length);
        document[i][j]=words[index];
        if (document[i][j].equals(word)) {
            counter++;
        }
    }
}
```

6. 写出单词出现的次数的消息并返回生成的矩阵：

```
System.out.println("DocumentMock: The word appears "+ counter+" times in the document");
return document;
```

7. 创建一个名为 DocumentTask 的类并指定它扩展参数化为 Integer 类的 RecursiveTask 类。此类将实现计算单词在一组行中出现的次数的任务：

```
public class DocumentTask extends RecursiveTask<Integer> {
```

8. 声明一个名为 document 的私有字符串矩阵和两个名为 start 和 end 的私有 int 属性。另声明一个名为 word 的私有 String 属性：

```
private String document[][];
private int start, end;
private String word;
```

9. 实现类的构造函数以初始化其所有属性：

```
public DocumentTask (String document[][], int start, int end, String word){
    this.document=document;
    this.start=start;
    this.end=end;
    this.word=word;
}
```

10. 实现 compute() 方法。如果 end 和 start 属性之间的差值小于 10，则任务通过调用 processLines() 方法计算这些位置之间的行中单词出现的次数：

```
@Override
protected Integer compute() {
    Integer result=null;
    if (end-start<10){
        result=processLines(document, start, end, word);
```

11. 否则，将行组分为两个对象，创建两个新的 DocumentTask 对象来处理这两个组，并使用 invokeAll() 方法在池中执行它们：

```
} else {
    int mid=(start+end)/2;
    DocumentTask task1=new DocumentTask(document,start,mid,word);
    DocumentTask task2=new DocumentTask(document,mid,end,word);
    invokeAll(task1,task2);
```

12. 然后，使用 groupResults() 方法将两个任务返回的值相加。最后，返回任务计算的结果：

```
try {
    result=groupResults(task1.get(),task2.get());
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
}
return result;
```

13. 实现 processLines() 方法。它接收字符串矩阵、start 属性、end 属性以及任务正在搜索的 word 属性作为参数：

```
private Integer processLines(String[][] document, int start,
    end,String word) {
```

14. 对于任务必须处理的每一行，创建一个 LineTask 对象来处理整行并将它们存储在任务列表中：

```
List<LineTask> tasks=new ArrayList<LineTask>();
for (int i=start; i<end; i++) {
    LineTask task=new LineTask(document[i], 0,
        document[i].length, word);
    tasks.add(task);
}
```

15. 使用 invokeAll() 方法执行该列表中的所有任务：

```
invokeAll(tasks);
```

16. 将所有这些任务返回的值相加并返回结果：

```
int result=0;
for (int i=0; i<tasks.size(); i++) {
    LineTask task=tasks.get(i);
    try {
        result=result+task.get();
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}
return result;
```

17. 实现 groupResults() 方法。它将两个数字相加并返回结果：

```
private Integer groupResults(Integer number1,Integer number2) {
    Integer result;
    result=number1+number2;
    return result;
}

