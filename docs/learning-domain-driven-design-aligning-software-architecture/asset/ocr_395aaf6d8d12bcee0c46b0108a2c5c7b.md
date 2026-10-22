![](bbox=[0.13, 0.08, 0.86, 0.58])

Figure 8-6. Service layer

It’s important to note that in the context of the architectural pattern, the service layer is a logical boundary. It is not a physical service.

The service layer acts as a façade for the business logic layer: it exposes an interface that corresponds with the public interface’s methods, encapsulating the required orchestration of the underlying layers. For example:

```
interface CampaignManagementService
{
OperationResult CreateCampaign(CampaignDetails details);
OperationResult Publish(CampaignId id, PublishingSchedule schedule);
OperationResult Deactivate(CampaignId id);
OperationResult AddDisplayLocation(CampaignId id, DisplayLocation
```