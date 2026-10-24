# A Java Performance Toolbox

Performance analysis is all about visibility—knowing what is going on inside an application and in the application’s environment. Visibility is all about tools. And so performance tuning is all about tools.

In Chapter 2, we looked at the importance of taking a data-driven approach to performance: you must measure the application’s performance and understand what those measurements mean. Performance analysis must be similarly data-driven: you must have data about what, exactly, the program is doing in order to make it perform better. How to obtain and understand that data is the subject of this chapter.

Hundreds of tools can provide information about what a Java application is doing, and looking at all of them would be impractical. Many of the most important tools come with the Java Development Kit (JDK), and although those tools have other open source and commercial competitors, this chapter focuses mostly on the JDK tools as a matter of experience.

# Operating System Tools and Analysis

The starting point for program analysis is not Java-specific at all: it is the basic set of monitoring tools that come with the operating system. On Unix-based systems, these are sar (System Accounting Report) and its constituent tools like vmstat, iostat, prstat, and so on. Windows has graphical resource monitors as well as command-line utilities like typeperf.

Whenever performance tests are run, data should be gathered from the operating system. At a minimum, information on CPU, memory, and disk usage should be collected; if the program uses the network, information on network usage should be gathered as well. If performance tests are automated, this means relying on command-line tools (even on Windows). But even if tests are running interactively, it