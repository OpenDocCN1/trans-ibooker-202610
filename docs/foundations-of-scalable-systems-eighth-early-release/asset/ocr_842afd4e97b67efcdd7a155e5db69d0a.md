# Deploying Microservices

To support frequent updates and benefit from the agility afforded by small teams, you need to be able to deploy new microservice versions easily and quickly. This is where we start to infringe on the world of continuous deployment and devops, which is way beyond the scope of this book (see “Summary and Further Reading” at the end of this chapter for reading recommendations). Still, deployment options impinge on the ease of scalability for a microservice. I’ll just describe one common approach for deploying microservices in this section.

Serverless processing platforms, as I described in Chapter 8, are an attractive microservices deployment approach. A microservice can be built to expose its API on the serverless platform of your choice. The serverless option has three advantages:

+   - Deployment is simple – just upload the new executable package for your microservice to the endpoint that you have configured for your function.
+   - Pay by usage – if your service has periods of low volume requests, your costs are low, even zero.
+   - Ease of Scaling – the platform you choose handles scaling of your function. You control precisely how this works through configuration parameters, but the serverless option takes the heavy lifting out of scalability.

When you deploy all your microservices on a serverless platform, you expose multiple endpoints that clients need to invoke. This introduces complexity as clients need to be able to discover the location – host IP address and port – of each microservice. What if you decide to refactor your microservices by perhaps combining two in or order to eliminate network calls? Or move an API implementation from one microservice to another? Or even change the endpoint – IP address and port – of an API?

Exposing backend changes directly to clients is never a good idea. The Gang of Four book⁵ taught us this many years ago with the façade pattern