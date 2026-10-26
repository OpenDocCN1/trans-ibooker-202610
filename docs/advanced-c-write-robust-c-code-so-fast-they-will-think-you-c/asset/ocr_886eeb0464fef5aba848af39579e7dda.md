```
std::cout << "Handler& operator=(const Handler&)" << std::endl;
    return *this;
}
Handler(Handler && obj)
{
    std::cout << "Handler(Handler && obj)" << std::endl;
}
Handler & operator=(Handler && obj)
{
    std::cout << "Handler & operator=(Handler && obj)" << std::endl;
    return *this;
}
~Handler()
{
    std::cout << "~Handler()" << std::endl;
}
};
```

3. Implement the doSomeJob() function, which actually does nothing here and just prints a log message:

```
void doSomeJob(Handler&& h)
{
    std::cout << "I'm here" << std::endl;
}
```

4. Enter the main() function and create a handler variable of the Handler type. Create thr1, pass the doSomeJob() function, and move the handler variable:

```
Handler handler;
std::thread thr1(doSomeJob, std::move(handler));
```

5. Detach the thr1 thread and add a small sleep for the main thread to avoid closing the application. We will be able to see the output from the detached thread:

```
thr1.detach();
using namespace std::chrono_literals;
std::this_thread::sleep_for(5s);
```

6. Run this code in your editor. In the terminal log, from the default constructor, you will see two logs from the move operator, one log from a destructor, a message from the doSomeJob() function, and, finally, two other log messages from the destructor. We can see that the move constructor is called twice.