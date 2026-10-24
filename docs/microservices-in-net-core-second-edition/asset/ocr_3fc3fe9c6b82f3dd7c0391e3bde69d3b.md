# CHAPTER 4  *Identifying and scoping microservices*

![](bbox=[0.129, 0.078, 0.877, 0.281])

**Figure 4.9**  To avoid bogging down the loyalty program microservice in technical details for handling each type of notification, we’ll introduce three supporting technical microservices, one for each type of notification.

This is better. The loyalty program microservice doesn’t have to implement all the details of dealing with each type of notification, which keeps the microservice’s scope narrow and focused. The situation isn’t perfect, though: the microservice still has to decide which of the supporting technical microservices to call for each registered customer.

This leads us to introducing one more microservice, which acts as a front for the three microservices handling the three types of notifications. This new notifications microservice is depicted in figure 4.10 and is responsible for choosing which type of notification to use each time a customer needs to be notified.

![](bbox=[0.129, 0.591, 0.875, 0.762])

**Figure 4.10**  To remove more complexity from the loyalty program microservice, we’ll introduce a notifications microservice that’s responsible for choosing a type of notification based on customer preferences.

This example of a supporting technical capability differs from the previous example of the ERP integration in that other microservices may also need to send notifications to specific customers. For instance, one of the functionalities of the POS system is to send