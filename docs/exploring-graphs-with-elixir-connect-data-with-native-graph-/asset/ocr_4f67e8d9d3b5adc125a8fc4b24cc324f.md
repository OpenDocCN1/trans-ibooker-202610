```
public String toString() { return "quite heavy"; }
}
```

This class represents a hypothetical heavyweight resource. In its constructor, we print a message to tell us when it's created. Let's use an instance of this class in the first trial version of the Holder class, named HolderNaive.

# lazy/fpij/HolderNaive.java

```
public class HolderNaive {
private Heavy heavy;

public HolderNaive() {
System.out.println("Holder created");
}

public Heavy getHeavy() {
if(heavy == null) {
heavy = new Heavy();
}

return heavy;
}

//...
```

At first glance, this code appears simple. We created a null reference, heavy, and assigned it to a proper instance on the first call to the getHeavy method. Let's use this class to create an instance of HolderNaive and see if it postpones the creation of the Heavy instance.

# lazy/fpij/HolderNaive.java

```
final HolderNaive holder = new HolderNaive();
System.out.println("deferring heavy creation...");
System.out.println(holder.getHeavy());
System.out.println(holder.getHeavy());
```