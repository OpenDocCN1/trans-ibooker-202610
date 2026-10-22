relevant commands, no two transactions can be considered atomic. This correlates with another aggregate design principle:

> *Only the data within an aggregate’s boundaries can be considered strongly consistent. Everything outside is eventually consistent.*

Use this as a guiding principle to make sure you are not abusing sagas to compensate for improper aggregate boundaries. Business operations that have to belong to the same aggregate require strongly consistent data.

The saga pattern is often confused with another pattern: process manager. Although the implementation is similar, these are different patterns. In the next section, we’ll discuss the purpose of the process manager pattern and how it differs from the saga pattern.

# Process Manager

The saga pattern manages simple, linear flow. Strictly speaking, a saga matches events to the corresponding commands. In the examples we used to demonstrate saga implementations, we actually implemented simple matching of events to commands:

+   - CampaignActivated event to PublishingService.SubmitAdvertisement command

+   - PublishingConfirmed event to Campaign.TrackConfirmation command

+   - PublishingRejected event to Campaign.TrackRejection command

The process manager pattern, shown in Figure 9-14, is intended to implement a business-logic-based process. It is defined as a central processing unit that maintains the state of the sequence and determines the next processing steps.²