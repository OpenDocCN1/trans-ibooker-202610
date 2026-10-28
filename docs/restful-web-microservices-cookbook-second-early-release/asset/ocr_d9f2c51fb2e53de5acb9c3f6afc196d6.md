# Problem

How can we make sure our service interfaces work even when that service is enlisted as one part of a larger solution? A solution that has been designed by someone else (the API consumer application) and that integrates with other APIs. What does it take to ensure our services can be easily integrated with other services that I have not seen before and do not control while still maintaining system safety and data integrity?

# Solution

Establishing the principle that your service should be able to interoperate with other, unknown, services can be a challenge. The easiest and safest way to do this is to implement the API as a set of stand-alone, stateless operations. These can accept inputs, perform an action, return results, and forget the whole experience.

Computing values is a good example of a simple, stateless transfer between services. Consider a postal code validator service that might support passing in a body with a full address (and possibly some identity metadata) and returning an associated postal code. All the needed state data is transferred in the request/response pair.

However, there will be times when you need to transfer a block of data into a service, allow that service to operate on that data for a while, and then export the results. In these cases, more explicit import/export operations may be a good solution.

The simplest way to transfer state between services is by passing data properties using a form supplied by the service *receiving* the state information. Fundamentally, this is how HTML and the other hypermedia formats work. However, some services may require additional metadata be transferred in order to process the incoming data or may not support native hypermedia controls. In those cases, you need to design additional support in your interface to explicitly transfer state data.

The next easiest way to create a service that is easy for someone else to use is to support a step that allows transferring state from one service to