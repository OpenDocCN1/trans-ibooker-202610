You have learned conditional statements with if and switch. In my opinion, you can use switch statements if the options are below 5; otherwise, you can use an if-statement with operators.

# Looping

The looping task is useful when you perform the same task continuously. In the Sketch program, we can implement looping tasks using for, while, and do..while statements. We can declare a for-statement as follows.

```
for(start;conditional;increment/decrement) {
    <codes>
}
```

For a while statement, we can implement as follows.

```
while(selection) {
    <codes>
}
```

We also can use do..while for looping. You can run the first code step, then we select a while statement.

```
do {
    <codes>
} while(selection);
```

Now we can build a Sketch program to implement looping using for, while, and do..while statement. Write this complete program using the Arduino software.

```
void setup() {
  // initialize serial communication
  Serial.begin(115200);
  while (!Serial) {
```