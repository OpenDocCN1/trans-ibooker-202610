

显示出虚线边框代表有争议的领土。尽管 Google 声称要“在地缘政治争端上保持中立”，但据《华盛顿邮报》报道，该公司通过向俄罗斯观众显示实线边框，明显站队了。14 Google 和其他几家网络地图提供商已就印度和巴基斯坦之间有争议的边界、伊朗和沙特阿拉伯之间的水道，以及日本和韩国之间的海域采取了类似的行动。

普通人可以在 Google 地图和其他专有服务中识别出有争议领土的偏见，但我们很难直接挑战他们的决定或施压要求他们修改底图。但我们可以借鉴其他策略来减少这些偏见。例如，OpenStreetMap 的贡献者——这个众包全球地图——已积极讨论在平台上承认有争议领土的不同方法。此外，我们还可以使用数据可视化工具在专有地图图层之上绘制不同的边界。作为一个例子，由加拿大一家非营利组织创建的 Native Land 地图，在现今地图上显示原住民领土和语言的轮廓，以公开提醒我们殖民政策和强制流离失所。挑战单一的 Google 地图平台的一种方式是创建并宣传替代方案。

地图排除偏见是指我们通过遗漏行为未能代表人们或土地的方式。有时这些行为是由 Google 和其他专有地图提供商采取的，有时我们也是在创建地图的日常决策中做出的。仔细看看你最近制作的地图，问问自己它们是否真正代表了标题声称要展示的内容。例如，如果你制作了一张美国州级数据的地图，你是如何处理哥伦比亚特区的？美国首都并不算作一个州，也没有在美国国会拥有投票代表。但 DC 有超过 700,000 名居民（超过怀俄明州或佛蒙特州），并且美国宪法第二十三修正案授予其选举人票，就像它是一个州一样（尽管它永远不能超过人口最少的州）。同样，你的美国地图是如何代表波多黎各的，这个拥有超过三百万居民的领土，其居民是美国公民，但没有国会投票权或总统选举权？其他美国领土又如何，例如美属萨摩亚、关岛、北马里亚纳群岛和美属维尔京群岛，其居民也是美国公民？当这些地方存在数据时，你的地图是否让它们可见——还是它们消失了？如果是后者，那么你需要考虑你的遗漏行为是否也是一种族群偏见，因为 DC 和这些领土的大多数居民是黑人、拉丁裔和太平洋岛民。

要明确的是，一些数据可视化工具使得包含传统上被排除在地图之外的人和地方变得非常困难。但有时问题出在我们自己，或我们工具的默认设置以及我们是否要尝试改变它们的决定。再仔细看看你最喜欢的地图工具，并在你选择为“美国”绘制数据时，仔细检查出现的地理轮廓。如果你输入包含 DC 和美国领土的数据——但地图只显示 50 个公认的州——那么这种遗漏将从你的地图中抹去四百万美国公民的存在。超越默认设置，确定你的工具是否提供更具包容性的选项。例如，Datawrapper 最近改进了其 USA > States and Territories 地图选项如何显示符号点和等值区制图数据，如图 14-13 所示。对于 Datawrapper 选项中尚未出现的其他地区，你可以创建并上传自己的 GeoJSON 格式的地图边界文件，如“Geospatial Data and GeoJSON”所述。或者，如果你的工具迫使你省略你数据故事的一部分，那么通过在地图注释或配套文本中描述其缺失来指出这种偏见。我们在数据可视化中的使命是讲述真实而有意义的故事，因此请包含属于地图上的人和地方，而不是忽视他们的存在。

## 1 Select your map

Symbol map

What type of map do you want to create?

USA territories

USA » States and Territories (hexagons)

USA » States and Territories

+ Add column

| Lat | Lon | Title |
|---|---|---|
| 64.4459613 | -149.680909 | Alaska |
| -14.293304 | -170.692511 | American Samoa |
| 38.8937936 | -76.9879976 | District of Columbia |
| 13.4501257 | 144.757551 | Guam |
| 19.5872677 | -155.4268897 | Hawaii |
| 14.1490205 | 145.2134525 | Northern Mariana Islands |
| 18.2214149 | -66.4132818 | Puerto Rico |
| 17.7289564 | -64.759016 | US Virgin Islands |

![](img/216890131d46798dfe1cd5f886cc608c_448_0.png)

Figure 14-13. Datawrapper recently improved how it displays DC and noncontiguous places in its USA > States and Territories option for both symbol and choropleth maps.

## Summary

In this chapter, you learned how to distinguish between wrong, misleading, and truthful visualizations, and strengthened your lie-detector skills to understand the importance of being honest when telling your own data stories. You also learned how to recognize ways to reduce four categories of data bias in general, and spatial bias in particular.

The next chapter will bring together all of the concepts from different parts of the book to emphasize the importance of storytelling in our data visualizations.

1 The “how to lie” tutorials were inspired by several excellent works in data visualization: Cairo, The Truthful Art, 2016; Cairo, How Charts Lie, 2019; Darrell Huff, How to Lie with Statistics (W.W. Norton & Company, 1954); Mark Monmonier, How to Lie with Maps, 3rd edition (University of Chicago Press, 2018); Nathan Yau, “How to Spot Visualization Lies” (FlowingData, February 9, 2017), https://oreil.ly/o9PLq; NASA Jet Propulsion Laboratory (JPL), “Educator Guide: Graphing Global Temperature Trends,” 2017, https://oreil.ly/Gw-6z.

2 Note that not everyone agrees because GDP doesn’t count unpaid household labor such as caring for one’s children, nor does it consider the distribution of wealth across a nation’s population.

3 The tutorial on misleading climate change data was inspired by a high school classroom activity created by the NASA Jet Propulsion Laboratory (JPL), as well as Alberto Cairo’s analysis of charts by climate change deniers. NASA JPL; Cairo, How Charts Lie, 2019, pp. 65–67, 135–141.

4 Cairo, How Charts Lie, 2019, p. 61.

5 Cairo, *How Charts Lie*, 2019, p. 69.

6 Robert Kosara, “Aspect Ratio and Banking to 45 Degrees” (Eagereyes, June 3, 2013), https://oreil.ly/0KNUb.

7 Cairo, *How Charts Lie*, 2019, p. 70.

8 Cairo, *How Charts Lie*, 2019, p. 58.

9 Monmonier, *How to Lie with Maps*, 3rd edition, 2018.

10 David Spiegelhalter, *The Art of Statistics: Learning from Data* (Penguin UK, 2019), pp. 22–5

11 Julia Angwin et al., “Machine Bias” (ProPublica, May 23, 2016), https://oreil.ly/3Q6Em.

12 Neil Vigdor, “Apple Card Investigated After Gender Discrimination Complaints” (Published 2019), *The New York Times: Business*, November 10, 2019, https://oreil.ly/gs5lb.

13 Jonathan Schwabish and Alice Feng, “Applying Racial Equity Awareness in Data Visualization,” preprint (Open Science Framework, August 27, 2020), https://doi.org/10.31219/osf.io/x8tbw. See also this web post summary of the paper, Jonathan Schwabish and Alice Feng, “Applying Racial Equity Awareness in Data Visualization” (Medium), accessed October 16, 2020, https://oreil.ly/uMaI6, and Urban Institute, “Urban Institute Data Visualization Style Guide,” 2020, https://oreil.ly/_GRS2.

14 Greg Bensinger, “Google Redraws the Borders on Maps Depending on Who’s Looking,” Washington Post, February 14, 2020, https://oreil.ly/agLUY.

## Chapter 15. Tell and Show Your Data Story

For our concluding chapter, we’ll draw on knowledge and skills you’ve developed while reading this book and offer some final recommendations for creating true and meaningful data stories. Here we emphasize storytelling. The goal of data visualization is not simply to make pictures about numbers, but also to craft a truthful narrative that convinces readers how and why your interpretation matters.

Writers have a saying—“show, don’t tell”—which means to let readers experience a story through the actions and feelings of its characters, rather than narration by the author. We take a different stance, as shown in our chapter title: tell and show your data story. Make a regular habit of these three steps: tell your audience what you found that’s interesting in the data, show them the visual evidence to support your argument, and remind us why it matters. In three words: tell, show, why. Whatever you do, avoid the bad habit of showing lots of pictures and leaving it up to the audience to guess what it all means. We rely on you, the storyteller, to guide us on a journey through the data and highlight what aspects deserve our attention. Describe the forest, not every tree, but point out a few special trees as examples to help us understand how different parts of the forest stand out.

In this chapter, you’ll learn how to build visualizations into the narrative of the storyboard that we started in Part I. You’ll try out ways to draw attention to what’s most meaningful in your data through text and color in “Draw Attention to Meaning”. You will also learn how to acknowledge sources and uncertainty in “Acknowledge Sources and Uncertainty”. Finally, we’ll discuss decisions you’ll need to make about the format of your data story, with continual emphasis on sharing interactive visualizations rather than static images in “Decide on Your Data Story Format”.¹

## Build a Narrative on a Storyboard

Let’s return to the exercise from “Start Sketching Your Data Story”. We encouraged you to scribble words and sketch pictures on sheets of paper to lay out at least four initial elements of your story:

+   1. Identify the problem that motivates your project.
+   2. Reframe the problem into a researchable question.
+   3. Describe your plan to find data to answer the question.
+   4. Dream up one or more visualizations you might create using imaginary data.

Spread out these sheets like a storyboard to define the sequence of your narrative, as shown in Figure 15-1. Imagine them as preliminary slides for your presentation, or paragraphs and pictures for your written report or web page, for how you will explain the process to your audience. If you prefer to construct your storyboard digitally, another option is to convert blocks of text and images from your sheets into a Google Slides presentation or a draft Google Doc, or your preferred tools for telling the data story. Of course, it’s perfectly normal to update the sheets you created at the beginning of your project to reflect changes in your thinking. For example, you may have refined your research question, found new sources during your search, and of course, turned your imagined visualizations into actual tables, charts, or maps with real data.

