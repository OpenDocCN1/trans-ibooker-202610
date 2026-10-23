
// 告诉立方体的 MeshRenderer 使用此材质。
cubeObject.GetComponent<MeshRenderer>().material = cubeMaterial;

// 现在我们将刚刚创建的对象存储为资源。

// 首先，存储 GameObject（使用并渲染网格和材质的组件集合），并将其标记为“主”对象。
ctx.AddObjectToAsset(name, cubeObject);
ctx.SetMainObject(cubeObject);

// 我们还需要存储网格和材质。
ctx.AddObjectToAsset(cubeMaterial.name, cubeMaterial);
ctx.AddObjectToAsset(cubeMesh.name, cubeMesh);

}
}
#endif

这看起来很长很吓人，但它只是一系列小的步骤。它定义了一个结构体 CubeDescription，用来描述立方体是什么：一个 Vector3 和一个 Color。然后它创建了一个 ScriptedImporter，名为 CubeImporter，读取相关文件的 JSON，并使用文件中的立方体信息创建一个新的网格。魔法！

![](img/e54c00769db669e3746fa1e50090241f_413_0.png)

要了解有关 Vector3 和 Unity 中其他数学类型的更多信息，请参阅第 4 章。

+   6. 返回 Unity。你的 Test.cube 文件现在将被导入。它现在包含三个可供 Unity 使用的资源：一个网格、一个材质，以及一个使用网格和材质的游戏对象。你可以像其他模型一样将文件拖入场景。

## Discussion

启用 Unity 理解你的自定义文件类型打开了一个充满可能性的世界。如果你例如构建了一个导出关卡文件的外部关卡编辑器，该系统将允许你为 Unity 编辑器配备读取你文件的能力。

![](img/e54c00769db669e3746fa1e50090241f_413_1.png)

Unity 社区为 Unity 不一定开箱即支持的许多常见文件格式构建了导入器。如果你缺少文件格式功能，在尝试自己实现之前先在互联网上搜索！

## Index

## Symbols

2.5D 场景，创建，107
2D 混合树，210
2D 图像，89-108
2D 物理，创建带有的精灵，93
2D 纹理，Unity 支持的文件格式，19
3D 图像，109-158
3D 对象，19
3D 物理系统，66，159
  （另见物理）
9 切片，339

## A

抽象类，389，390
Account 按钮，5
操作
  将操作绑定到手柄控制器上的按钮，63
  使用 Input Actions 字段更改组件的操作，64
  使立方体能够响应操作，63
Add Component 按钮
  通过它创建脚本，29
  Input -> Player Input，59
  Physics 2D -> Surface Effector 2D，100
AddForce 方法（Rigidbody2D），98
添加向量，73
按场景增量加载，228
代理，307
  创建 BallAgent 示例，307-310
  在 Unity 编辑器中设置，310
  训练，311
AI（人工智能），281

定义 AI 实体和玩家可以遵循的路径，288-289
敌人检测躲避位置，297-302
让游戏中的实体遵循路径，289-291
albedo（基础）颜色，111
Alpha Clip Threshold 值，142
  连接到 Dissolve Amount，140
Alpha 值，142
ambientCG，116
锚点，335
角度
  找到到目标的角度，86
  在向量之间，处理，83-84
动画剪辑，189
  拖入混合树检查器，208
  设置范围，203
动画曲线，193，342
Animation 面板，191
  显示动画的关键帧，192
动画参数，198
动画，189-223
  为 UI 的部分制作动画，339-342
  随时间对着色器进行动画，134-135
  自动切换摄像机，219
  基础角色移动，194-199
  混合移动，208
  电影式摄像机跟踪，216-219
  控制动画着色器的速度，136-137
  创建精灵动画，92
  反向运动学，199-203
  保持多个对象在视野中，220

蒙版移动，203-207
同步导航和动画，212-216
对象的单一动画，190-194
Animator 组件，93，190
动画控制器，192，208
anyKeyDown 属性，56
Application.persistentDataPath 属性，373，374
ApplyForces 模式，187
反余弦，84
在向量上执行算术，73
人工智能（见 AI）
AssetPostprocessor 类，369
资源
  创建用于存储新自定义 Input Actions 的资源，60
  管理，18
  处理，368-370
  使用 ScriptableObject 在资源中存储数据，52-54
Assets 文件夹，10
  通过它创建 C# 脚本，26
Assets 菜单
  Create -> Animation，190
  Create -> Animator Controller，190，199
  Create -> Avatar Mask，205
  Create -> Input Actions 菜单，61
  Create -> Material，109，113
属性绘制器，自定义，365-367
音频，313
  使用脚本播放，326
  设置混音器，316-318
  使用音频效果，318-320
  使用多个音频区域，324-326
音频文件，19
  Unity 支持的，314
AudioClip 资源，313
AudioListener 组件，316
AudioManager 组件示例，328-330
AudioSource 组件，314
  控制播放的方法，326
avatar 遮罩，205
Awake 类，38
Awake 方法，31
B
使用烘焙的发射源，145
烘焙光照，143-144，149
行为，281-302
创建可保存的 MonoBehaviour，391-392
创建 SaveableBehaviour，389-390
定义 AI 实体和玩家可以遵循的路径，288
敌人检测何时可以看到玩家，281-287
敌人检测躲避位置，297-302
让游戏中的实体遵循路径，289-291
为 Player Input 组件设置 Invoke Unity Events，59
为 ML 代理设置，310
Behavior Parameters 组件，310
Bfxr（复古风格音效），314
绑定，61
  将操作绑定到按钮，62
  设置 Use In Control Scheme 为 Gamepad，63
Blackboard 面板，132
混合树
  关于，210
  创建，208
混合移动，208
Blender，.blend 文件作为 Unity 中的资源，19
光晕效果，在后处理中使用 Built-In Render Pipeline 设置，117
使用高动态范围（HDR）颜色，123
使用 Universal Render Pipeline，122
与溶解效果一起使用，142
blue_panel 纹理，337
弹跳动画，190
框，选择对象，251-255
  BoxSelectable 组件示例，251
BoxSelection 组件示例，251-255
BoxCollider，15
BoxCollider2D 组件，100
大脑（Cinemachine），219
Build Settings 视图，19，226
Built-In Render Pipeline，117
ButtonClicked 组件，333
  附加到 Canvas，333
按钮
  直接访问，57
  锚点配置，预设，335
  创建类型为 Button 的新操作，61

现代游戏手柄右侧的按钮，63
旋转按钮动画，339-342
ButtonWasClicked 方法，333
ButtonWasClickedWithParameter，333
按键位置，62

## C

C#，25
  命名类，26
摄像机
  Camera 对象，66
  电影式摄像机跟踪，216-219
  配置以便可以应用后处理效果，117
  创建俯视摄像机，233-235
  创建轨道摄像机，266-269
  创建不会穿过墙壁的轨道摄像机，269
  创建渲染纹理摄像机，156
  推拉摄像机，221-223
  保持多个对象在视野中，220
  透视，107
  选择并向摄像机添加 Post Process Layer 组件，120
  自动切换，219
  打开后处理，122
画布
  将 ButtonClicked 组件附加到 Canvas 对象，333
  Canvas 组件，332
  UI 元素作为子项添加，332
  Canvas Scaler 组件附加，335
  作为按钮的父级创建，340
  创建以绘制框，251
  创建以显示任务状态，237
  创建，EventSystem 组件和，65 理解，336
胶囊，286
CaptureScreenshot 方法，374
CaptureScreenshotAsTexture 方法，374
CaptureScreenshotAtEndOfFrame 方法，375
汽车（见车辆）
Cg 语言，128
CharacterCollider 组件，184
  推动刚体，185-187
CharacterController 组件，163
CharacterMovement 组件，196
检查点，271-276
子游戏对象，12
Cinemachine，216-219
  使用 ClearShot 摄像机，220
  创建轨道摄像机，221
  Look At Group，220
CinemachineTargetGroup 组件，220
ClearShot 摄像机，220
Click Paste Component As New，15
Collab 菜单，5
Collider2D 组件，100
碰撞器，68，93，175
  复合，96
  触发器，177
  轮碰撞器，259
碰撞
  自定义精灵碰撞形状，94-96
  检测对象是否接触另一个对象，175
颜色
  为立方体创建向导提供，371
  使用高动态范围（HDR）颜色，123，131
交换律，82
Component 菜单，15
  Event -> Physics Raycaster，66
  Miscellaneous -> Animator，190
组件
  创建脚本组件，26
  查找所需类型的组件，36-37
  使用 Scale 方法缩放，75
  使用游戏对象的组件，34-36
  工作，13-15
复合碰撞器，96
CompositeCollider2D 组件，96
ConstantForce2D 组件，100
传送带，创建，100
坐标，使用向量存储，72-77
协程
  CaptureScreenshotAtEndOfFrame，375
  Start 方法，349
  UnityWebRequestAsyncOperation，395
  使用以管理代码运行时间，40-44
两个向量之间角度的余弦，75，84
CreateAssetMenu 属性，52
CreateCubeWizard 组件示例，370-372
叉积（向量），76
立方体，添加到场景，13

