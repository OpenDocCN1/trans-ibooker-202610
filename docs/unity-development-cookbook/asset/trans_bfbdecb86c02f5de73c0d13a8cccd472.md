

## 2.8 使用对象池管理对象

## 问题

你有一个项目，其中你会定期创建和销毁对象，并且希望该过程更高效。

## 解决方案

创建一个*对象池*：一个系统，允许对象在不再需要时暂时停用，并在被请求时重新激活（与对象在不需要时自行销毁、完全从内存中移除的情况相反）。

我们将通过创建一个脚本来实现这一点，该脚本管理一个 GameObject 队列并公开一个名为 GetObject 的方法。当调用 GetObject 时，它会先查看队列中是否有任何未使用且可以回收的现有对象。如果有，一个对象将被从队列中移除并返回；如果没有，则会创建一个新对象。

当对象不再需要时，它会联系创建它的对象池，被停用，并被加回队列，以便将来重复使用。对象在任何时候都不会被销毁；它只是暂时停放以供以后重复使用。

## 实现系统

为了实现此功能，我们将创建一个执行此行为的新脚本。我们还将创建一个接口，对象可以实现该接口以便在进入或离开队列时收到通知；这是可选的，但对于需要在（重新）进入场景时执行设置的物体很有用：

- 1. 创建一个新的 C# 脚本，命名为 ObjectPool.cs。
- 2. 向其中添加以下代码：

```
public interface IObjectPoolNotifier {
    // Called when the object is being returned to the pool.
    void OnEnqueuedToPool();

    // Called when the object is leaving the pool, or has just been
    // created. If 'created' is true, the object has just been created,
    // and is not being recycled.

    // (Doing it this way means you use a single method to do the
    // object's setup, for your first time and all subsequent times.)
    void OnCreatedOrDequeuedFromPool(bool created);
}
```

3. 更新 ObjectPool 类，使其如下所示：

```
public class ObjectPool : MonoBehaviour
{
    // The prefab that will be instantiated.
    [SerializeField]
    private GameObject prefab;

    // The queue of objects that are not currently in use
    private Queue<GameObject> inactiveObjects = new Queue<GameObject>();

    // Gets an object from the pool. If there isn't one in the queue,
    // a new one is created.
    public GameObject GetObject() {

        // Are there any inactive objects to reuse?
        if (inactiveObjects.Count > 0) {

            // Get an object from the queue
            var dequeuedObject = inactiveObjects.Dequeue();

            // Queued objects are children of the pool, so we move them
            // back to the root
            dequeuedObject.transform.parent = null;
            dequeuedObject.SetActive(true);

            // If there are any MonoBehaviours on this object that
            // implement IObjectPoolNotifier, let them know that this
            // object just left the pool

            var notifiers = dequeuedObject
                .GetComponents<IObjectPoolNotifier>();

            foreach (var notifier in notifiers) {
                // Notify the script that it left the pool
                notifier.OnCreatedOrDequeuedFromPool(false);
            }

            // Return the object for use
            return dequeuedObject;
        } else {

            // There's nothing in the pool to reuse. Create a new object.
            var newObject = Instantiate(prefab);
```

```
// Add the pool tag so that it's able to return to the pool
// when done.
var poolTag = newObject.AddComponent<PooledObject>();

poolTag.owner = this;

// Mark the pool tag so that it never shows up in the
// Inspector. There's nothing configurable on it; it only
// exists to store a reference back to the pool that
// creates it.
poolTag.hideFlags = HideFlags.HideInInspector;

// Notify the object that it was created
var notifiers = newObject
    .GetComponents<IObjectPoolNotifier>();

foreach (var notifier in notifiers)
{
    // Notify the script that it was just created
    notifier.OnCreatedOrDequeuedFromPool(true);
}

// Return the object we just created.
return newObject;
}
}

// Disables an object and returns it to the queue for later reuse.
public void ReturnObject(GameObject gameObject)
{
    // Find any component that we need to notify
    var notifiers = gameObject
        .GetComponents<IObjectPoolNotifier>();

    foreach (var notifier in notifiers)
    {
        // Let it know that it's returning to the pool
        notifier.OnEnqueuedToPool();
    }

    // Disable the object and make it a child of this one, so that
    // it doesn't clutter up the Hierarchy
    gameObject.SetActive(false);
    gameObject.transform.parent = this.transform;

    // Put the object into the inactive queue
    inactiveObjects.Enqueue(gameObject);
}
}
```

![](img/e54c00769db669e3746fa1e50090241f_61_0.png)

这段代码还无法编译，因为 PooledObject 类尚不存在。我们即将添加它。

为了使对象能够返回到创建它们的池中，它们需要对池有一个引用。为了支持这一点，我们将创建一个非常简单的脚本，它没有任何逻辑，并存储对对象池的引用。

![](img/e54c00769db669e3746fa1e50090241f_61_1.png)

由于此脚本实际上不会影响游戏玩法，因此没有理由将其包含在 Inspector 中。这就是 hideFlags 属性控制的内容；通过在 GetObject 方法中将其设置为 HideInInspector，我们告诉 Unity 不要显示它。对于此用例来说很方便，但请注意使用此功能，因为如果看不到组件，很容易忘记组件的存在。你可能会意外编写一个组件，导致一些无法理解的行为，因为你没有意识到该组件的存在。

- 1. 创建一个新的脚本，命名为 PooledObject.cs，并向其中添加以下代码：

```
// A simple script that just exists to store a reference to an
// ObjectPool. Exists to be used by the ReturnToPool extension method.
public class PooledObject : MonoBehaviour
{
    public ObjectPool owner;
}
```

为了简化将 GameObject 返回到对象池的过程，我们将向 GameObject 添加一个方法，使其非常方便。为此，我们添加一个名为 ReturnToPool 的扩展方法；一旦此代码存在，你就可以在任何 GameObject 上调用 ReturnToPool，它将返回到创建它的池。

- 2. 向 PooledObject.cs 添加以下代码：

```
// A class that adds a new method to the GameObject class: ReturnToPool.
public static class PooledGameObjectExtensions {

    // This method is an extension method (note the "this" parameter).
    // This means that it’s a new method that’s added to all instances
    // of the GameObject class; you call it like this:
    // gameObject.ReturnToPool()

    // Returns an object to the object pool that it was created from
    public static void ReturnToPool(this GameObject gameObject) {
```

```
// Find the PooledObject component.
var pooledObject= gameObject.GetComponent<PooledObject>();

// Does it exist?
if (pooledObject == null) {
    // If it doesn't, it means that this object never came from
    // a pool.
    Debug.LogErrorFormat(gameObject,
        "Cannot return {0} to object pool, because it was not" +
        "created from one.", gameObject);

    return;
}

// Tell the pool we came from that this object should be
// returned.
pooledObject.owner.ReturnObject(gameObject);
}
```

我们现在已完成系统的实现。

## 测试

为了测试，我们将创建两个脚本。一个将使用对象池，另一个将存在以便在延迟后返回自身（这实际上意味着它所附加的对象）到对象池。我们将首先创建对象池：

- 1. 创建一个新的 C# 脚本，命名为 ObjectPoolDemo.cs，并添加以下代码：

```
// An example of using an object pool.
public class ObjectPoolDemo : MonoBehaviour
{
    // The object pool from which we'll be getting our objects
    [SerializeField]
    private ObjectPool pool;

    IEnumerator Start() {

        // Get and place an object from the pool every 0.1-0.5 seconds
        while (true) {

            // Get (or create, we don't care which) an object from pool
            var o = pool.GetObject();

            // Pick a point somewhere inside a sphere of radius 4
            var position = Random.insideUnitSphere * 4;

            // Place it
            o.transform.position = position;
```

