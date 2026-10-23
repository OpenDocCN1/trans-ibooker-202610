upgrade from Unicode 8 to 10 includes 16,018 new characters and 10 new scripts. A script is a collection of letters and other written signs used to represent textual information. For example, Unicode 10 adds Masaram Gondi, a South-Central Dravidian language.

One of the long-awaited additions to Unicode 10 is the “Colbert emoji”, a face with a neutral mouth and single eyebrow raised made popular by comedian Stephen Colbert. The following example makes use of the Colbert emoji:

```
@Test
void canUseColbertEmoji() {
    String unicodeCharacter = "\uD83E\uDD28";
    assertEquals("🤨", unicodeCharacter);
}
```

Next up, we look at a lower-level change in Java 11: nest-based access control. The impact to the end user is less visible, but it addresses a long-standing technical debt that was finally refactored.

# NEST-BASED ACCESS CONTROL

To understand the next change to Java 11, we need to take a look at a nested class. Let’s assume that you have an outer class called Outer.java and an inner class called Inner.Java within it. Inner.java can access all private fields and methods of Outer.java because they logically belong together. Figure 1-5 visualizes the data structure.