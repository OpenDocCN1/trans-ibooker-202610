+   - How will you account for changes in cost centers? Early on, you may have a spreadsheet or a configuration management database (CMDB), but later you will have a meta layer of allocation data in your FinOps platform to dynamically remap against your organization’s ever-changing structure.
+   - How will you account for people or applications shifting between teams, or recombining into different teams? After they switch, how will you get the information that they’ll now care about, since their slice of the data will be different?
+   - How will you notify people that there have been changes to the allocation constructs?
+   - What are the tags you really need? Early on it may be only three, but later there may be dozens. However, if you do expand to dozens, it’s likely you have too many tags, and a future loop through the lifecycle might see you optimizing the number of tags based on the updated goals set in the optimize phase.
+   - Will you do things like “lunch and learns” from your CCoE to regularly present the best practices and get people excited?

As you move around the FinOps loop, you answer more questions. Are you efficient? You’ll need to go through the loop to find out. And after a few times around, you’ll realize that you never actually arrive at the end. You just get better, and your questions get deeper:

+   - Which teams are driving the costs? Are they being efficient? Can you link their costs to unit metrics?
+   - Do you have budgets in place for each team? Are you managing teams to those budgets? Are you able to do activity-based costing?
+   - What’s the state of your tagging strategy? Look at what tags are in place and what they are reporting. Look at coverage gaps. Consider how to allocate untaggable or shared costs.
+   - How will you keep your allocation constructs in sync and up to date? A spreadsheet? Cadence-based syncing with your CMDB? API integration between your FinOps platform and your CMDB?
+   - What is the commitment-based discount strategy? What commitment-based discount programs are there? How well are they being used? How could they be optimized? Which new ones should you make?
+   - And then...rinse and repeat, repeatedly.

You pick the low-hanging fruit in each phase and then move on to the next. Then you readjust your goals and go back around. But you should always start with cost visibility. Then in the next cycle, you set basic budgets, discussed in Chapter 13. And the cycle after that, you’re managing them.