## 其他风格指南

我们强烈建议你制定一份风格指南并使用它！如果你不喜欢我们的，并且你也不想自己修改，你可以从别处偷一个：

- The Google Shell Style Guide

虽然我们对其中一些有不同的看法和分歧，但它非常好且相当全面。最好的情况是，你可以直接使用它，最坏的情况是，它是一个起点。有很多值得喜欢的地方，你可以做得更差（比如什么都没有），而且很多其他项目都采用了它，所以……

除其他事项外，我们不同意：

- 百行限制。我们看到了这一点，尤其是在 Google 这样的环境中，但我们有很多超过该限制的脚本，它们“主要是调用其他实用程序并且数据操作相对较少。”
- “缩进两个空格”对我们来说有点浅；我们喜欢对续行使用两个空格，其他情况使用四个空格。
- “${var}”太忙了。我们在可能的情况下更喜欢 "$var"。

— function cleanup() {? Ouch, no.

— ;; 关闭 case..esac 块，应与块的开头保持相同的缩进，而不是与块中的代码保持相同的缩进。

- 以下还有其他你可以参考的指南：
  - https://linuxcommand.org/lc3_adv_standards.php
  - https://www.ovirt.org/develop/infra/infra-bash-style-guide.html
  - https://wiki.bash-hackers.org/scripting/style
  - http://mywiki.wooledg.org/BashGuide/Practices
    - Great info: http://mywiki.wooledg.org/BashPitfalls

- 或在网上搜索：
  - "Shell Style Guide"
  - "Bash Style Guide"
  - "Shell script coding standards"
