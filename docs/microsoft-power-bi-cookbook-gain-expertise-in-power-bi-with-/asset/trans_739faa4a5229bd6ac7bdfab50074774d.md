

本章其他配方中包含了本示例中使用的各个报表组件的更多详细信息。

## There's more...

与 *Chapter 3, Building a Power BI Data Model* 中分享的数据建模简单却重要的细节类似，若干设计实践显著提升了 Power BI 报表的价值和用户采用率：

- **Minimalism:** 任何对理解并非严格必需的报表元素都应移除。示例包括图像、冗余图表轴、冗长文本和不必要的数据图例。
- **Efficient visuals:** 利用在相同空间内提供额外洞察的可视化对象和功能；示例包括 KPI 和组合图表可视化、工具提示、趋势线和颜色饱和度。
- **Meaningful colors:** 颜色应用于传达含义，例如相对于目标或度量的高或低差异。应选择性地使用颜色，避免因颜色种类或密度过多而使用户感到不知所措或分心。
- **Organized:** 报表可视化应保持对齐、分布均匀，并位于相关可视化附近。最重要的可视化应位于画布左上角，空白应分隔可视化。
- **Consistent:** 布局和格式选择，例如可视化放置、字体和文本对齐应保持一致。例如， slicer 可视化应始终放置在可视化的左侧或顶部。

关于一致性，Power BI 支持包含标准调色板、字体、字体大小以及控制报表和可视化默认值的其他元数据元素的报表主题。Power BI 报表默认设置的几乎每个方面都可被控制，包括轴标题是否显示、标签方向、图例标记，以及页面和可视化在 **Format** 选项卡中出现的几乎所有内容。

许多组织将拥有标准报表主题，甚至标准报表布局。通常，这些主题反映了企业品牌和身份。在本示例中，对报表应用了报表主题，以便色觉缺陷的用户能够清晰地理解：

- 从报表主题库下载 **Color Blind Friendly** 主题 https://aka.ms/pbithemes。 使用 ColorblindSafe-Longer.json 文件。
- 在 Power BI Desktop 中打开 CH4_Start.pbix 并将此文件另存为 CH4_R1.pbix。

在 **View** 选项卡的 ribbon 中，单击 **Themes** 区域中的下拉箭头并选择 **Browse for themes**：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_202_0.png)

浏览到下载的 JSON 文件，选择它并单击 **Open**。 将出现一条消息，说明主题已成功导入。 主题设置将应用于报表。 现在用于格式化可视化的颜色反映了主题的颜色。 保存 CH4_R1.pbix。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_202_1.png)

主题无法在 Power BI 服务中应用，并且某些自定义可视化不应用报表主题。 此外，报表主题不影响图像的颜色，也不覆盖对可视化中特定数据点应用的任何自定义颜色。 在开发报表时尽早应用主题非常重要，以避免在应用主题后花费数小时的格式设置突然被撤销的情况！

每个报表主题 JSON 文件都具有与 *Figure 4.6* 所示一致的颜色代码映射元素结构。

```json
{
  "name": "ColorBlindSafe-Longer",
  "dataColors": ["#074650", "#009292", "#edc948", "#eb6ba4", "#490091", "#b5d6ff", "#b3dafe", "#6db6ff", "#914800", "#23cfd3"],
  "background": "#FFFFFF",
  "foreground": "#074650",
  "tableAccent": "#f2e6db"
}
```

JSON 文件中仅 **Name** 字段是必需的。可以在 dataColors 字段中输入任意数量的不同代码。

## See also

- ▶ Visualizations in Power BI reports: http://bit.ly/2poUeMv
- ▶ Tips For Power BI Report Design – Best Practices: http://bit.ly/3fmFXPf
- ▶ Power BI Best Practices Guide: http://bit.ly/3cxCs29
- ▶ Power BI Community Themes Gallery: https://aka.ms/pbithmes
- ▶ Use report themes in Power BI Desktop: http://bit.ly/31vBixz
- ▶ Theme generator: https://themes.powerbi.tips/

## Filtering at Different Scopes

过滤器可针对 Power BI 报表的每个架构级别进行配置，包括整个报表、报表页面和单个可视化。由于报表过滤器在设计时配置且不像切片器那样在画布上公开，因此过滤器提供了一种强大而高效的方法，可根据特定需求自定义报表元素。 与访问报表的用户或团队或要分析的特定实体相一致的报表级和页面级过滤器可提供即时关注和一定程度的组织。例如，为一个产品类别构建的报表页面可以为其他产品类别页面复制，每个页面包含不同的页面级过滤器。可视化级过滤器提供更大的灵活性，因为除了报表级和页面级过滤器之外，还可以定义包括度量在内的复杂过滤条件。

在本配方中，提供了在三个不同范围实现过滤器的示例。本节演示了 Top N 可视化级过滤条件，并在 How it works 中分享了过滤器类型生成的 DAX 查询示例。

### Getting ready

为准备本配方，请执行以下步骤：

- 1. 打开 CH4_R1.pbix 文件并将其另存为 CH4_R2.pbix。
- 2. 右键单击页面选项卡并选择 Rename Page，将 Page 1 重命名为 Europe：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_203_0.png)

Figure 4.7: Page renaming and creation

### Authoring Power BI Reports

- 3. 使用 **+** 图标创建四个额外的报表页面，命名为 **France**、**Germany** 和 **United Kingdom**，以及 **Scratch**：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_204_0.png)

Figure 4.8: Report pages

- 4. 保存您的工作。

### How to Filter at Different Scopes

要实现本配方，请使用以下步骤：

- 1. 切换到 **Europe** 页面。
- 2. 在 **Fields** 窗格中，将 **Sales Territories** 表中的 **Sales Territory Group** 列拖到 **Filters** 窗格中的 **Filters on all pages** 区域。
- 3. 使用 **Basic filtering** 选择 **Europe**。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_204_1.png)

Figure 4.9: Report-level filter for European Sales and Margin Monthly Report

- 4. 同样地，在 **France**、**Germany** 和 **United Kingdom** 的国家页面上，将 **Sales Territories** 表中的 **Sales Territory Country** 列从 **Fields** 窗格拖到 **Filters** 窗格中的 **Filters on this page** 区域，并从 **Basic filtering** 中为页面选择相应的国家。
- 5. 现在选择 **Scratch** 页面。
- 6. 通过从 **Visualizations** 窗格中选择 **Table visual** 创建 **Table visualization**，然后从 **Fields** 窗格中添加 **Sales Territories** 表中的 **Sales Territory Country** 和 **Internet Sales** 表中的 **Internet Gross Sales** 度量。
- 7. 选择 **Table visualization**，并在 **Filters** 窗格中展开 **Filters on this visual** 部分中的 **Internet Gross Sales** 区域。
- 8. 从 **Show items when the value** 下的下拉菜单中选择 **is greater than**，并在下拉菜单下方的字段中输入 `3000000`。
- 9. 单击 **Apply filter** 将过滤器应用于可视化。

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_205_0.png)

Figure 4.10: Visual-level filter

### How it works

报表级过滤器会影响报表中的所有页面；因此，当过滤器应用于 **Filters** 窗格的 **Filters on all pages** 区域时，只要模型中存在维度关系且 DAX 未覆盖过滤器，所有页面上的所有可视化都会遵守此过滤器。对于上一配方中描述的报表，已确定欧洲团队无需查看其他销售组或国家，因此我们对整个报表（所有页面）按 **Sales Territory Group of Europe** 进行过滤。同样，页面级过滤器仅影响其应用的页面上的可视化。因此，我们将每个国家页面过滤为适当的 **Sales Territory Country**。

报表级和页面级过滤器最常在维度层次结构的相对静态且唯一值较少的不同级别实现，例如本配方中的示例。日期范围过滤器通常应应用于数据检索查询或报表画布中，使用日期切片器等可视化。由于这些过滤器分别应用于报表或页面中的所有可视化，因此应避免对高基数列（如 **ProductKey** 或 **Sales Order Number**）进行过滤。DAX 度量不能用于定义报表级和页面级过滤器。

可视化级过滤器可以基于数据模型中的度量和列。这些度量和列可能存在于可视化中，也可能不存在。

