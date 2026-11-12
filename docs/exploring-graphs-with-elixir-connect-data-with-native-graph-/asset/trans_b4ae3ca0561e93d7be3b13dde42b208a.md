

假设我们向业务方澄清需求，他们建议成功时打印名称，失败时打印错误详情；我们可以针对这一点进行设计。但是，如果他们改变主意，我们希望设计能够适应这种变化，且对代码的改动尽可能小。出于这个原因，发生错误的地方通常不是处理错误的正确位置。我们的大部分代码可能需要将结果和失败传播给调用代码，并让边缘代码根据要求处理结果的显示或错误的恰当处理。

接下来，我们将编写一个小程序，使用命令式风格的代码来处理异常。

## 异常处理与命令式风格

您的用户不关心您使用什么编程风格。无论您使用命令式风格还是函数式风格实现代码，结果都应该相同。无论您选择哪种范式，程序都必须优雅地处理失败。在本节中，我们将从命令式风格代码入手，直接深入到处理失败的核心。一旦我们看到代码如何处理异常以及程序的输出，我们将探讨将该代码转换为函数式风格的选项，并验证输出是否与命令式风格代码的输出一致。

我们将沿用上一节的要求：给定机场 IATA 代码列表，检索并打印机场名称，并打印找不到的名称的错误详情。让我们先从给定 IATA 代码时检索机场名称的代码开始：

exceptionhandling/fpij/AirportInfo.java

```java
package fpij;

import java.util.Scanner;
import java.net.URL;
import java.io.IOException;

public class AirportInfo {
public static String getNameOfAirport(String iata)
throws IOException, AirportInfoException {

var url = "https://soa.smext.faa.gov/asws/api/airport/status/" + iata;

try(var scanner = new Scanner(new URL(url).openStream())) {
var response = scanner.nextLine();

if(!response.contains("Name")) {
    throw new AirportInfoException("Invalid airport code " + iata);
}

return response.split("/")[3]; //a bruteforce way to get the Name
}
}
}
```

`getNameOfAirport` 方法获取机场的 IATA 代码，与 Web 服务通信以获取该机场的信息，并从响应中提取机场名称。由于代码的特性——可能会出现访问远程 Web 服务失败或服务无法识别给定代码的情况——失败是不可避免的。

`getNameOfAirport` 方法不是处理任何错误的正确位置。如果服务调用失败，我们仅将 `IOException` 从 URL 传播到 `getNameOfAirport` 的调用方。另一方面，如果服务调用成功但我们没有得到想要的机场详细信息，`getNameOfAirport` 方法将抛出一个特定于领域的受检异常 `AirportInfoException`，如下所示：

exceptionhandling/fpij/AirportInfoException.java

```java
package fpij;

public class AirportInfoException extends Exception {
    public AirportInfoException(String message) {
        super(message);
    }

    public AirportInfoException(Throwable cause) {
        super(cause);
    }

    public AirportInfoException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

现在我们有了给定 IATA 代码时获取机场名称的代码，让我们关注下一个更高级别的函数。我们想要获取给定 IATA 代码列表中所有机场的名称。让我们先看看如何用命令式风格编写代码——之后，我们将看到如何用函数式风格做同样的事情。

`getNamesOfAirports` 方法获取机场列表，调用 `AirportInfo` 类的 `getNameOfAirport` 方法，并返回名称列表。这可以处理正常路径，但我们还需要考虑失败的可能性。程序员通常倾向于在发生失败时抛出或传播异常。如果我们有“一旦失败就停止”的要求，这可能有效，但当用户希望看到成功和失败的结果时，这并不是一个解决方案。我们需要捕获成功结果和失败详情，并将它们传递给调用方。

以下是新 `AirportNames` 类中 `getNamesOfAirports` 方法的实现：

exceptionhandling/fpij/AirportNames.java

```java
public class AirportNames {
public static List<DataOrException<String>> getNamesOfAirports(
List<String> iataCodes) {
List<DataOrException<String>> result = new ArrayList<>();
for(var iataCode: iataCodes) {
try {
result.add(DataOrException.of(
AirportInfo.getNameOfAirport(iataCode).toUpperCase())));
} catch(IOException | AirportInfoException ex) {
result.add(DataOrException.of(ex));
}
}
return result;
}
}
```

在上述代码中，DataOrException 作为结果的包装器——成功时是机场名称的字符串，失败时是异常。AirportNames 的 getNamesOfAirports 命令式地遍历 IATA 代码列表，并使用我们之前编写的 getNameOfAirport 方法从服务中获取机场名称。如果调用成功，该方法将获取的名称转换为大写，将其包装为 DataOrException 的实例，并添加到结果列表中。如果调用失败，该方法则在 catch 块中将失败详情包装为 DataOrException 的实例并添加到结果列表中。尽管命令式风格较为冗长，但在处理异常方面非常出色，我们在此充分利用了这一点。让我们快速看看用于包装数据或异常的 DataOrException 接口：

exceptionhandling/fpij/DataOrException.java

```java
package fpij;

public sealed interface DataOrException<T> {
public default boolean isDataPresent() { return true; }

public default T getDataOrThrow() {
throw new RuntimeException("No data");
}

public default Exception getExceptionOrThrow() {
throw new RuntimeException("No Exception");
}

public static <T> DataOrException<T> of(T data) {
return new Data<T>(data);
}

public static <T> DataOrException<T> of(Exception exception) {
    return new TheException<T>(exception);
}

record Data<T>(T data) implements DataOrException<T> {
    public T getDataOrThrow() { return data; }
}

record TheException<T>(Exception exception) implements DataOrException<T> {
    public boolean isDataPresent() { return false; }

    public Exception getExceptionOrThrow() { return exception; }
}
}
```

DataOrException 的第一个重载静态方法返回一个携带给定数据的包装器实例。该方法的第二个重载版本返回一个携带异常的包装器实例。使用此机制，我们可以方便地在函数之间传输数据或异常。

让我们从 main 方法中调用 getNamesOfAirports 方法，提供几个我最喜欢的得克萨斯州（Lone Star state）机场的 IATA 代码：

![](img/f9269002f2414fff96e5e6b4dc20839f_298_0.png)

```java
public static void main(String[] args) {
    var iataCodes = List.of("AUS", "DFW", "HOU", "IHA", "SAT");

    for(var result: getNamesOfAirports(iataCodes)) {
        if(result.isDataPresent()) {
            System.out.println(result.getDataOrThrow());
        } else {
            System.out.println("Error: " +
                result.getExceptionOrThrow().getMessage());
        }
    }
}
```

让我们运行这段代码，看看它是如何工作的。

```
AUSTIN-BERGSTROM INTL
DALLAS-FORT WORTH INTL
WILLIAM P HOBBY
Error: Invalid airport code IHA
SAN ANTONIO INTL
```

输出显示了有效 IATA 代码对应的机场名称。我本想输入 IAH，却误输入了 IHA，结果发现没有该代码的机场。别担心，代码优雅地处理了这一情况，礼貌地告诉我们发生了什么，然后继续显示列表中下一个机场的名称。

作为边缘函数的 main 方法可以根据要求决定如何处理异常。它可以根据需求的变化而演进：仅打印成功的名称、仅打印错误、将它们分组等。从设计角度来看，这个解决方案更容易扩展。例如，将来如果一个 getNamesOfAirports 调用方想要所有结果和失败，而另一个调用方希望在第一个异常时快速失败，我们可以修改 getNamesOfAirports，使其根据调用方的偏好（可以作为参数传递）返回完整的成功/失败列表或仅返回第一个失败。

前面的代码是命令式风格，但您可能会问，函数式风格呢？我们知道与命令式风格相比，函数式风格代码更优雅，但它在处理错误时也应该优雅。让我们看看将代码重构为函数式风格的努力接下来会如何。

