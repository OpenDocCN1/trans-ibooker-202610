```
stats=new ArrayList<String>();
}
```

3. Implement the newThread() method. This method will receive a Runnable interface and return a thread object for this Runnable interface. In our case, we generate the name of the thread object, create the new thread object, and save the statistics:

```
@Override
public Thread newThread(Runnable r) {
    Thread t=new Thread(r,name+"-Thread_"+counter);
    counter++;
    stats.add(String.format("Created thread %d with name %s on %s\n", t.getId(),t.getName(),new Date()));
    return t;
}
```

4. Implement the getStatistics() method; it returns a String object with the statistical data of all the thread objects created:

```
public String getStats(){
    StringBuffer buffer=new StringBuffer();
    Iterator<String> it=stats.iterator();
    
    while (it.hasNext()) {
        buffer.append(it.next());
        buffer.append("\n");
    }
    
    return buffer.toString();
}
```

5. Create a class called Task and specify that it implements the Runnable interface. In this example, these tasks are going to do nothing apart from sleeping for 1 second:

```
public class Task implements Runnable {
    @Override
    public void run() {
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```