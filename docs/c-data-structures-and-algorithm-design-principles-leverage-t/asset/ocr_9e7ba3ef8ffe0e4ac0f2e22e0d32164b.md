```
Job j1("John", 10);
Job j2("Jerry", 4);
Job j3("Jimmy", 5);
Job j4("George", 7);
Job j5("Bill", 8);
Job j6("Kenny", 10);
printer.addNewJob(j1);
printer.addNewJob(j2);
printer.addNewJob(j3);
printer.addNewJob(j4);
printer.addNewJob(j5);

if(not printer.addNewJob(j6)) // Can't add as queue is full.
{
    std::cout << "Couldn't add 6th job" << std::endl;
}

printer.startPrinting();

printer.addNewJob(j6); // Can add now, as queue got emptied
printer.startPrinting();
}
```

6. Here is the output of the preceding code:

```
Added job in the queue: id: 1, user: John, time: 10 seconds
Added job in the queue: id: 2, user: Jerry, time: 4 seconds
Added job in the queue: id: 3, user: Jimmy, time: 5 seconds
Added job in the queue: id: 4, user: George, time: 7 seconds
Added job in the queue: id: 5, user: Bill, time: 8 seconds
Couldn't add 6th job
Processing job: id: 1, user: John, time: 10 seconds
Processing job: id: 2, user: Jerry, time: 4 seconds
Processing job: id: 3, user: Jimmy, time: 5 seconds
Processing job: id: 4, user: George, time: 7 seconds
Processing job: id: 5, user: Bill, time: 8 seconds
Added job in the queue: id: 6, user: Kenny, time: 10 seconds
Processing job: id: 6, user: Kenny, time: 10 seconds
```