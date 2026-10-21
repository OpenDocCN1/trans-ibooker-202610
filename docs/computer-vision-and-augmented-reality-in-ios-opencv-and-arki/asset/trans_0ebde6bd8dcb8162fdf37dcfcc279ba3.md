

## ARKit 3 中的面部追踪

![](img/7d8e54ee4578088e252f4c75b5cd4c88_152_0.png)

图 7-6. ARKit 3 中的面部追踪

### 用户等级：基础

自增强现实引入行业以来，它为我们提供了开发面向移动技术用户的互动且令人兴奋的机会的能力。AR 可以在多种方式上为我们节省时间，包括查看理想的沙发在我们公寓中的外观、判断所选的油漆颜色是否适合室内装饰，甚至即时试戴新妆容。

面部追踪是 iPhone X 及更新机型上 ARKit 3 的强大功能。我们可以用合适的组件快速创建用于试戴珠宝、口红或眼镜的应用程序。在本示例中，我们将集中于最后一种选择：创建一个用于试戴眼镜的 AR 应用，并带有简单的镜框颜色切换。

## ARKit 3 的设置和规范

由于 ARKit 3 的 AR 面部追踪需要 iPhone X 中引入的 A11 Bionic 芯片，因此我们需要该系统或更新的系统来使我们的示例正常工作。我们需要 Unity 2019.1 或更高版本来构建我们的项目

以利用 ARKit 3 的所有功能，例如多面部追踪。在构建项目后，我们需要安装所需的 ARKit 包。自 2019 年 7 月起，ARKit for Unity 已包含在 ARFoundation 捆绑包中，因此我们需要从 Window 菜单打开 Package Manager 并安装以下包：

- 1. ARFoundation
- 2. AR Subsystems
- 3. AR Face Tracking 用于所有必需的面部追踪脚本
- 4. XR Legacy Input Helpers 用于 Tracked Pose Driver 脚本

安装后，将项目移动到 iOS 平台并设置播放器配置是个好主意。在 Player Settings 窗口中将 Target minimum iOS Version 设置为 11.0，因为 ARKit 3.0 需要最低 SDK 版本 11.0。

在同一窗口的 Architecture 下拉菜单中选择 ARM64，因为 ARKit 仅支持 64 位设备。你还需要指定 Camera Usage Description，即当应用请求使用系统摄像头的批准时出现的脚本。你可以将文本更改为你想要的任何内容，例如“ARKit 3 Face Detection。”

## 增强现实中的会话对象

项目正确设置后，我们就可以开始创建必需的 AR Session 对象。首先，我们在场景中创建一个 ARSession GameObject 并向其添加两个脚本：一个 AR Session 脚本和一个 AR Input Manager 脚本。

然后，我们使用 AR Session Origin 脚本和 AR Face Manager 创建一个 AR Session Origin 对象。由于 ARKit 很少使用设备的正面摄像头——它仅在面部追踪/识别场景中使用——

Face Manager 脚本至关重要。AR Session Origin 中有一个用于 Camera 脚本的空字段。

让我们只需将 Main Camera 对象放置在 AR Session Origin 对象内部，然后在 AR Session Origin 脚本中引用它。需要三个脚本：

- Pose Driver with Tracking（在 Tracked Pose Driver 中，将 Pose Source 更改为 Color Camera 并启用 Use Relative Transform）
- AR Camera History
- AR Camera Manager

你是否发现 AR Session Origin 脚本中的 AR Face Manager 中有一个用于 Face Prefab 的空字段？那是我们将用来想象我们的基于 AR Face 的眼镜的预制件。

## 预制件 AR 眼镜

我们需要创建一个新的 GameObject，我们将其命名为 GlassesPrefab。我们对预制件需要做的第一件事是为其附加两个组件。第一个是 ARFace 变量，它将为我们提供已识别的面部点数据。第二个是 ARGlassesController，这是一个脚本，将负责根据 ARFace 更新的数据定位眼镜。

![](img/7d8e54ee4578088e252f4c75b5cd4c88_155_0.png)

### 图 7-7. ARFace 应用

之后，我们需要在预制件内构建一个 GameObject，它将附加到我们的眼镜网格上，我们将使用 ARGlassesController 对其进行操作。预制件的最终形式应类似于前一图像所示。被操作的网格必须作为子对象保存在我们的预制件内。ARFaceManager 控制预制件的位置，而 ARFace 顶点是与检测到的面部相关的特征的局部点。

## ARGlassesController 类

```
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

[RequireComponent(typeof(ARFace))]
public class ARGlassesController : MonoBehaviour
{
    [field: SerializeField]
    public static ARGlassesController Instance { get; private set; }

    [field: SerializeField]
    public Transform ModelTransform { get; private set; }

    [field: SerializeField]
    public Material FrameMaterial { get; private set; }

    private ARFace ARFaceComponent { get; set; }

    private const string MATERIAL_COLOR_SETTING_NAME = "_Color";
    private const int AR_GLASSES_PLACEMENT_VERTICE_INDEX = 16;

    public void ChangeFrameColor (Color color)
    {
        if (FrameMaterial != null)
        {
            FrameMaterial.SetColor(MATERIAL_COLOR_SETTING_NAME, color)
        }
    }

    protected virtual void Awake ()
    {
        if (Instance == null)
        {
            Instance = this;
        }
    }
```

```
ARFaceComponent = GetComponent();
}

protected virtual void OnDestroy ()
{
    Instance = null;
}

protected virtual void OnEnable ()
{
    ARFaceComponent.updated += TryToUpdateModelStatus;
    ARSession.stateChanged += TryToUpdateModelStatus;
    TryToUpdateModelStatus();
}

protected virtual void OnDisable ()
{
    ARFaceComponent.updated -= TryToUpdateModelStatus;
    ARSession.stateChanged -= TryToUpdateModelStatus;
}

private void TryToUpdateModelStatus (ARFaceUpdatedEventArgs eventArgs)
{
    TryToUpdateModelStatus();
}

private void TryToUpdateModelStatus (ARSessionStateChangedEventArgs eventArg
{
    TryToUpdateModelStatus();
}
```

```
protected virtual void OnDisable ()
{
    ARFaceComponent.updated -= TryToUpdateModelStatus;
    ARSession.stateChanged -= TryToUpdateModelStatus;
}

private void TryToUpdateModelStatus (ARFaceUpdatedEventArgs eventArgs)
{
    TryToUpdateModelStatus();
}

private void TryToUpdateModelStatus (ARSessionStateChangedEventArgs eventAr;
{
    TryToUpdateModelStatus();
}

private void TryToUpdateModelStatus ()
{
    bool isFaceVisible = GetFaceVisibility();
    ModelTransform.gameObject.SetActive(isFaceVisible);

    if (isFaceVisible == true)
    {
        ModelTransform.localPosition = ARFaceComponent.vertices[AR_(
    }
}

private bool GetFaceVisibility()
{
    return enabled == true && ARFaceComponent.trackingState != Tracking!
}
```

## ARKit 3 中的 ARFace 数据和面部检测

过程很简单：当 ARFaceManager 检测到面部时，它会发送新的 ARFace 数据。由于我们的控制器链接到 ARFace 的 updated 事件，脚本会在检测到新的面部点坐标集时更新眼镜的位置。

顶点数组索引通常适用于人类面部上的相同特征点；唯一根据个人面部特征而变化的是整体位置和面部网格。我们将眼镜与顶点索引 16 对齐，得到了以下结果。

![](img/7d8e54ee4578088e252f4c75b5cd4c88_159_0.png)

## 选择颜色

![](img/7d8e54ee4578088e252f4c75b5cd4c88_160_0.png)

## 颜色选择器

我们喜欢使用连接到简单 UI 中按钮的 ColorController 脚本来调整镜框颜色，因此我们将 ARGlassesController 制作成单例，以便 ColorController 在场景中可用时可以访问它。

```
using UnityEngine;

public class ColorController : MonoBehaviour
{
    [field: SerializeField]
    public Color Color { get; private set; }

    public void SetFrameColor()
    {
        if (ARGlassesController.Instance != null)
        {
            ARGlassesController.Instance.ChangeFrameColor(Color);
        }
    }
}
```

SetFrameColor 函数链接到场景中每个颜色按钮的 Click 事件，使我们能够在点击按钮时将镜框颜色更改为控制器中指定的颜色。

## 总结

前面的说明涵盖了增强现实面部追踪应用的制作。这只是增强现实和 ARKit 中众多可能应用的一个实例。ARKit 面部追踪 API 可用于构建更复杂的应用程序，而不仅仅是时尚和化妆品，还可用于工业和企业应用，例如员工识别和用户身份验证。今天，苹果为我们提供了复杂且多功能的工具，可以赋能数千个应用程序。鉴于未来公司将更多投资于增强现实硬件和软件，我们很高兴看到该技术在不久的将来如何发展，以及苹果在不久的将来在硬件和软件方面将提供什么。

