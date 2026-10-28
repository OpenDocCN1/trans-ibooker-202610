
The solution consists of getting the people to their respective floors in descending order. Let's tackle this scenario based on the following image:

![](img/6709a71136a6b327c831c6c15aff2f6a_687_0.png)

Let's traverse the steps of this scenario:

- 1. This is the initial state. The elevator is on the ground floor and five people are ready to take it. Let's consider that the minimum time is 0 (so, 0 units of time).
- 2. In the elevator, we take the people who are going to the 4th floor and the one person who is going to the 2nd floor. Remember that we can take a maximum of three people at a time. So far, the minimum time is 0.
- 3. The elevator goes up and stops at the 2nd floor. One person gets off. Since each floor represents a unit of time, we have a minimum time of 2.
- 4. The elevator goes up and stops at the 4th floor. The remaining two people get off. The minimum time becomes equal to 4.
- 5. At this step, the elevator is empty. It must go down to the ground floor to pick up more people. Since it goes down four floors, the minimum time becomes 8.
- 6. We pick up the remaining two people. The minimum time remains as 8.
- 7. The elevator goes up and stops at the 1st floor. One person gets off. The minimum time becomes 9.
- 8. The elevator goes up and stops at the 2nd floor. One person gets off. The minimum time becomes 10.
- 9. At this step, the elevator is empty. It must go down to the ground floor. Since it goes down two floors, the minimum time becomes 12.

So, the total minimum time is 12. Based on this scenario, we can elaborate the following algorithm:

- 1. Sort the given array in descending order of destinations.
- 2. Create groups of k persons. The time needed for each group will be 2 * floors[group].

So, sorting our testing data will result in floors = {4, 4, 2, 2, 1}. We have two groups. One group contains three people (4, 4, 2), while the other group contains two people (2, 1). The total minimum time is (2 * floors[0]) + (2 * floors[3]) = (2 * 4) + (2 * 2) = 8 + 4 = 12.

In terms of code, we have the following:

```
public static int time(int k, int floors[]) {

    int aux;
    for (int i = 0; i < floors.length - 1; i++) {
        for (int j = i + 1; j < floors.length; j++) {
            if (floors[i] < floors[j]) {
                aux = floors[i];
                floors[i] = floors[j];
                floors[j] = aux;
            }
        }
    }

    // iterate the groups and update
    // the time needed for each group
    int time = 0;
    for (int i = 0; i < floors.length; i += k) {
        time += (2 * floors[i]);
    }

    return time;
}
```

Of course, you may end up choosing a better sorting algorithm. The complete application is called ScheduleOneElevator. This was the last coding challenge of this chapter.

## Scheduling multiple elevators

But how do we schedule multiple elevators with an arbitrary number of floors? Well, most probably, in an interview, you won't need to implement a solution for more than one elevator, but you could be asked how you'd design a solution for more.

The problem of scheduling multiple elevators and the algorithm are famous and difficult. There is no best algorithm for this problem. In other words, creating an algorithm that can be applied to the real-world scheduling of elevators is really difficult, and apparently, it is patented.

The Elevator algorithm (https://en.wikipedia.org/wiki/Elevator_algorithm) is a good place to start. Before thinking about how to design a solution for multi-elevators, you must make a list of all the assumptions or constraints that you want to consider. Every available solution/algorithm has a list of assumptions or constraints referring to the number of floors, number of elevators, capacity of each elevator, number of average people, rush hour times, elevator speed, load and unload times, and so on. Mainly, there are three solutions, as follows:

- **Sectors:** Each elevator is allocated to a sector (it services a subset of floors).
- **Nearest elevator:** Each person is assigned to the nearest elevator (it does this based on the elevator's position, the direction of call, and the current direction of the elevator).
- **Nearest elevator with capacity considerations:** This is similar to the nearest elevator option, but it takes the load in each elevator into account.

### Sectors

For example, a building that has eight floors and three elevators can be serviced like this:

- Elevator 1 serves floors 1, 2, and 3.
- Elevator 2 serves floors 1, 4, and 5.
- Elevator 3 serves floors 1, 6, 7, and 8.

Each elevator services the ground floor since the ground floor has the highest arrival rate.

### Nearest elevator

Allocate a score to each elevator. This score represents the suitability score for an elevator when a new person arrives:

- Toward the call, same direction: FS = (N + 2) - d
- Toward the call, opposite direction: FS = (N + 1) - d
- Away from the call: FS = 1

Where, N = #Floors – 1, and d = distance between elevator and call.

### Nearest elevator with capacity considerations

This is exactly the same as the nearest elevator situation, but it takes into account the excess capacity of the elevator:

- Toward the call, same direction: FS = (N + 2) - d + C
- Toward the call, opposite direction: FS = (N + 1) - d + C
- Away from the call: FS = 1 + C

Here, N is #Floors – 1, d is the distance between the elevator and the call, and C is the excess capacity.

I strongly recommend that you search and study different implementations for this problem and try to learn the one that you find the most suitable for you. I suggest that you start from here:

- https://github.com/topics/elevator-simulation
- https://austingwalters.com/everyday-algorithms-elevator-allocation/.

Now, let's summarize this chapter.

## Summary

In this chapter, we covered the most popular problems that fit into the mathematics and puzzles categories. While many companies avoid such problems, there are still major players such as Google and Amazon that rely on these kinds of problems in their interviews.

Practicing such problems represents a good exercise for our brains. Besides the math knowledge, these problems sustain analytical thinking based on deductions and intuition, which means they are great support for any programmer.

In the next chapter, we'll tackle a hot topic in interviews: concurrency (multithreading).

## Section 4: Bonus – Concurrency and Functional Programming

Companies are very sensitive to topics such as concurrency and functional programming. This chapter covers the most popular questions surrounding these two topics. These four chapters are bonus chapters; the approach of which is not similar to the chapters you've read so far. Owing to the nature of the topics, we will touch upon them briefly, elaborating on the questions asked in interviews on the respective topics. You can find the codes used in the chapter on the GitHub repository, link to which is shared in the technical requirements section of the chapter.

- Chapter 16, Concurrency
- Chapter 17, Functional-Style Programming
- Chapter 18, Unit Testing
- Chapter 19, System Scalability

### 16 Concurrency

Developing single-threaded Java applications is rarely feasible. Therefore, most of your projects will be multithreaded (that is, they will run in a multithreaded environment). This means that, sooner or later, you'll have to tackle certain multithreading problems. In other words, at some point, you'll have to get your hands dirty with code that manipulates Java threads directly or via dedicated APIs.

This chapter covers the most popular questions about Java concurrency (multithreading) that occur in general interviews about the Java language. As usual, we will start with a brief introduction that covers the main aspects of Java concurrency. Therefore, our agenda is straightforward, covering the following topics:

- Java concurrency (multithreading) in a nutshell
- Questions and coding challenges

Let's begin with the fundamental knowledge of our topic, Java concurrency. Use the following nutshell section to extract answers to some basic questions about concurrency, such as What is concurrency?, What is a Java thread?, What is multithreading?, and more.
