# Knowing When to Stop Optimizing

Another subtle trade-off to consider is your time. Could you spend infinite time squeezing out performance gains? Probably—but you have other features to build and bugs to fix. Once your site is within acceptable thresholds for most users, further improvements might yield diminishing returns. For example, if your site’s already scoring a 90 in Lighthouse, chasing 100 might not noticeably help users—but it could consume a lot of effort that might be better spent elsewhere (and perhaps compromise your code’s clarity too). Especially when optimizations require complex refactoring, they might only be worth it if there is a clear need, like user complaints or a competitive analysis showing that your site lags behind the competitors.

However, sometimes performance issues are silent killers—that is, users leave without complaining, so you never learn about the issue. Rely on your analytics and field data (see Chapter 2). If 20% of users have very slow times, that’s a sizable chunk you could improve, even if the average looks good. Decide what level of user experience you consider acceptable (Google Chrome’s guidelines, which include “p75 LCP < 2.5 s,” are a good starting point). Once you hit “good” for most metrics for most users, you may declare victory and only revisit performance periodically or when adding big changes.

# Conclusion

Performance optimization isn’t about making everything as fast as possible at all costs. It’s about making things fast enough that users are happy, in a way that’s sustainable for development. By understanding the trade-offs and using data to inform decisions, you can find the right balance between speed and everything else you care about, like features, ease of coding, and code clarity.

To quote Tim Kadlec’s advice one more time: “Find a solid base to build from, determine the considerations most important to the project and always