
## Configuring ECS Services

12. Select the `ecommerce` cluster and click **Create** in the **Services** tab. Now, we are configuring our service.

13. Select launch type as **Fargate**, and select our previously created task definition `users`, revision 1.

14. Type in `users` for **Service name**, and leave the rest as it is.

15. Type in `1` for **Number of tasks**.

16. Leave the rest as it is, and click **Next Step**.

17. On the next screen, select the only **virtual private cloud** (**VPC**) we have, and at least two subnets. Select the security group we previously created and leave **Auto-assign public IP** as **ENABLED**, as shown in the following screenshot:

![](assets/b92f40d2-fb40-4aa4-bb28-6718a191a35f.png)

18. Select **Application Load Balancer** under **Load balancing**, as shown in the following screenshot:

![](assets/1852a9f4-b55e-4f7b-ae7e-254cc28f9976.png)

19. Select the only load balancer available, then select the single available listening port (`80`). Leave the rest as it is now, but write `/v1/users\*` into the **Path pattern** and `1` in the **Evaluation order**, as shown in the following screenshot:

![](assets/1f010d74-efc4-4169-a4b4-cd18bdd3b42c.png)

When you do this step again for the other services, you will need to write `2` for the second service, and `3` for the third. This signifies that the priority is a unique field, and while all our services have different tasks, the load balancer still expects a prioritized order.

20. Lastly, write `/v1/users/health` in **Health check path** and uncheck **Enable service discovery integration**, as we do not need it now. Then, click **Next step**.

21. Select **Do not adjust the service's desired count** at **Service Auto Scaling**, and click **Next Step** (we will cover this in the next chapter).

22. On the next screen, you will see a summary of this service. Click **Create Service**. The service is created now, as can be seen in the following screenshot:

![](assets/95098bf3-27ce-4cf1-87be-51c732c26224.png)

Now, you need to repeat the same steps for `orders` and `products` as well. Remember to use the names properly. Note that the services are attempting to start but will not succeed; we have not pushed a Docker image yet.

Now, we have gone through the entire process of setting up our services. You have learned how repositories, services, clusters, and tasks are set up in AWS. Now, we are going to look at how to automatically deploy using AWS CodePipeline, in the next section.

## Using CodePipeline for CD

So, we have our three services set up, and they *would* be running once we upload a Docker image. You learned in the last chapter, [Chapter 14](#e8c07878-f14b-46a3-8101-c541eef12c87.xhtml), *Docker and the Cloud*, how to do that through your command line; however this time, we want to get AWS to do it from our source directly. The process to be automatically deployed is called **CD**.

Let's quickly talk about how CodePipeline operates. CodePipeline works in stages. At every stage, it is doing something with our code and sending it to the next stage (through the pipe). The stages, however, are often composed of other services such as CodeBuild. CodeBuild is a tool that essentially uses EC2 units to compile code for us. Both CodeBuild and CodePipeline are incredibly flexible and customizable. It is worth investing some time into learning more about how to use them.

To get started, you need to have three repositories set up in GitHub. They can be private or public; either will work fine. We need them for our three services, as follows:

*   User Management Service
*   Product Management Service
*   Order Management Service

In this particular case, GitHub should be used. Other Git repository hosts will also work, but this chapter assumes the use of GitHub as this is natively integrated into AWS.

Once you have your GitHub repositories ready, you can follow these steps:

1.  Open the `buildspec.json` file that is in each folder and adjust the `<REP_URL>` to be the correct repository URL.

2.  Open AWS CodePipeline under `https://console.aws.amazon.com/codesuite/codepipeline/pipelines`.

3.  Click on **New Pipeline** and enter `users` under **Pipeline name**. Leave the rest as it is. Then, click **Next**, as shown in the following screenshot:

![](assets/f84e2360-c5d5-4911-a6aa-30ceb433df22.png)

When you go through this again, make sure that you use the service role you have created in this step. Meaning it will say `New service role`, and that is okay here. You can then reuse that service role once you create the pipelines for orders and products.

4.  On the next screen, select **GitHub** as the source. Then, click **Connect to GitHub**, as shown in the following screenshot:

![](assets/e5cddb42-25f1-4ecf-b146-571e85e4e8d6.png)

5.  AWS will now guide you through a window that authorizes AWS to access your GitHub files. Allow AWS to do that, and once you're done, you can select the repository and the branch.

6.  On the next screen, select **AWS CodeBuild** as the **Build provider**, and then click on **Create project**, as shown in the following screenshot:

![](assets/18864f49-a0b9-4ed4-9cf5-40815b0197fd.png)

7.  On the next screen, fill out the details, as follows:

*   **Project name**: `users`
*   **Environment image**: `Managed Image`
*   **Operating system**: `Amazon Linux 2`
*   **Runtime**: `Standard`
*   **Image**: `AWS/codebuild/amazonlinux2-x86_64-standard:2.0`
*   **Privileged**: `Yes`
*   **Service Role**: `New service role`

8.  Leave the rest as it is, and click **Continue to CodePipeline**.

9.  Your screen should now look like this:

![](assets/f4ef0cee-2666-4d19-9233-6fbff0804456.png)

10. Click **Next** and select **Amazon ECS** as your **Deploy provider**. Then, type in `ecommerce` as the cluster and `users` as the service. Your screen should look like this:

![](assets/6ae5d41a-bc3b-4bee-946f-ecf2998ccf00.png)

11. Click **Next**, and then, on the review screen, click **Create Pipeline**.

12. You will now be redirected to the CodePipeline screen, which shows you all the steps your code is now going through.

13. You will notice that the process does not go beyond the CodeBuild stage, and there is an error. We need to adjust some roles. Go to **Identity and Access Management** (**IAM**) by clicking on **Services** and then selecting **IAM**. Then, select **Roles**, and select the newly created role. It should have **CodeBuild** in it and will look like `codebuild-users-service-role`. Select **Attach Policy**, and then search for `AmazonEC2ContainerRegistryFullAccess` and add it to the role.

14. Once you have finished this for `users` and everything looks good, repeat this section for `products` and `orders`.

The final screen for a pipeline looks like this:

![](assets/0df98b73-2562-4262-ad2a-e9b61c78b969.png)

At the bottom, we have the **Deploy** section, which has succeeded now, as can be seen in the following screenshot:

![](assets/4620ed18-85cd-46b8-99f1-2b4579143b57.png)

Yay! We did it! Our services should have compiled and deployed now. Notice that you only need to commit and push to GitHub for it to automatically deploy to your system.

## Summary

In this chapter, you have learned how to set up AWS to run Vapor applications as a microservice architecture. We went through the process of setting up a load balancer and creating a cluster that can run the services.

We then went through the process of setting up an automatic CD, and we connected it to a GitHub repository, which means that pushing code to that repository will result in the automatic deployment of our service to AWS. In the end, we have a fully functional API up and running.

It is worth noting here that I have personally talked to Amazon engineers to confirm that Amazon itself uses this exact approach to deploy their microservices. While this approach might be a little complicated, it has been tested and proven to be reliable by companies such as Amazon and others.

## Questions

To make sure your understanding is correct, answer the following questions:
