deploying a content delivery network (CDN) point of presence (PoP) in those regions to reduce latency. If your RUM shows that most users have good INP but a small percent have terrible INP, investigate the cause. If it’s due to old devices, consider dropping heavy effects for those devices, if possible.

Don’t forget to feed the findings back into your lab testing. If RUM says that your p75 LCP is 3 s but your lab test always shows 1.5 s, maybe your lab conditions are too optimistic. Adjust them to better match real conditions, perhaps increasing throttle or testing on slower CPUs. Then you can better reproduce those real-world issues in the lab.

# Setting Up Performance Budgets and Alerts

Once you have your RUM data flowing, establish your thresholds to aim for and create alerts for any regressions:

*Your CWVs budget*

Perhaps your CWV performance budget looks like this: “Our goals are: p75 LCP < 2.5 s on mobile, p75 INP < 200 ms, p75 CLS < 0.1.” You can set Search Console to alert you if any of these metrics go worse than the budgeted values so you can take action. For instance, if many URLs drop to “Poor,” it will flag an issue.

*Resource budgets*

Many teams enforce budgets outside the CWVs, such as that no page should ship more than X KB of JavaScript or CSS, or no image larger than Y. Use webpack-bundle-analyzer to check bundle sizes, and the Lighthouse CI can check “total bytes of JavaScript” (there’s also a Lighthouse audit for that).

*CI blocking*

As mentioned, integrate performance tests in CI. For example, run Lighthouse on a key page as part of the build. If the score or specific