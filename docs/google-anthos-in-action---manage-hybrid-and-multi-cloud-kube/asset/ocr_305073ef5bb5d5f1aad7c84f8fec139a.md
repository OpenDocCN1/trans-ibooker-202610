# Admission Controller Policy
Validation Flow

![](bbox=[0.119, 0.098, 0.877, 0.556])

In the above workflow, the API server has received a request to create a new pod. Since the cluster has ACM installed with the Policy Controller enabled, the request is sent to the Gatekeeper service, which forwards the request to the Gatekeeper pod which checks the configured policies for any violations.

At this point, you may be wondering what kind of latency or performance issues a cluster may experience once Gatekeeper has been deployed. On average, policy evaluation takes about 1 millisecond, even when larger policies are evaluated. Like any system that allows you to create your own objects, it is possible to create a policy that will impact the performance of the policy engine - resulting in overall system latency. Creating policies is beyond the scope of this chapter, if you want to learn more about policies and performance, you can find additional details on the Open Policy Agent website at [https://www.openpolicyagent.org/docs/latest/policy-performance/](https://www.openpolicyagent.org/docs/latest/policy-performance/)