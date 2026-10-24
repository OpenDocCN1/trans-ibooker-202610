

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_35_0.png)

**图 2-1. 声明变量**

图 2-1 显示了上述代码。要打印消息，我们使用 Serial.print() 和 Serial.println() 函数。我们可以使用 Serial.print() 在不换行 "\r\n" 的情况下打印消息。否则，我们可以使用 Serial.println() 打印带换行的消息。

所有使用 Serial 库打印的消息都将显示在串行通信通道上。现在我们可以保存此程序。然后，编译并上传到 Arduino Nano 33 IoT 板。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_37_0.png)

**图 2-3. Serial Monitor 工具上的程序输出**

如果您在 Serial Monitor 工具上看不到输出消息，可以点击 Arduino Nano 33 IoT 板上的 RESET 按钮。您可以在 micro USB 连接器旁边找到此按钮。您可以在图 2-4 中看到 RESET 按钮的位置。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_37_1.png)

**图 2-4. 在 Arduino Nano 33 IoT 上点击 RESET 按钮**

在 loop() 函数中，我们什么都不做。所有代码都在 setup() 函数中运行。这就是你可能看不到程序输出的原因，因为我们看到它太晚了。

```cpp
void loop() {
}
```

## 运算符

Sketch 程序采用 C++ 运算符。我们可以声明算术运算符来执行数学运算。我们可以使用以下算术运算符：

- % （余数）
- * （乘法）
- + （加法）
- - （减法）
- / （除法）
- = （赋值运算符）

对于布尔运算符，我们实现 && 用于逻辑与，|| 用于逻辑或，! 用于逻辑非。

## 条件语句

我们可以执行基于动作的条件。例如，如果光传感器获得低强度值，我们想打开一盏灯。在 Sketch 中，我们使用 if 和 switch 语法实现条件语句。带有 if 的条件语句可以如下声明：

```cpp
if(<conditional>) {
  // do something
} else {
  // do something
}
```

我们可以将条件值放在 <conditional> 上，例如应用布尔和算术运算符。作为演示，我们可以在 Arduino Nano 33 IoT 上创建一个 Sketch 程序。你编写这个完整程序。

```cpp
long num_a;
long num_b;

void setup() {
  // initialize serial communication
  Serial.begin(115200);
  while (!Serial) {
    ;
  }
}

void loop() {
  num_a = random(100);
  num_b = random(100);

  // print
  Serial.print("num_a: ");
  Serial.print(num_a);
  Serial.print(", num_b: ");
  Serial.println(num_b);

  if(num_a > num_b) {
    Serial.println("num_a > num_b");
  }else {
    Serial.println("num_a <= num_b");
  }

  delay(2000);
}
```

将此程序保存为 conditional。现在你可以编译并上传此程序到 Arduino Nano 33 IoT 板。打开 Serial Monitor 工具即可看到此程序输出。图 2-5 显示了我对条件程序的程序输出。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_41_0.png)

**图 2-5. 条件 if 程序的程序输出**

它是如何工作的？

此程序在 loop() 函数上为 num_a 和 num_b 变量生成随机值。

```cpp
void loop() {
    num_a = random(100);
    num_b = random(100);
}
```

接下来，我们使用 Serial 对象在串行终端上打印这些随机值。我们可以调用 Serial.print() 和 Serial.println() 函数。

```cpp
// print
Serial.print("num_a: ");
Serial.print(num_a);
Serial.print(", num_b: ");
Serial.println(num_b);
```

最后，我们使用条件-if 语句评估 num_a 和 num_b 上的值。我们检查 num_a 值是否大于 num_b。然后，我们在串行终端上打印结果。

```cpp
if(num_a > num_b) {
    Serial.println("num_a > num_b");
}else {
    Serial.println("num_a <= num_b");
}
```

下一个演示是使用 switch 语句实现条件。在一般情况下，我们可以如下声明 switch 语句。

```cpp
switch(value) {
              case val1: <code>
                                      break;
              case val2: <code>
break;
case val3: <code>
    break;
}
```

对于演示，我们构建一个程序使用 switch 语句评估 num_a 值。我们设置一个最大值为 5 的随机值。打开 Arduino 软件并编写此完整程序。

```cpp
long num_a;

void setup() {
    // initialize serial communication
    Serial.begin(115200);
    while (!Serial) {
        ;
    }
}
void loop() {
    num_a = random(5);

    // print
    Serial.print("num_a: ");
    Serial.println(num_a);
    switch(num_a) {
        case 0:
            Serial.println("num_a value is 0");
            break;
        case 1:
            Serial.println("num_a value is 1");
            break;
        case 2:
            Serial.println("num_a value is 2");
            break;
        case 3:
            Serial.println("num_a value is 3");
            break;
        case 4:
            Serial.println("num_a value is 4");
            break;
    }
    delay(2000);
}
```

将此程序保存为 ConditionalSwitch。你可以编译并上传此程序到 Arduino Nano 33 IoT。要查看程序输出，你可以打开 Serial Monitor 工具。你可以在图 2-6 中看到我的程序输出。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_44_0.png)

**图 2-6. switch 程序上的程序输出**

它是如何工作的？

此程序开始在 loop() 函数上生成随机值。结果存储在 num_a 变量中。然后，我们将此值打印到串行终端。

```cpp
void loop() {
    num_a = random(5);

    // print
    Serial.print("num_a: ");
    Serial.println(num_a);
}
```

接下来，我们使用 switch 语句评估 num_a 变量。我们检查 num_a 的值为：0、1、2、3 和 4。我们在每个 switch-case 语句上打印消息。

```cpp
switch(num_a) {
    case 0:
        Serial.println("num_a value is 0");
        break;
    case 1:
        Serial.println("num_a value is 1");
        break;
    case 2:
        Serial.println("num_a value is 2");
        break;
    case 3:
        Serial.println("num_a value is 3");
        break;
    case 4:
        Serial.println("num_a value is 4");
        break;
}
```

你已经学习了使用 if 和 switch 的条件语句。在我看来，如果选项在 5 以下，你可以使用 switch 语句；否则，你可以使用带运算符的 if 语句。

## 循环

循环任务在你需要连续执行相同任务时很有用。在 Sketch 程序中，我们可以使用 for、while 和 do..while 语句实现循环任务。我们可以如下声明 for 语句。

```cpp
for(start;conditional;increment/decrement) {
    <codes>
}
```

对于 while 语句，我们可以如下实现。

```cpp
while(selection) {
    <codes>
}
```

我们也可以使用 do..while 进行循环。你可以先运行第一步代码，然后我们选择一个 while 语句。

```cpp
do {
    <codes>
} while(selection);
```

现在我们可以构建一个 Sketch 程序，使用 for、while 和 do..while 语句实现循环。使用 Arduino 软件编写此完整程序。

```cpp
void setup() {
  // initialize serial communication
  Serial.begin(115200);
  while (!Serial) {
    ;
  }
}

void loop() {
    long val = random(15);
    int i;

    // print
    Serial.print("val: ");
    Serial.println(val);

    // looping
    Serial.println("Looping: for");
    for(i=0;i<val;i++){
        Serial.print(i);
        Serial.print(" ");
    }
    Serial.println();

    Serial.println("Looping: while");
    int start = 0;
    while(start < val) {
        Serial.print(start);
        Serial.print(" ");

        start++;
    }
    Serial.println();

    Serial.println("Looping: do..while");
    start = 0;
    do {
        Serial.print(start);
        Serial.print(" ");
        start++;
    }while(start < val);
    Serial.println();

    delay(3000);
}
```

你可以将此程序保存为 Looping。你可以编译并上传此程序到 Arduino Nano 33 IoT 板。上传程序到 Arduino Nano 33 IoT 板后，你可以打开 Serial Monitor 工具查看程序输出。你可以在图 2-7 中看到我的程序输出。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_48_0.png)

**图 2-7. 循环的程序输出**

它是如何工作的？

我们为循环程序设置一个随机值。

```cpp
void loop() {
  long val = random(15);
  int i;
```

