the team, and maximizing the forward value. However, a list of people invited to them is often limited to technical groups. Much like development cycle communication, retrospectives are perceived as a technology-only feedback loop. But if we agree that technology’s purpose is to support business, the goal of *understanding the impact of change*, as technical as it might be, becomes impossible without the actual understanding of the impact an event can have on business. Inviting the people who possess domain knowledge to educate the rest of the team is valuable not only from an educational perspective, but also from an angle of framing prioritized action items that would provide maximum business value gain. Additionally, collaborative retrospective is a great tool for encouraging collaboration and building a strong culture that increases transparency and builds trust within and between teams.

# TIP

Conduct retrospectives even after events with a positive outcome. There is valuable information to be shared and learned for all of the groups, regardless of the type of outcome.

Let’s examine a common event: a latency spike. An alert came in because something happened, unbeknown to the operations group, that caused degraded performance for users. The team responded quickly and kept the outage to a minimum. All in all, Mean Time To Recovery (MTTR) was commendable. Nonetheless, an outage is an event that deserves a retrospective.

Based on the telemetry that was collected, it looks like there was a period of about 20 minutes during which users’ experience was degraded or they were not able to interact with any website features at all, due to connection timeouts caused by latency in content servicing (Figure 5-3). That’s a sizable outage. However, after reviewing the overall incident rate and performance trends, it appeared that overall performance over the longer interval is well within the 99th percentile (a measurable KPI for the group), which meant that its Service-Level Agreements (SLAs) were not violated, which, in turn, meant that although there should be improvements made to flatten the spikes going forward, it is not considered a major outage requiring prioritization of effort.

![](bbox=[0.07, 0.6, 0.95, 0.8])

Figure 5-3. A chart showing the spike in degraded user experience due to latency in content servicing

From a technical perspective, the outcome of the retrospective is viable. The team validated the current state of the system against SLAs and adequately prioritized resulting tasks based on the information they had. However, a retrospective is another decision point for which context is important.