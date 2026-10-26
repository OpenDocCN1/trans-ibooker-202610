+   8. Implement a third **for** loop where we detach `std::threads`:
+   
+   ```cpp
+   for (auto iter = threads.begin(); iter != threads.end(); ++iter)
+   {
+       (*iter).detach();
+   }
+   ```

+   9. Add a small sleep for the main thread to avoid closing the application. Now, we can see how detached threads work:
+   
+   ```cpp
+   using namespace std::chrono_literals;
+   std::this_thread::sleep_for(5s);
+   ```

+   10. Finally print the result into the terminal:
+   
+   ```cpp
+   for (const auto& str : result)
+   {
+       std::cout << str;
+   }
+   ```

+   11. Run this code in your editor. In the terminal, you can see that all strings are in uppercase, which means that all threads were moved and run successfully. You will get the following output:

![](bbox=[0.133, 0.506, 0.932, 0.841])

**Figure 5.18:** The result of the exercise's execution