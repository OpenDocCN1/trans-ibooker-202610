# 6.5.2 Invoking services with a Load Balancer-aware Spring REST template

Next, we’ll see an example of how to use a REST template that’s Load Balancer-aware. This is one of the more common mechanisms for interacting with the Load Balancer via Spring. To use a Load Balancer-aware RestTemplate class, we need to define a RestTemplate bean with a Spring Cloud @LoadBalanced annotation.

For the licensing service, the method that we’ll use to create the RestTemplate bean can be found in the LicenseServiceApplication class in src/main/java/com/optimagrowth/license/LicenseServiceApplication.java. The following listing shows the getRestTemplate() method that will create the Load Balancer-backed Spring RestTemplate bean.

# Listing 6.13 Annotating and defining a RestTemplate construction method

```
//Most of the import statements have been removed for conciseness
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
@RefreshScope
public class LicenseServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(LicenseServiceApplication.class, args);
    }

    @LoadBalanced
    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
Gets a list of all the instances for the organization services
```

Now that the bean definition for the backed RestTemplate class is defined, any time you want to use the RestTemplate bean to call a service, you only need to autowire it to the class using it.

Using the backed RestTemplate class pretty much behaves like a standard Spring RestTemplate class, except for one small difference in how the URL for the target service is defined. Rather than using the physical location of the service in the RestTemplate call, you need to build the target URL using the Eureka service ID of the service you want to call. The following listing allows us to see this call. The code for this listing can be found in the src/main/java/com/optimagrowth/license/service/client/OrganizationRestTemplateClient.java class file.

# Listing 6.14 Using a Load Balancer-backed RestTemplate to call a service

```
//Package and import definitions left out for conciseness
@Component
public class OrganizationRestTemplateClient {
```