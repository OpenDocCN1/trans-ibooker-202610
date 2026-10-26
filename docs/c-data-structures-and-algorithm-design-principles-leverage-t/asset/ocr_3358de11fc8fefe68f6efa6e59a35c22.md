Assume that all the names are positive integers and that the given list is sorted. Your task is to write a program that can look up the vaccination status of a given student in the list and outputs to the officials whether the student needs to be vaccinated.

Students need to be vaccinated in case of two conditions:

+   - If they are not present in the list
+   - If they are present in the list but have not been administered a flu shot

Since the list can have a large number of students, your program should be as fast and efficient as possible. The final output of your program should look as follows:

![](bbox=[0.158, 0.306, 0.844, 0.467])

Figure 4.8: Sample output of Activity 8

# High-level Steps

The solution to this activity uses a slightly modified version of the binary search algorithm. Let's get started:

+   1. Represent each student as an object of the **Student** class, which can be defined as follows:

```
class Student
{
    std::pair<int, int> name;

    bool vaccinated;
}
```

+   2. Overload the required operators for the **Student** class so that a vector of students can be sorted using the Standard Library's `std::sort()` function.
+   
+   3. Use a binary search to see if the student is present on the list.
+   
+   4. If the student isn't present in the list, your function should return true since the student needs to be administered the vaccine.