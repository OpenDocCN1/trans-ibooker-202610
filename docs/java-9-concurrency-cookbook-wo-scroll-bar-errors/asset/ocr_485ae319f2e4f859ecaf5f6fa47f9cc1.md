+   7. Create and start a Thread object to execute a client task:

```
Client client=new Client(list);
Thread thread=new Thread(client);
thread.start();
```

+   8. Get three String objects from the list every 300 milliseconds using the take() method of the list object. Repeat this cycle five times. Write the strings in the console:

```
for (int i=0; i<5 ; i++) {
    for (int j=0; j<3; j++) {
        String request=list.take();
        System.out.printf("Main: Removed: %s at %s. Size: %d\n",
                        request,new Date(), list.size());
    }
    TimeUnit.MILLISECONDS.sleep(300);
}
```

+   9. Write a message to indicate the end of the program:

```
System.out.printf("Main: End of the program.\n");
```

# How it works...

In this recipe, you used LinkedBlockingDeque, parameterized by the String class, to work with a non-blocking concurrent deque of data.

The Client class uses the put() method to insert strings into the deque. If the deque is full (because you have created it with fixed capacity), the method will block the execution of its thread until there is empty space in the list.

The Main class uses the take() method to get strings from the deque. If the deque is empty, the method blocks the execution of its thread until there are elements in the deque.

Both the methods of the LinkedBlockingDeque class used in this example can throw an InterruptedException exception if they are interrupted while they are being blocked. So, you have to include the necessary code to catch this exception.