

![](img/b56145aa9b751926a7aedbd7172bc818_148_0.png)

Figure 5.16 Styled cards

## 5.5 Transition on hover and focus-within

首先我们需要创建一个媒体查询，用于检查设备是否支持悬停交互、浏览器窗口宽度至少为 700px，并且用户在设备上未启用 prefers-reduced-motion。

### Prefers reduced motion

用户可能希望退出运动密集的动画。他们可以通过在设备上启用设置来实现，这会通过 prefers-reduced-motion 属性传达给浏览器。我们需要确保尊重用户的设置，因此在确定是否对内容进行动画的查询中，我们将包含该设置未被设置的情况，即值为 no-preference。有关 prefers-reduced-motion 的更多信息，请查看第 3 章中关于可访问性和 reduced motion 查询的部分。

因此我们的媒体查询如下所示： @media (hover: hover) and (min-width: 700px) and (prefers-reduced-motion: no-preference) { }。请注意，我们可以链接多个需要满足的参数，以便查询中的 CSS 被应用。

为了隐藏除标题之外的所有内容，我们将使用 transform 属性将内容向下移动到卡片底部，值为 translateY()。TranslateY() 允许我们在不影响页面流的情况下垂直移动内容，这意味着被移动元素的周围内容完全不受移动影响，不会重新定位或让出位置。

为了计算元素需要移动的距离，我们将再次使用 calc() 函数。我们将标题向下移动卡片的高度 350px 减去 8rem（容器的顶部边距 + 容器的 padding-top + 标题的大小）。

因此卡片的内部部分被向下移动，如图 5.17 所示。

![](img/b56145aa9b751926a7aedbd7172bc818_149_0.png)

Figure 5.17 Moving the content down

由于我们将在用户将悬停移离该部分时为显示内容设置动画，我们不希望底部剩余的内容保留，因为如果用户将悬停在溢出图片的内容上，内容会向上移动到图片中，失去悬停然后再次向下移动。这种行为会重复造成闪烁。因此我们将内部容器的高度设置为 5rem，并在段落和链接被隐藏时隐藏溢出。

请注意，在第二张卡片中，当内容应被隐藏时，段落内容仍会有一点可见。因此我们还将使用不透明度隐藏非标题内容，当它不应被看到时。此外，我们使用 translateY 将该内容向下移动 1rem，这将允许我们在悬停时将其重新动画化时给予一点运动。

总的来说，用于隐藏内容并缩短内部容器的 CSS 如下所示（Listing 5.15）。为了选择所有不是标题的内容，我们可以使用 :not() 伪类。

### Listing 5.15 Hiding the non header content

```
@media (hover: hover) and (min-width: 700px) and (prefers-reduced-motion: no-preference) {
    #A
    main > section > div {
        transform: translateY(calc(350px - 8rem));
        height: 5rem;
        overflow: hidden;
    }
    main > section > div > *:not(h2) {
        opacity: 0;
        transform: translateY(1rem);
    }
}
```

#A The media query
#B Moving and shortening the inner content container
#C Hiding all of the non H2 content

not() 伪类允许我们过滤选择器。在这种情况下，我们想要定位任何不是 h2 的元素。图 5.18 描述了该过程。

![](img/b56145aa9b751926a7aedbd7172bc818_150_0.png)

### Figure 5.18 Selecting anything that is not an h2 inside of the inner container

现在内容被隐藏了（图 5.19），我们可以专注于再次显示它。

![](img/b56145aa9b751926a7aedbd7172bc818_151_0.png)

### Figure 5.19 Hiding content

为了再次显示内容，我们需要在悬停和获得焦点时撤销我们为隐藏它所做的一切。由于我们没有从 DOM 中移除链接，它们只是视觉上被隐藏。程序上它们仍然存在，用户可以通过键盘 Tab 到链接。因此我们需要在用户悬停在卡片上时以及链接获得焦点时显示内容。因为我们想要在子元素（链接）获得焦点时对祖先（内容容器）执行操作，我们可以使用 :focus-within 伪类。该伪类允许我们根据元素的后代是否当前处于焦点来有条件地应用样式。

因此，无论链接处于焦点还是部分被悬停，我们都通过将 translateY() 参数设置为 0（无垂直位移），并将内部容器的高度设置为 350px（外部容器的高度）减去 4rem（容器的垂直内边距和边距总和）来将容器移回原位。

我们还需要恢复不透明度被设置为 0 并且被向下移动 1rem 的段落和链接。

最后，我们通过向正在显示和隐藏的元素添加 transition 来完成我们的悬停和 focus-within 效果。由于我们预定义了在它们之间更改的状态，并且我们只希望动画在更改发生时运行一次，我们不需要使用关键帧，我们可以简单地指示 CSS 使用 transition 属性对发生时的所有更改进行动画处理，我们为其分配的值为 all 700ms ease-in-out。这将使所有更改都被动画化，动画完成需要 700 毫秒，并且它会从慢开始，加速，然后在完成前减速。

因此我们的悬停和 focus-within CSS 如下所示（Listing 5.16）。

### Listing 5.16 Showing content on hover and on focus-within

```
@media (hover: hover) and (min-width: 700px) and (prefers-reduced-motion: no-preference) {
  main > section > div {
    transform: translateY(calc(350px - 8rem));
    height: 5rem;
    overflow: hidden;
    transition: all 700ms ease-in-out; #E
  }

  div > *:not(h2) {
    opacity: 0;
    transform: translateY(1rem);
    transition: all 700ms ease-in-out; #E
  }

  section:hover div,      #A
  section:focus-within div {      #B
    transform: translateY(0);
    height: calc(350px - 4rem);
  }

  section:hover div > *:not(h2),      #C
  section:focus-within div > *:not(h2){      #D
    opacity: 1;
    transform: translateY(0);
  }
}
```

#A on section hover move container back into place

#B on section focus-within move container back into place

#C on hover move all non h2 elements inside the container back into place with full opacity

#D on section focus-within move all non h2 elements inside the container back into place with full opacity

#E Animating the changes

应用这些更改后（图 5.20），剩下的就是为我们的页面设置背景，我们的项目就完成了。

### HOTELS

![](img/b56145aa9b751926a7aedbd7172bc818_153_0.png)

Figure 5.20 Hover and focus-within effect

为了让图片更突出，我们将为整个页面添加一个非常深的灰色几乎是黑色的背景，从而完成项目。

为了应用背景颜色，我们将 background 属性的值设置为 #010101 添加到我们现有的 body 规则中（Listing 5.17）。

### Listing 5.17 Adding the background

```
body {
  display: grid;
  place-items: center;
  margin-top: 40px;
  padding: 20px;
  background: #010101;
}
```

图 5.21、5.22 和 5.23 显示了我们在不同屏幕尺寸下的最终项目。

![](img/b56145aa9b751926a7aedbd7172bc818_154_0.png)

Figure 5.21 Project at window width 600px

Figure 5.22 Project at window width 850px

Figure 5.23 Project at window width 1310px with prefers-reduced-motion enabled

## 5.6 Summary

- 我们使用 grid 属性作为水平布局卡片的方式。通过同时使用媒体查询和 grid-template-columns，我们根据最小浏览器窗口大小调整布局，如果最小宽度为 950px 则为 4 列宽，如果在 700px 到 950px 之间则设置为 2 列，其他情况则默认为堆叠状态
- `text-transform` 属性可以在不影响内容可访问性的情况下视觉上将文本更改为大写；复制内容时不会全部为大写
-  sparingly 使用 `text-transform: uppercase` 并且不要在大面积内容上使用
- `background-clip` 属性的值为 text 可以将背景图像裁剪在文本周围
- `background-clip` 属性仍需要前缀，并且该属性在实现过程中可能会发生变化
- 我们可以使用媒体查询检查设备是否支持悬停，并调整布局以防止用户在设备不支持悬停时看到内容
- 我们可以使用 `and` 在同一个媒体查询上链接多个条件
- 我们可以在媒体查询中使用 `prefers-reduced-motion` 来尊重用户对动画和运动的偏好。
- `:not()` 伪类表示与选择器列表不匹配的元素。
- `translateY()` 会在不影响重排的情况下垂直移动内容
- 我们可以使用 `transition` 属性在状态之间对样式更改进行动画处理
- 为了根据元素的后代是否处于焦点来有条件地应用样式，我们使用 `focus-within` 伪类

