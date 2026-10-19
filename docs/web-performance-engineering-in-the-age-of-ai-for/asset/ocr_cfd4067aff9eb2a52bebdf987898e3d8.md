The rest of this chapter takes a closer look at how to use specific lab and field measurement tools.

# Lab Tools

Lab tests are reproducible and let you test changes quickly. They complement field data by allowing deeper debugging. The goal of this section is to show you how to use these tools to catch regressions and verify improvements. Each tool has its nuances. All of the tools covered here can emulate mobile devices and apply network throttling to approximate real user conditions. Your results might vary from run to run, especially LCP (due to variability in network timing) and TBT (due to simulation). Always run a tool a few times, or do three runs and report the median scores. Your environment can also cause variation. DevTools and Lighthouse use your machine, but they might throttle it; PageSpeed Insights uses a datacenter.

Let’s dive into how to make the most of each.

# Lighthouse: Performance Auditing

Lighthouse is a popular open source tool for performance auditing, developed by the Google Chrome team. Lighthouse loads your page in a controlled environment according to the constraints of the device and network you specify, and collects performance metrics. It then gives a performance score on a scale of 0 to 100 (higher is better), which is a weighted composite of these metrics. Lighthouse focuses primarily on page load performance.

To use Lighthouse effectively, run it on critical pages and examine the opportunities for improvement it lists, like “reduce unused JS” or “enable text compression.” These are valuable hints about what might be slowing the load. Lighthouse can guide you to where the big problems lie, like large images, slow server response, or heavy scripting. It also flags your CWVs in lab form. It is integrated into Chrome DevTools (go to Audits or the “Lighthouse” panel) and is also available via the command-line interface