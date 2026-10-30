# Dimensions

Try to {::} GROUP BY columns that look most different between the successful and failed.

Figure 8-3. In this close-up of the results from Figure 8-2, the orange bar (light gray in the image) shows that global.availability_zone appears as us-east-1a in 98% of events in the selected area, and the blue bar (dark gray) shows that's the case in only 17% of baseline events

In this example, you can quickly see that slow-performing events are mostly originating from one particular availability zone (AZ) from our cloud infrastructure provider. The other information automatically surfaced also points out one particular virtual machine instance type that appears to be more affected than others. Other dimensions surfaced, but in this example the differences tended to be less stark, indicating that they were perhaps not as relevant to our investigation.