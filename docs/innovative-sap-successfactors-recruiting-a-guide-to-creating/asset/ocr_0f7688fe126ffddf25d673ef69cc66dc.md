Chapter 1 covered basic aspects of ODATA APIs. You can also get the events in a customer instance (or any instance for that matter) by simply executing GET <<endpoint>>odata/v2/getExtEventMetaDataDefinition; however, keep in mind that there’s no filter. You will get all events in an instance.

# Subscribing to an Event

The first step in setting up an integration with Intelligent Services is identifying which event to subscribe to. To do this, navigate to Admin Center ➤ ISC (Intelligent Services Center). Figure 12-2 shows the seven recruiting events described in this document. The current section will use “Update of Job Requisition” as an illustration throughout.

![](bbox=[0.11, 0.383, 0.926, 0.537])

*Figure 12-2.* Partial Screenshot Intelligent Services Center Depicting the Recruiting Events

For illustrating this, we will focus on Update of Job Requisition event and show programmatic examples of code. Selecting the Update of Job Requisition brings us to the main ISC page as shown in Figure 12-3.

![](bbox=[0.11, 0.699, 0.926, 0.831])

*Figure 12-3.* Details of the Update of Job Requisition Event