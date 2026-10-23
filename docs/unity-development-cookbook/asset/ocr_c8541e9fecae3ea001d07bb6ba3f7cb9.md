3. Double-click the script. Unity will open it in your editor: by default, Visual Studio if you’re on a PC, Visual Studio for Mac if you’re on a Mac, or your text editor if you’re on Linux.

![](bbox=[0.19, 0.16, 0.27, 0.25])

You can customize which program Unity uses to edit scripts by going to Unity’s preferences and choosing External Tools → External Script Editor. We’re particularly keen on Visual Studio Code and BBEdit on macOS.

You’ll see code that looks like this:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Mover : MonoBehaviour
{
    void Start()
    {
    }

    void Update()
    {
    }
}
```

![](bbox=[0.18, 0.57, 0.27, 0.66])

Start is called before the first frame update, and Update is called once per frame.

4. Modify the Mover class to look like this:

```
public class Mover : MonoBehaviour
{
    public Vector3 direction = Vector3.up;

    float speed = 0.1f;

    void Update()
    {
        var movement = direction * speed;
```