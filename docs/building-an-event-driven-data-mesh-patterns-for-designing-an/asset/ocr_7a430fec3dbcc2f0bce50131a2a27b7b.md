There are many ways to design events for event-driven architectures. However, some are more suitable than others for use in an event-driven data mesh. This chapter covers the best strategies for designing events for your event-driven data products, including how to avoid the numerous pitfalls that you will encounter along the way.

# Introduction to Event Types

There are two main types of events that underpin all of event design: the *state* event, as we first introduced in “State Events and Event-Carried State Transfer” on page 55, and the *delta* event, which we’ll cover in more detail in this chapter.

Figure 7-1 shows a simple square wave in steady state, periodically altering from one state to another based on a delta. Similar to this square wave, we model our events to either capture the state itself or the edge that transitions from one state to another.

![](bbox=[0.143, 0.635, 0.861, 0.815])

Figure 7-1. State and delta during a change