
Lightsail manages the TLS certificate, load balancer, compute, and storage. It can also manage MySQL and PostgreSQL databases as part of your deployment if your application requires it. Lightsail performs routine health checks on your application and will automatically replace a container you deploy that may have become unresponsive for some reason. Changing the power and scale parameters in the `lightsail create-container-service` command will allow you to create services for demanding workloads.

Using this recipe, you could deploy any common container-based application (e.g., Wordpress) and have it served on the internet in a short period of time. You could even point a [custom domain alias](https://oreil.ly/rB5Xf) at your Lightsail deployment for an SEO-friendly URL.

#### Challenge

Scale your service so that it will be able to handle more traffic.

# 6.4 Deploying Containers Using AWS Copilot

## Problem

You need a way to use your existing `Dockerfile` to quickly deploy and manage a load balanced web service using best practices in a private network.

## Solution

Starting with a `Dockerfile`, you can use [AWS Copilot](https://oreil.ly/zH8YD) to quickly deploy an application using an architecture shown in [Figure 6-7](#ch06.html#aws_copilot_load_balanced_web_service_i).

![](assets/awsc_0607.png)

### Prerequisite

*   [AWS Copilot CLI](https://oreil.ly/PvZlD)

### Preparation

Follow the steps in this recipe's folder in the [chapter code repository](https://github.com/AWSCookbook/Containers).

### Steps

1.  Copilot requires an ECS service-linked role to allow Amazon ECS to perform actions on your behalf. This may already exist in your AWS account. To see if you have this role already, issue the following command:

    ```
    aws iam list-roles --path-prefix /aws-service-role/ecs.amazonaws.com/
    ```

    (If the role is displayed, you can skip the following role-creation step.)

    Create the ECS service-linked role if it does not exist:

    ```
    aws iam create-service-linked-role --aws-service-name ecs.amazonaws.com
    ```

    ###### Note

    IAM service-linked roles allow AWS services to securely interact with other AWS services on your behalf. See the AWS [article](https://oreil.ly/CwzCZ) on using these roles.

2.  `cd` to this recipe's directory in this chapter's [code repository](https://github.com/AWSCookbook/Containers):

    ```
    cd 604-Deploy-Container-With-Copilot-CLI
    ```

    ###### Note

    You could provide your own `Dockerfile` and content for this recipe. If you choose to use your own container with this recipe, ensure that the container listens on port 80/TCP. Or configure the alternate port with the `copilot init` command.

3.  Now use AWS Copilot to deploy the sample NGINX `Dockerfile` to Amazon ECS:

    ```
    copilot init --app web --name nginx --type 'Load Balanced Web Service' \
        --dockerfile './Dockerfile' --port 80 --deploy
    ```

    ###### Note

    If you don't specify any arguments to the `copilot init` command, it will walk you through a menu of options for your deployment.

    The deployment will take a few moments. You can watch the progress of the deployment in your terminal.

#### Validation checks

After the deployment is complete, get information on the deployed service with this command:

```
copilot svc show
```

### Cleanup

Follow the steps in this recipe's folder in the [chapter code repository](https://github.com/AWSCookbook/Containers).

## Discussion

###### Tip

Copilot exposes a command-line interface that simplifies deployments to Amazon ECS, AWS Fargate, and AWS App Runner. It helps streamline your development workflow and deployment lifecycle.

The `copilot init` command created a directory called `copilot` in your current working directory. You can view and customize the configuration by using the `manifest.yml` that is associated with your application.

###### Note

The test environment is the default environment created. You can add additional environments to suit your needs and keep your environments isolated from each other by using the `copilot env init` command.

Copilot configures all of the required resources for hosting containers on Amazon ECS according to many best practices. Some examples are deploying to multiple Availability Zones, using subnet tiers to segment traffic, and using AWS KMS to encrypt.

The AWS Copilot commands can also be embedded in your CI/CD pipeline to perform automated deployments. In fact, Copilot can orchestrate the creation and management of a CI/CD pipeline for you with the `copilot pipeline` command. For all of the current supported features and examples, visit the [AWS Copilot home page](https://aws.github.io/copilot-cli).

#### Challenge

Reconfigure your Load Balanced Web Service to deploy to [AWS App Runner](https://oreil.ly/BvUTg) instead of Amazon ECS.

# 6.5 Updating Containers with Blue/Green Deployments

## Problem

You want to use a deployment strategy with your container-based application so you can update your application to the latest version without introducing downtime to customers, while also being able to easily roll back if the deployment was not successful.

## Solution

Use AWS CodeDeploy to orchestrate your application deployments to Amazon ECS with the Blue/Green strategy, as shown in [Figure 6-8](#ch06.html#bluesolidusgreen_target_group_associati).

![](assets/awsc_0608.png)

### Preparation

Follow the steps in this recipe's folder in the [chapter code repository](https://github.com/AWSCookbook/Containers).

### Steps

1.  With the CDK stack deployed, open a web browser and visit the `LOAD_BALANCER_DNS` address on TCP port 8080 that the CDK output displayed. You will see the "Blue" application running:

    ```
    E.g.:
    firefox $LOAD_BALANCER_DNS:8080
    or
    open http://$LOAD_BALANCER_DNS:8080
    ```

2.  Create an IAM role with the statement in the provided `assume-role-policy.json` file using this command:

    ```
    aws iam create-role --role-name ecsCodeDeployRole \
        --assume-role-policy-document file://assume-role-policy.json
    ```

3.  Attach the IAM managed policy for `CodeDeployRoleForECS` to the IAM role:

    ```
    aws iam attach-role-policy --role-name ecsCodeDeployRole \
        --policy-arn arn:aws:iam::aws:policy/AWSCodeDeployRoleForECS
    ```

4.  Create a new ALB target group to use as the "Green" target group with CodeDeploy:

    ```
    aws elbv2 create-target-group --name "GreenTG" --port 80 \
        --protocol HTTP --vpc-id $VPC_ID --target-type ip
    ```

5.  Create the CodeDeploy application:

    ```
    aws deploy create-application --application-name awscookbook-605 \
        --compute-platform ECS
    ```

    ###### Note

    CodeDeploy requires some configuration. We provided a template file for you (`codedeploy-template.json`) in this recipe's directory of the [code repository](https://github.com/AWSCookbook/Containers).

6.  Use the `sed` command to replace the values with the environment variables you exported with the `helper.py` script:

    ```
    sed -e "s/AWS_ACCOUNT_ID/${AWS_ACCOUNT_ID}/g" \
        -e "s|PROD_LISTENER_ARN|${PROD_LISTENER_ARN}|g" \
        -e "s|TEST_LISTENER_ARN|${TEST_LISTENER_ARN}|g" \
        codedeploy-template.json > codedeploy.json
    ```

    ###### Tip

    `sed` (short for *stream editor*) is a great tool to use for text find-and-replace operations as well as other types of text manipulation in your terminal sessions and scripts. In this case, `sed` is used to replace values in a template file with values output from `cdk deploy` set as environment variables.

7.  Now, create a deployment group:

    ```
    aws deploy create-deployment-group --cli-input-json file://codedeploy.json
    ```

    `AppSpec-template.yaml` contains information about the application you are going to update. The CDK preprovisioned a task definition you can use.
