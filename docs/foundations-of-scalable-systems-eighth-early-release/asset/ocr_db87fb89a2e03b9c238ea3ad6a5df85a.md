While the syntax and semantics of these RPC/RMI technologies vary, the essence of how each operates is the same. Let’s continue with our Java example of mybank.com to use this as an example of the whole class of approaches. Java offers a Remote Method Invocation (RMI) API for building client-server applications.

Using Java RMI, we can trivially make our MyBank interface example from above into a remote interface, as illustrated below.

```
import java.rmi.*;
// Simple mybank.com server interface
public interface MyBank extends Remote{
    public float balance  (String accNo)
        throws RemoteException;
    public boolean statement(String month)
        throws RemoteException ;
    // other operations
}
```

The java.rmi.Remote interface serves as a marker to inform the Java compiler we are creating an RMI server. In addition, each method must throw java.rmi.RemoteException. These exceptions represent errors that can occur when a distributed call between two objects is invoked over a network. The most common reasons for such an exception would be a communications failure or the server object having crashed.

We then must provide a class that implements this remote interface. The sample code below shows an extract of the server implementation - the complete code for this example is in this book’s code github repository.

```
public class MyBankServer extends UnicastRemoteObject
                        implements MyBank {
    // constructor/method implementations omitted
    public static void main(String args[]){
        try{
            MyBankServer server=new MyBankServer();
            // create a registry in local JVM on default port
            Registry registry = LocateRegistry.createRegistry(1099);
            registry.bind("MyBankServer", server);
            System.out.println("server ready");
```