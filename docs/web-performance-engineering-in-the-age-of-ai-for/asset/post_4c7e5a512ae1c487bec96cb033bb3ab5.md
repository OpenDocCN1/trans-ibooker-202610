
every time things jump on screen. Layout instability can cause serious frustration and even accidental clicks on the wrong element.

CLS is calculated simply: it looks at the portion of the screen that shifted and how far it moved, and tallies a score. Small movements or shifts of off-screen elements barely count, but big, visible jumps yield a higher score. The goal is to have a CLS score of less than 0.1 for 75% of page loads. In practice, that means almost no noticeable unexpected shifts. If you meet this goal, your page is likely very stable and user-friendly in terms of visual consistency. If it’s more than 0.1, look at the elements to find and fix the cause. Does Lighthouse list the top shifting elements? Does the DevTools timeline or WebPageTest’s session filmstrip show any janky animation?

The CWVs distill Google’s advice on the most critical UX aspects of performance. Understanding them will help you prioritize what to fix or optimize.

## Other Important Metrics

Other important metrics that aren’t part of the CWVs include:

### *Total blocking time (TBT)*

A lab-only metric summing how long tasks >50 ms run between FCP and TTI. It’s a proxy for how busy the main thread is. If it’s high, INP might also be an issue. You can lower TBT by optimizing JavaScript (see Chapter 7). For example, if your TBT is 600 ms, you probably have a big chunk of script blocking the main thread.

### *Speed index (SI)*

This somewhat arcane metric measures how quickly content is visually populated (using a filmstrip tool). It’s not a Core Web Vital, but is part of the Lighthouse score. If LCP is fine, usually SI is fine too. If SI is high, you likely have a progressive rendering issue (the content starts appearing late).

### *Time to interactive (TTI)*

This measures the time until the page is idle enough to reliably respond. If you optimize TBT and JavaScript, TTI will drop. TTI sometimes overshoots LCP if you have a lot of postload JavaScript. If TTI is much after LCP, it means the user sees the content but can’t interact fully with it until later—which might correlate with heavy JavaScript tying things up.

## Interpreting and Acting on Reports

If Lighthouse says your site’s interactivity is high (say, eight seconds), that likely means users see content but can’t use it for another eight seconds: an obvious UX issue. If CLS is flagged as “needs improvement,” hunt down which elements are causing layout jumps. If your tool doesn’t tell you, maybe it’s an image without dimensions or an ad slot.

Consider each metric and consider it from the user’s point of view. Do the measurements you’re getting align with feedback or session recordings, if available? “I clicked but nothing happened” is a clue indicating interactivity problems; “the page jumps around” tells you to look into CLS issues.

Consider closing the loop: measure, find issues, fix them, then measure again. Performance work is iterative. One team set up a performance budget and discovered they had exceeded it when marketing added a huge third-party script. Thanks to monitoring, they caught it and worked with marketing to load it differently, with no regression in UX metrics. By making performance metrics as much a part of your monitoring as error logs or uptime, your team can keep UX at the forefront. On the Chrome team and other teams at Google, we treat certain performance metrics as key success criteria. At Chrome itself, we constantly track how fast pages load and respond, because any regression could hurt our billions of users.

In summary, measure what you treasure. Performance is crucial; you now know how to keep an eye on it in your day-to-day development and in production. By combining lab and field results and integrating performance checks into your regular workflow, you can make performance sustainable and actionable.

The rest of this chapter takes a closer look at how to use specific lab and field measurement tools.

## Lab Tools

Lab tests are reproducible and let you test changes quickly. They complement field data by allowing deeper debugging. The goal of this section is to show you how to use these tools to catch regressions and verify improvements. Each tool has its nuances. All of the tools covered here can emulate mobile devices and apply network throttling to approximate real user conditions. Your results might vary from run to run, especially LCP (due to variability in network timing) and TBT (due to simulation). Always run a tool a few times, or do three runs and report the median scores. Your environment can also cause variation. DevTools and Lighthouse use your machine, but they might throttle it; PageSpeed Insights uses a datacenter.

Let’s dive into how to make the most of each.

## Lighthouse: Performance Auditing

Lighthouse is a popular open source tool for performance auditing, developed by the Google Chrome team. Lighthouse loads your page in a controlled environment according to the constraints of the device and network you specify, and collects performance metrics. It then gives a performance score on a scale of 0 to 100 (higher is better), which is a weighted composite of these metrics. Lighthouse focuses primarily on page load performance.

To use Lighthouse effectively, run it on critical pages and examine the opportunities for improvement it lists, like “reduce unused JS” or “enable text compression.” These are valuable hints about what might be slowing the load. Lighthouse can guide you to where the big problems lie, like large images, slow server response, or heavy scripting. It also flags your CWVs in lab form. It is integrated into Chrome DevTools (go to Audits or the “Lighthouse” panel) and is also available via the command-line interface

- Specific items like the LCP element, what its load time was, and which resource caused it

Use Lighthouse primarily to see your metric values and to catch obvious problems. Don’t obsess over the single score. Trying to optimize for a perfect 100 can lead to diminishing returns or unnatural patterns. For instance, Lighthouse might say that you “reduced unused CSS,” which is good, but splitting the Cascading Style Sheets (CSS) too much might actually hurt real users by causing extra requests.

Running Lighthouse before and after making changes is a good way to verify that your changes achieved the expected improvements. Say you decide to optimize your page’s images by compressing them and adding dimensions. If you run Lighthouse before the changes and then afterward, you might expect to see results like the following:

- CLS should drop to around 0 if images were the cause. Lighthouse might drop the “Image elements have no explicit width/height” warning.
- LCP might improve if images are loading faster due to compression.
- TBT or INP might not change unless you also remove heavy JavaScript. In that case, TBT should go down.

Lighthouse also has a tool to integrate in your organization’s CI/CD pipeline. It can alert you if a performance score or specific metric regresses beyond a certain threshold. You might set it up to run on a representative page after each commit. It has also added user flows, so you can script what a user might do and then let it play out.

In summary, Lighthouse is your quick audit tool to get a broad sense of page performance and some guidance on what to fix. It’s often the first thing you’ll run on a new site.

warnings in plain language. For instance, if your AI-generated code throws a warning like “[Deprecation] Some API is deprecated,” the console’s “Explain” feature can use AI to tell you what that means and how to update it. This is a huge boon when dealing with unfamiliar output from an AI: sometimes AI uses an outdated approach it saw in its training data, and DevTools might warn about it, so you know to modernize that bit of code.

![](img/f68eedc514234fb0520cb1fa5894630d_38_0.png)

Figure 2-1. The Chrome DevTools Performance panel now includes an AI assistant (Gemini) that can analyze a recorded profile

To use these features, ensure you’re on a recent version of Chrome, or at least a Chrome DevTools version that includes Gemini (Chrome 111+ started introducing these AI tools in preview). For performance, the workflow is:
