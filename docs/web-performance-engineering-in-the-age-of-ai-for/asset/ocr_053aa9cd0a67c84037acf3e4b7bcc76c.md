+   - Specific items like the LCP element, what its load time was, and which resource caused it

Use Lighthouse primarily to see your metric values and to catch obvious problems. Don’t obsess over the single score. Trying to optimize for a perfect 100 can lead to diminishing returns or unnatural patterns. For instance, Lighthouse might say that you “reduced unused CSS,” which is good, but splitting the Cascading Style Sheets (CSS) too much might actually hurt real users by causing extra requests.

Running Lighthouse before and after making changes is a good way to verify that your changes achieved the expected improvements. Say you decide to optimize your page’s images by compressing them and adding dimensions. If you run Lighthouse before the changes and then afterward, you might expect to see results like the following:

+   - CLS should drop to around 0 if images were the cause. Lighthouse might drop the “Image elements have no explicit width/height” warning.

+   - LCP might improve if images are loading faster due to compression.

+   - TBT or INP might not change unless you also remove heavy JavaScript. In that case, TBT should go down.

Lighthouse also has a tool to integrate in your organization’s CI/CD pipeline. It can alert you if a performance score or specific metric regresses beyond a certain threshold. You might set it up to run on a representative page after each commit. It has also added user flows, so you can script what a user might do and then let it play out.

In summary, Lighthouse is your quick audit tool to get a broad sense of page performance and some guidance on what to fix. It’s often the first thing you’ll run on a new site.