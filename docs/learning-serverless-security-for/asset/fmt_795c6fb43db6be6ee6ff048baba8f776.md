
In this section, you’ll set up API Gateway to route requests securely to your Lambda function. More specifically, you’ll create a REST API, add resources and HTTP methods, and configure a Lambda proxy integration, as shown in [Figure 7-5](#ch07.html#ch07-api-gateway-setup).

![](assets/lsse_0705.png)

After that, you’ll deploy the API to a stage and update the Lambda function to include CORS headers for cross-origin requests.^[5](#ch07.html#id728) To help you understand the process and key components involved, let’s define a few key terms:

Amazon API Gateway

This fully managed service allows you to create, deploy, manage, monitor, and secure APIs for your applications. It serves as a “front door” for applications to access data, business logic, or functionality from backend services like AWS Lambda, EC2, and other HTTP endpoints.

API Gateway REST API

A type of Amazon API Gateway API that is composed of resources and methods defining how clients can interact with your backend services. It supports HTTP methods such as `GET`, `POST`, `PUT`, and `DELETE`, and allows fine-grained control over request handling and authorization.

API stage

A named reference to a deployment of an API Gateway API. It represents a snapshot of the API configuration and enables you to manage multiple environments, such as development, staging, and production, each with its own URL, stage variables, and settings.

Integration request

Defines how API Gateway forwards client requests to the backend service. An integration request maps method request data, such as headers, query strings, and payloads, into the format expected by the integration endpoint.

Integration response

Defines how the backend’s response is mapped back to the client. An integration response can transform status codes, headers, and body payloads to match the API’s defined method response.

Method request

Specifies the client-facing interface of an API, including the HTTP method, required request parameters, and authorization settings.

Method response

Defines the client-facing response structure of an API method including HTTP status codes, response models, and headers and maps backend responses to this defined format.

Proxy integration

A type of API Gateway integration that minimizes transformation in API Gateway. With a Lambda proxy integration, the full client request—including headers, query parameters, and body—is sent to the Lambda function as is, and the function’s response is returned directly to the client. With an HTTP proxy integration, API Gateway forwards the entire HTTP request to a backend HTTP endpoint, and the backend’s response is returned to the client with minimal modifications.

Now that you have a solid grasp of the key components and concepts, let’s walk through the journey of a request as it travels through an API Gateway REST API.^[6](#ch07.html#id737) When a client sends a request to your API Gateway REST API, the request is first routed to the appropriate resource and HTTP method. API Gateway then evaluates any required headers, query parameters, or authorization settings defined in the method request. If the method uses a Lambda proxy integration, the full request—including headers, query string parameters, and body—is forwarded directly to the Lambda function. Once the Lambda function processes the request, its response is returned to API Gateway, which applies any configured integration response and method response mappings for status codes, headers, and payloads before delivering the final response to the client.

###### Note

This section assumes you are using the Serverless Lab account, which hosts the vulnerable-by-design serverless lab environment.

Let’s go through the steps to set up the API Gateway REST API:

1. Switch back to the browser tab where the CloudShell console in your Serverless Lab AWS account is already open.

2. Run the following command in the CloudShell terminal after the `$` sign to define the environment variables that will be used for setting up and configuring your API gateway:

```
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
REGION="us-east-1"
LAMBDA_NAME="lambda-0010"
API_NAME="APIGateway0010"
ROUTE_PATH="lambda"
STAGE_NAME="prod"
```

3. Use the `aws apigateway create-rest-api` command to create an API Gateway REST API with the specified name `APIGateway0010`:

```
REST_API_ID=$(aws apigateway create-rest-api \
  --name "$API_NAME" \
  --region $REGION \
  --query 'id' --output text)
```

4. Use the `aws apigateway create-resource` command to create a new resource:

```
PARENT_ID=$(aws apigateway get-resources \
  --rest-api-id $REST_API_ID \
  --query "items[?path=='/'].id" --output text)

RESOURCE_ID=$(aws apigateway create-resource \
  --rest-api-id $REST_API_ID \
  --parent-id $PARENT_ID \
  --path-part $ROUTE_PATH \
  --query 'id' --output text)
```

5. Set up a `GET` method for the REST API:

```
aws apigateway put-method \
  --rest-api-id $REST_API_ID \
  --resource-id $RESOURCE_ID \
  --http-method GET \
  --authorization-type "NONE"
```

6. Use the `aws apigateway put-integration` command to set up the integration between the API gateway and the Lambda function, using an AWS proxy integration:

```
URI_1="arn:aws:apigateway:$REGION:lambda:path"
URI_2="2015-03-31/functions/arn:aws:lambda"
URI_3="$REGION:$ACCOUNT_ID:function:$LAMBDA_NAME/invocations"
URI="$URI_1/$URI_2:$URI_3"

aws apigateway put-integration \
  --rest-api-id $REST_API_ID \
  --resource-id $RESOURCE_ID \
  --http-method GET \
  --type AWS_PROXY \
  --integration-http-method POST \
  --uri $URI
```

7. Configure the method response for the `GET` method by adding CORS headers to allow cross-origin requests:

```
MRO="method.response.header.Access-Control-Allow-Origin=true,"
MRH="method.response.header.Access-Control-Allow-Headers=true,"
MRM="method.response.header.Access-Control-Allow-Methods=true"
PARAMS="$MRO$MRH$MRM"

aws apigateway put-method-response \
  --rest-api-id $REST_API_ID \
  --resource-id $RESOURCE_ID \
  --http-method GET \
  --status-code 200 \
  --response-parameters $PARAMS
```

This yields the following output:

```
{
    "statusCode": "200",
    "responseParameters": {
        "method.response.header.Access-Control-Allow-Headers": true,
        "method.response.header.Access-Control-Allow-Methods": true,
        "method.response.header.Access-Control-Allow-Origin": true
    }
}
```

8. Run the following commands to configure the integration response for the `GET` method to ensure that the CORS headers are returned with the response:

```
MRO="method.response.header.Access-Control-Allow-Origin=\"'*'\","
MRH="method.response.header.Access-Control-Allow-Headers=\"'*'\","
MRM="method.response.header.Access-Control-Allow-Methods=\"'GET,OPTIONS'\""
PARAMS="$MRO$MRH$MRM"

aws apigateway put-integration-response \
  --rest-api-id $REST_API_ID \
  --resource-id $RESOURCE_ID \
  --http-method GET \
  --status-code 200 \
  --response-parameters $PARAMS
```

This returns the following output:

```
{
  "statusCode": "200",
  "responseParameters": {
    "method.response.header.Access-Control-Allow-Headers": "'*'",
    "method.response.header.Access-Control-Allow-Methods": "'GET,OPTIONS'",
    "method.response.header.Access-Control-Allow-Origin": "'*'"
  }
}
```

9. Configure the `OPTIONS` method for CORS to allow the browser to send preflight requests:
