```
CurrentSpeed = currentSpeed;
}
}
}
```

Now assume you have two direct descendants of the Car type named MiniVan and SportsCar. Each overrides the abstract TurboBoost() method by displaying an appropriate message via console message. Insert two new C# class files into your project, named MiniVan.cs and SportsCar.cs, respectively. Update the code in each file with the relevant code.

```
//SportsCar.cs
using System;
namespace Carlibrary
{
public class SportsCar : Car
{
public SportsCar(){ }
public SportsCar(
string name, int maxSpeed, int currentSpeed)
: base (name, maxSpeed, currentSpeed){ }

public override void TurboBoost()
{
Console.WriteLine("Ramming speed! Faster is better...");
}
}
}
```

```
//MiniVan.cs
using System;
namespace Carlibrary
{
public class MiniVan : Car
{
public MiniVan(){ }
public MiniVan(
string name, int maxSpeed, int currentSpeed)
: base (name, maxSpeed, currentSpeed){ }

public override void TurboBoost()
{
// Minivans have poor turbo capabilities!
State = EngineStateEnum.EngineDead;
Console.WriteLine("Eek! Your engine block exploded!");
}
}
}
```