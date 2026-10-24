increase, which is the same as 33/100 or one-third, he recommends an aspect ratio of 3:1 (because the fraction is flipped by placing width before height), or in other words, a line chart that is three times wider than its height.⁵

![](bbox=[0.15, 0.05, 0.88, 0.3])

Figure 14-2. Misleading chart with a stretched aspect ratio.

# NOTE

Some experts advise that aspect ratios for line charts should follow the banking to 45 degrees principle, which states that the average orientation of line segments should be equal to an angle of 45°, upward or downward, to distinguish individual segments. But this requires statistical software to calculate slopes for all of the lines, and still isn’t a rule that fits all cases. Read “Aspect Ratio and Banking to 45 Degrees,” a good overview by Robert Kosara.⁶

Cairo does not propose his aspect ratio recommendation as a universal rule because he recognizes how it fails with very small or very large values. For example, if we apply Cairo’s recommendation to our global temperature change chart, the difference between the lowest and highest values (−0.5° to 1°C) represents a 300% increase. In this case, we calculate the percent change using the lowest value of −0.5°C, rather than the initial value of 0°C, because dividing by zero is not defined, so (1°C − (−0.5°C)) / |−0.5°C| = 3 = 300%. Following Cairo’s general recommendation, a 300% increase suggests a 1:3 aspect ratio, or a line chart three times taller than its width, as shown in Figure 14-3. While this very tall chart is technically correct, it’s misleading because it exaggerates change, which is contrary to Cairo’s main message. The aspect ratio recommendation becomes ridiculous when we divide by numbers that are very close to zero.