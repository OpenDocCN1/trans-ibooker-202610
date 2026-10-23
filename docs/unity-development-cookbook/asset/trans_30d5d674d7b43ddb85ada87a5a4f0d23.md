

当你将 movement 乘以 Time.deltaTime 时，movement 不再代表一个固定的距离，而是一个随时间变化的距离。当你将它提供给 transform 的 Translate 方法时，你会发现物体以固定的速度移动，而不管渲染器生成新帧的速度有多快。

deltaTime 并不是你唯一可用的属性。如果你想知道项目运行了多长时间，你可以访问 time 变量。你还可以通过修改 Time.timeScale 属性来减慢和加快时间，这对于暂停项目非常有用。

## 2.4 使用游戏对象的组件

### 问题

你想要使用附加到游戏对象的组件。

### 解决方案

使用 GetComponent 方法获取与你的脚本附加在同一游戏对象上的组件。

调用 GetComponent 时，你指定要查找的组件类型作为泛型参数。例如，要获取游戏对象上的 Renderer 组件，你调用方法 GetComponent<Renderer>。

![](img/e54c00769db669e3746fa1e50090241f_48_0.png)

如果游戏对象上没有正确类型的组件，GetComponent 将返回 null。如果你尝试通过调用它的方法或访问其属性和字段来使用此对象，你将导致抛出 NullPointerException，这至少会立即停止脚本的当前帧，最坏的情况会导致整个项目崩溃。始终先检查结果是否为 null。

一旦你获得了对另一个组件的引用，你就可以使用它。例如，如果你有一个 MeshRenderer，你可以通过 GetComponent 访问组件，然后访问其属性，随着时间的推移改变其材质的颜色：

```
public class ColorFading : MonoBehaviour
{
    void Update()
    {
        var meshRenderer = GetComponent<MeshRenderer>();
```

```
// Check to make sure that it's valid before we use it
if (meshRenderer == null) {
    return;
}

var sineTime = Mathf.Sin(Time.time) + 1 / 2f;
var color = new Color(sineTime, 0.5f, 0.5f);
meshRenderer.material.color = color;
}
```

### 讨论

除了 GetComponent 方法之外，还有许多相关方法可以访问多个组件、访问父对象上的组件或访问子对象上的组件：

- GetComponent<T>
在当前对象上查找类型为 T 的组件

- GetComponents<T>
查找当前对象上所有类型为 T 的组件并以数组形式返回

- GetComponentInChildren<T>
在当前对象或其任何子对象（或其子对象的子对象，进行深度优先搜索）上查找第一个类型为 T 的组件

- GetComponentsInChildren<T>
与 GetComponentInChildren<T> 相同，但查找所有匹配的组件并以数组形式返回

- GetComponentInParent<T>
在当前对象或其父对象（直到顶级对象）上查找第一个类型为 T 的组件

- GetComponentsInParent<T>
在当前对象或其父对象（直到顶级对象）上查找第一个类型为 T 的组件

如果你每帧都会使用一个对象，你可以通过在 Awake 或 Start 中调用 GetComponent（或其相关函数）并将结果存储在类的变量中来稍微提高性能。

例如，这里是之前相同的代码，但有一个不同之处——GetComponent 只在 Start 方法中调用一次：

```
public class ColorFading : MonoBehaviour
{
    // The mesh renderer component will be stored in this after Start
    MeshRenderer meshRenderer;

    void Start()
    {
        // Get the component and cache it
        meshRenderer = GetComponent<MeshRenderer>();
    }

    void Update()
    {
        // Check to make sure that it's valid before we use it
        if (meshRenderer == null) {
            return;
        }

        // meshRenderer has already been stored, so we can just start
        // using it

        var sineTime = Mathf.Sin(Time.time) + 1 / 2f;
        var color = new Color(sineTime, 0.5f, 0.5f);
        meshRenderer.material.color = color;
    }
}
```

## 2.5 查找附加到游戏对象的对象

### 问题

你想要使用附加到游戏对象的组件，但你不知道哪个游戏对象拥有它们。

### 解决方案

使用 FindObjectOfType 方法查找单个所需类型的组件：

```
// Find a single Mover object in the scene
var mover = FindObjectOfType<Mover>();
```

![](img/e54c00769db669e3746fa1e50090241f_51_0.png)

如果有多个指定类型的组件，FindObjectOfType 将随机选择一个。随机对象通常按其 InstanceID 的升序选择。例如，对象 -24084 会在对象 -24082 之前返回。

如果你想找到特定类型的所有组件，请使用 FindObjectsOfType，它返回所有组件的数组：

```
// Find all Mover objects in the scene
var allMovers = FindObjectsOfType<Mover>();
```

请注意，与 GetComponent（参见配方 2.4）一样，FindObjectsOfType 如果找不到你要找的内容将返回 null。在尝试使用它们之前，始终检查你的结果是否为 null。

![](img/e54c00769db669e3746fa1e50090241f_51_1.png)

这些方法只会返回当前处于活动状态的对象。它们找不到任何非活动对象。

### 讨论

FindObjectOfType 通过搜索场景中每个对象上的每个组件来工作。不要在 Update 方法中调用它，因为它会给 Unity 带来大量工作，你不想每帧都这样做。相反，在 Start 或 Awake 时调用该方法，并将结果存储在变量中以供以后使用。

## 2.6 单例

### 问题

你想要创建一个类，它始终只有一个实例，并且其他代码可以始终访问它。

### 解决方案

单例是一种设计为永远只有一个实例的类。例如，全局可用的事物，如游戏玩法管理器、输入管理器或其他实用工具，通常实现为单例。

要创建一个，首先创建你的游戏对象，并创建并附加一个新脚本。

接下来，在你的类上创建一个公共静态变量，使用类的类型。然后，在 Awake 方法中，将该变量的值设置为 this，这意味着代码的任何其他部分都将能够引用该类的该特定实例。例如：

```
public class SimpleSingleton : MonoBehaviour
{
    // A 'static' variable is shared between all instances of the class
    public static SimpleSingleton instance;

    void Awake() {
        // When this object wakes up, it sets the instance variable to itself;
        // because the instance variable is public and static, any
        // class from any location can access it and call its methods.
        instance = this;
    }

    // An example method that any other part of the code can call, as long
    // as there's a game object in the scene that has a
    // SimpleSingleton component attached
    public void DoSomething() {
        Debug.Log("I'm the singleton! Hello!");
    }
}
```

你可以通过在类上使用 instance 变量来访问单例实例：

```
// Accessing the singleton and calling a method on it
SimpleSingleton.instance.DoSomething();
```

此单例实例可以从代码中的任何位置访问，只要场景中有一个游戏对象附加了该脚本即可。

### 讨论

如果你有超过几个单例类，编写 instance 变量和 Awake 方法可能会变得繁琐，重复的代码可能导致意外错误。

你可以通过使用泛型创建更复杂的单例类。以下类设计为被子类化，并提供 instance 方法的自动实现。它还处理设置 instance 变量以指向单个实例的问题——如果脚本已附加到场景中的游戏对象，它将被找到；如果不存在，将创建一个新的：

```
// This class is generic, which means that you can create multiple versions
// of it that vary depending on what type you specify for 'T'.
// In this case, we're also adding a type constraint, which means that 'T'
// must be a MonoBehaviour subclass.
public class Singleton<T> : MonoBehaviour where T : MonoBehaviour
{
