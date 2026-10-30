Expressing that with an SLI means qualifying events and then determining whether they meet our conditions. In this example, your SLI would do the following:

+   - Look for any event with a request path of /home.
+   - Screen qualifying events for conditions in which the event duration < 100 ms.
+   - If the event duration < 100 ms and was served successfully, consider it OK.
+   - If the event duration > 100 ms, consider that event an error even if it returned a success code.

Any event that is an error would spend some of the error budget allowed in your SLO. We’ll closely examine patterns for proactively managing SLO error budgets and triggering alerts in the next chapter. For now, we’ll summarize by saying that given enough errors, your systems could alert you to a potential breach of the SLO.

SLOs narrow the scope of your alerts to consider only symptoms that impact what the users of our service experience. If an underlying condition is impacting “a user loading our home page and seeing it quickly,” an alert should be triggered, because someone needs to investigate why. However, there is no correlation as to why and how the service might be degraded. We simply know that something is wrong.

In contrast, traditional monitoring relies on a cause-based approach: a previously known cause is detected (e.g., an abnormal number of threads), signaling that users might experience undesirable symptoms (slow page-load times). That approach fuses the “what” and “why” of a situation in an attempt to help pinpoint where investigation should begin. But, as you’ve seen, there’s a lot more to your services than just the known states of up, down, or even slow. Emergent failure modes can impact your users long before coarse synthetic probes will tell you. Decoupling “what” from “why” is one of the most important distinctions in writing good monitoring with maximum signal and minimum noise.