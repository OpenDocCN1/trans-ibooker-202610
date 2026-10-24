```
private void writeObject(ObjectOutputStream oos) throws IOException {
    ...
    for (Map.Entry<K,V> e : entrySet()) {
        oos.writeObject(e.getKey());
        oos.writeObject(e.getValue());
    }
    ...
}
```

This looks very much like the code that didn’t work for the Point example. The difference in this case is that the code still writes out object when those objects can be the same. A TreeMap cannot have two nodes that are the same, so there is no need to write out node references. The TreeMap can have two values that are the same, so the values must be written out as object references.

This brings us full circle: as I stated at the beginning of this section, getting object serialization optimizations correct can be tricky. But when object serialization is a significant bottleneck in an application, optimizing it correctly can offer important benefits.

# What About Externalizable?

Another approach to optimizing object serialization is to implement the Externalizable interface rather than the Serializable interface.

The practical difference between these two interfaces is in the way they handle non-transient fields. The Serializable interface writes out nontransient fields when the writeObject() method calls the defaultWriteObject() method. The Externalizable interface has no such method. An Externalizable class must explicitly write out all fields, transient or not, that it is interested in transmitting.

Even if all the fields in an object are transient, it is better to implement the Serializable interface and call the defaultWriteObject() method. That path leads to code that is much easier to maintain as fields are added to (and deleted from) the code. And there is no inherent benefit to the Externalizable interface from a performance point of view: in the end, what matters is the amount of data that is written.