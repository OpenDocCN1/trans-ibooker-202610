
# VR/AR设计与存在感

给玩家观察四周的理由至关重要，因为这有助于建立存在感——大脑会构建空间的3D模型。新玩家起初不会倾向于这样做。但渐渐地，你可以引导他们。《I Expect You To Die》通过将玩家置于一个他们不期望自由移动的场所来实现这一点。如果玩家能向前导航，他们就会想看向前方。如果你让他们原地不动，他们最终会开始环顾四周。《I Expect You To Die》的一个关卡发生在一辆停着的车里。玩家坐在驾驶座上。起初，他们检查面前的东西——方向盘、油门和刹车踏板。当这些不起作用时，玩家逐渐开始探索车内：换挡杆、手套箱、副驾驶座，最终他们会想知道后座有什么。大多数玩家一旦开始探索车内，都会被世界的真实感震惊。这似乎是熟悉感与环顾四周共同作用建立了存在感。如果这是一款关于开车的游戏，我怀疑存在感不会来得这么快（尽管晕动症很可能会）。

不仅要给玩家观察四周的理由（以及有趣的观察和交互对象），还应明智地给玩家深入观察事物的机会。设计师出于乐于助人，倾向于让所有东西从玩家的起始点就清晰可见。但请记住位置追踪的力量。在我们的游戏中，我们给了玩家很多理由不仅转头，还要移动头部。多个手套箱、后座上的物品、细小文字、允许物体掉落到车地板上的物理效果，甚至一个要求你前倾的视网膜扫描系统（如果你不把头移开，它会尝试用激光杀死你），这些都是我们鼓励玩家在环境中移动头部的不同方式。

## Consider Brownboxing

大多数开发者明白，当你承担一个充满未知的游戏项目时，必须在日程中预留大量时间来应对发现和问题。VR和AR项目有很多未知数，必然需要实验和适应的时间。但有趣的是：既然你在尝试模拟现实，实际上可以通过在现实中构建体验来学到很多！Shawn Patton开创了“brownboxing”技术，即用一堆旧纸箱在现实世界中模拟VR场景，然后像任何纸面测试一样进行测试，让某人担任游戏主持人，告诉你当你触摸、拿起或操作不同物体时会发生什么。你不能那样测试所有东西，但你可以快速了解很多关于玩家如何想用手与场景交互、以及他们能触及什么。一旦你的brownbox迭代完成，测量它，交给建模师，你就会有一个优秀的第一个“whiteboxed”数字原型基础。

**FIGURE 22.3**

![](`images/fig22_3.jpg`)

Shawn Patton和Matt Mahon测试《I Expect You To Die》的潜艇关卡。

## Different Hardware Enables Different Experiences

你使用的VR和AR输入设备在很大程度上定义了你创造的体验。仅使用游戏手柄的系统与使用运动追踪手部控制器的系统截然不同，后者又与追踪空手的系统截然不同。要求玩家静止站立的系统与给予玩家自由走动的系统创造的体验也大相径庭。此外，处理能力、音频传递方式等许多其他因素也存在根本差异。

极少有VR体验能轻松移植到各种不同的VR输入系统上。因此，如果你想创造一个伟大的VR体验，应该首先选择计划使用的输入系统，并围绕它设计游戏。是的，这意味着你的游戏可能难以移植到其他平台，但它将在你选择的平台上变得*卓越*。

还记得透明度之镜吗？这里有一个很好的搭档。

在撰写本章时（2019年），VR和AR系统才刚开始被视为严肃的游戏平台。写这一章我冒着风险，几年内这里说的一切可能都会显得愚蠢。然而，我决定分享这些想法，因为虽然VR和AR系统不太可能成为主导的视频游戏硬件形式，但它们提供的强大存在感体验让我确信，它们将在游戏设计世界中拥有永久且重要的地位。尽管它们的形式和技术将快速演变和改变，但我有信心这些平台上存在感的重要性将保持不变。

鉴于存在感深受体验的外观和感觉影响，这可能是谈论美学的好时机。

# CHAPTER TWENTY-THREE

# The Look and Feel of a World Is Defined by Its *Aesthetics*

**FIGURE 23.1**

![](`images/fig23_1.jpg`)

## Monet Refuses the Operation

> *Doctor, you say that there are no haloes*
>
> *around the streetlights in Paris*
>
> *and what I see is an aberration*
>
> *caused by old age, an affliction.*
>
> *I tell you it has taken me all my life*
>
> *to arrive at the vision of gas lamps as angels,*
>
> *to soften and blur and finally banish*
>
> *the edges you regret I don't see,*
>
> *to learn that the line I called the horizon*
>
> *does not exist and sky and water,*
>
> *so long apart, are the same state of being.*
>
> *Fifty-four years before I could see*
>
> *Rouen cathedral is built*
>
> *of parallel shafts of sun,*
>
> *and now you want to restore*
>
> *my youthful errors: fixed*
>
> *notions of top and bottom,*
>
> *the illusion of three-dimensional space,*
>
> *wisteria separate*
>
> *from the bridge it covers.*
>
> *What can I say to convince you*
>
> *the Houses of Parliament dissolve*
>
> *night after night to become*
>
> *the fluid dream of the Thames?*
>
> *I will not return to a universe*
>
> *of objects that don't know each other,*
>
> *as if islands were not the lost children*
>
> *of one great continent. The world*
>
> *is flux, and light becomes what it touches,*
>
> *becomes water, lilies on water,*
>
> *above and below water,*
>
> *becomes lilac and mauve and yellow*
>
> *and white and cerulean lamps,*
>
> *small fists passing sunlight*
>
> *so quickly to one another*
>
> *that it would take long, streaming hair*
>
> *inside my brush to catch it.*
>
> *To paint the speed of light!*
>
> *Our weighted shapes, these verticals,*
>
> *burn to mix with air*
>
> *and changes our bones, skin, clothes*
>
> *to gases. Doctor,*
>
> *if only you could see*
>
> *how heaven pulls earth into its arms*
>
> *and how infinitely the heart expands*
>
> *to claim this world, blue vapor without end.*
>
> —Lisel Mueller

## The Value of Aesthetics

美学是元素四象限的第三象限。一些游戏设计师鄙视游戏中的美学考量，称它们为与他们所认为重要的——游戏机制——无关的单纯“表面细节”。但我们必须永远记住，我们设计的不仅仅是游戏机制，而是完整的体验。而美学考量是让任何体验更愉悦的一部分。优秀的美术能为游戏做奇妙的事情：

*   它能吸引玩家进入他们可能会错过的游戏。
