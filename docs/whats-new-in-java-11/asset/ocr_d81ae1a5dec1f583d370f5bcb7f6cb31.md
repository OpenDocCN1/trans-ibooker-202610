# Table 1-1. *Modules removed from Java 11*

| Module | Packages |
|---|---|
| JavaBeans Activation Framework (JAF) | `javax.activation` |
| Java Transaction API (JTA) | `java.transaction` |
| Commons Annotation API | `java.xml.ws.annotation` |
| Java Architecture for XML Binding (JAXB) | `jdk.xml.bind` |
| Java API for XML-Based Web Services (JAX-WS) | `jdk.xml.ws` |
| CORBA | `javax.activity`, `javax.rmi`, `org.omg` |

So, what will happen if you use one of these modules in your code and directly upgrade to Java 11? Your code will no longer compile, or you could run into runtime errors. Although prior versions allowed for a workaround by manually adding the module using `--add-modules`, this no longer works for Java 11.

Your best bet is to find a replacement for the functionality available as an external dependency and add it to your compilation classpath. You will find that many of the removed modules have a representation on Maven Central. Table 1-2 lists some possible replacements. For additional references, refer to JEP 320.