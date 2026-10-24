operation, you can see over time which methods are being compiled. In this example, jstat repeats the information for process ID 5003 every second (1,000 ms):

```
% jstat -printcompilation 5003 1000
Compiled  Size  Type  Method
207       64    1     java/lang/CharacterDataLatin1 toUpperCase
208       5     1     java/math/BigDecimal$StringBuilderHelper getCharArray
```

The compilation log may also include a line that looks like this:

```
timestamp compile_id COMPILE SKIPPED: reason
```

This line (with the literal text COMPILE SKIPPED) indicates that something has gone wrong with the compilation of the given method. In two cases this is expected, depending on the reason specified:

# Code cache filled

The size of the code cache needs to be increased using the ReservedCodeCache flag.

# Concurrent classloading

The class was modified as it was being compiled. The JVM will compile it again later; you should expect to see the method recompiled later in the log.

In all cases (except the cache being filled), the compilation should be reattempted. If it is not, an error prevents compilation of the code. This is often a bug in the compiler, but the usual remedy in all cases is to refactor the code into something simpler that the compiler can handle.

Here are a few lines of output from enabling PrintCompilation on the stock REST application:

```
28015  850     4   net.sdo.StockPrice::getClosingPrice (5 bytes)
28179  905  s   3   net.sdo.StockPriceHistoryImpl::process (248 bytes)
28226  25 %  3   net.sdo.StockPriceHistoryImpl::<init> @ 48 (156 bytes)
28244  935     3   net.sdo.MockStockPriceEntityManagerFactory\MockStockPriceEntityManager::find (507 bytes)
29929  939     3   net.sdo.StockPriceHistoryImpl::<init> (156 bytes)
106805 1568  !  4   net.sdo.StockServlet::processRequest (197 bytes)
```

This output includes only a few of the stock-related methods (and not necessarily all of the lines related to a particular method). A few interesting things to note: the first such method wasn’t compiled until 28 seconds after the server was started, and 849 methods were compiled before it. In this case, all those other methods were methods of the server or JDK (filtered out of this output). The server took about 2 seconds to start; the remaining 26 seconds before anything else was compiled were essentially idle as the application server waited for requests.