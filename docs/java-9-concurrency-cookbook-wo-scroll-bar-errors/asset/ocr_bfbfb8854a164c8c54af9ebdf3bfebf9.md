In this recipe, you will learn how to use this tool to analyze your Java-concurrent application.

# Getting ready

Before you start this recipe, download FindBugs from the project's web page (http://findbugs.sourceforge.net/). You can download a standalone application or an Eclipse plugin. In this recipe, I used the standalone version.

At the time of this writing, the actual version of FindBugs (3.0.1) doesn't include support for Java 9. You can download a preview of the 3.1.0 version with support for Java 9 from https://github.com/findbugsproject/findbugs/releases/tag/3.1.0_preview1.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named Task that extends the Runnable interface:
+   ```
+   public class Task implements Runnable {
+   ```
+   2. Declare a private ReentrantLock attribute named lock:
+   ```
+   private ReentrantLock lock;
+   ```
+   3. Implement a constructor of the class:
+   ```
+   public Task(ReentrantLock lock) {
+       this.lock=lock;
+   }
+   ```