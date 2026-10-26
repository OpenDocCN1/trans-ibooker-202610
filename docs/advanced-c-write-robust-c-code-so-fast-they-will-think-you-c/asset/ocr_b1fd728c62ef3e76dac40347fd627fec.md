```
}
}
    result[idx] = bufferOut;
}
catch(...)
{
    std::cout << "Invalid index" << std::endl;
}
}

private:
const std::vector<std::string>& m_bufferIn;
};
```

3. Enter the main() function, create a constant value called numberOfTasks, and set it to 5. Then, create a vector of a Converter object and reserve its size with numberOfTasks. Then, create a vector of std::thread objects and reserve its size with numberOfTasks:

```
const int numberOfTasks = 5;
std::vector<Converter> functions;
functions.reserve(numberOfTasks);
std::vector<std::thread> threads;
threads.reserve(numberOfTasks);
```

4. Create the vector of strings, textArr, and push five different big strings to be converted:

```
std::vector<std::string> textArr;
textArr.emplace_back("In the previous topics, we learned almost all that we need to work with threads. But we still have something interesting to consider - how to synchronize threads using future results. When we considered condition variables we didn't cover the second type of synchronization with future results. Now it's time to learn that.");
textArr.emplace_back("First of all, let's consider a real-life example. Imagine, you just passed the exam at the university. You were asked to wait some amount of time for results. So, you have time to coffee with your mates, and every 10-15 mins you check are results available. Then, when you finished all your other activities, you just come to the door of the lecture room and wait for results.");
textArr.emplace_back("In this exercise, we will write a simple application where we will use std::move() with threads. First of all, we will implement a class that is move constructible. This class will convert lowercase text into uppercase text. Then we will create a vector of
```