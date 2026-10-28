6. Then, it calls the countDown() method of the CountDownLatch object:

```
controller.countDown();
```

7. Finally, it writes another message with the number of participants whose arrival is pending, using the getCount() method of the CountDownLatch object:

```
System.out.printf("VideoConference: Waiting for %d
participants.\n",controller.getCount());
```

8. Next, implement the main method of the video conference system. It's the run() method that every Runnable object must have:

```
@Override
public void run() {
```

9. First, use the getCount() method to write a message with the number of participants in the video conference:

```
System.out.printf("VideoConference: Initialization: %d
participants.\n",controller.getCount());
```

10. Then, use the await() method to wait for all the participants. As this method can throw an InterruptedException exception, you must include the code to process it:

```
try {
    controller.await();
```

11. Finally, write a message to indicate that all the participants have arrived:

```
System.out.printf("VideoConference: All the participants have come\n");
System.out.printf("VideoConference: Let's start...\n");
} catch (InterruptedException e) {
e.printStackTrace();
}
```

12. Next, create the Participant class and specify that it implements the Runnable interface. This class represents each participant in the video conference:

```
public class Participant implements Runnable {
```

13. Declare a private Videoconference attribute named conference:

```
private Videoconference conference;
```