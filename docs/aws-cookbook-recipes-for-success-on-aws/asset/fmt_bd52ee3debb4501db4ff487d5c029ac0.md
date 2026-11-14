
```json
{
  "Width": 0.15338942408561707,
  "Height": 0.03961481899023056,
  "Left": 0.06334125995635986,
  "Top": 0.39024031162261963
}
```

![](assets/awsc_0808.png)

### Note

Per the [Textract Developer Guide](https://oreil.ly/Xh9Bk), each `BoundingBox` property has a value between 0 and 1. The value is a ratio of the overall image width (applies to `left` and `width`) or height (applies to `height` and `top`). For example, if the input image is 700 × 200 pixels, and the top-left coordinate of the bounding box is (350,50) pixels, the API returns a `left` value of 0.5 (350/700) and a `top` value of 0.25 (50/200).

### Cleanup

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/ArtificialIntelligence).

## Discussion

You may have used traditional optical character recognition (OCR) software in the past when you have scanned physical documents and used the OCR output to get text-based output of the content. [Amazon Textract](https://aws.amazon.com/textract) takes this a step further using ML models for even more accurate text recognition with additional feature and context information you can use in your applications. You were able to determine the location of a string of text within an image by using this recipe. You could use similar functionality in Textract to forward certain portions of text from images (or scanned forms) to different teams for human review or automating sending certain parts of forms to different microservices in your application for processing or persisting. Application developers do not need to train a model before using Textract; Textract comes pretrained with many datasets to provide [highly accurate](https://oreil.ly/X07aq) character recognition.

### Challenge

Determine the location of two different words in an image and calculate the distance between them.

# Chapter 9. Account Management

## 9.0 Introduction

While the previous chapters have focused on deploying and configuring resources *inside* your AWS account, we want to provide some recipes that show examples of what you can do at the whole account level.

As you continue to scale your AWS usage, you will find it useful to have tools and services available to ease the burden of management, especially if you start to add additional AWS accounts to your environment. We are seeing more and more people choose to use AWS accounts as “containers” for their specific applications. Some companies provide individual accounts for production and nonproduction workloads to business units within a company; some even set up “shared services” accounts to provide internal services to business units within a company to share common resources across the many accounts they may be managing. Your AWS account [spans Regions](https://oreil.ly/INVpe) to give you global capabilities, as shown in [Figure 9-1](#ch09.html#aws_account_perspective).

![](assets/awsc_0901.png)

We like to think of the tools and services that AWS provides as building blocks that you can use to customize your cloud environment to meet your specific needs. Some of these can be used at an account level to give you more management capabilities over your environments as you scale (i.e., adding additional team members or adding additional AWS accounts). You can organize and consolidate billing for many accounts by using [AWS Organizations](https://aws.amazon.com/organizations) and provide centralized access management through [AWS Single Sign-On](https://aws.amazon.com/single-sign-on), both of which you will explore in the recipes in this chapter, in addition to some recipes to help you configure and maintain a secure environment at the account level.

### Tip

You should always keep an eye on the [AWS Prescriptive Guidance](https://aws.amazon.com/prescriptive-guidance) website for the most up-to-date guidance on account-level capabilities and recommendations.

### Workstation Configuration

#### General setup

Follow the [“General workstation setup steps for CLI recipes”](#preface01.html#general_workstation_setup_steps_for_cli) to validate your configuration and set up the required environment variables. Then, clone the chapter code repository:

```
git clone https://github.com/AWSCookbook/AccountManagement
```

## 9.1 Using EC2 Global View for Account Resource Analysis

### Problem

You have been presented with an AWS account for a client. You need to export a CSV file containing all provisioned compute instances, disk volumes, and network resources across all Regions within an AWS account.

### Solution

Navigate to EC2 Global View in the AWS Console. Export a CSV of resources in your account (see [Figure 9-2](#ch09.html#generating_a_csv_of_resources_with_ectw)).

![](assets/awsc_0902.png)

#### Prerequisite

*   AWS account with resources deployed

#### Steps

1.  In the AWS Console, search for and then click EC2 Global View, as shown in [Figure 9-3](#ch09.html#searching_for_global_view).

    ![](assets/awsc_0903.png)

2.  Navigate to the “Global search” menu and click Download CSV (see [Figure 9-4](#ch09.html#downloading_a_csv_from_global_view)).

    ![](assets/awsc_0904.png)

##### Validation checks

Open the downloaded CSV file in your favorite editor on your workstation. From there, you should be able to scroll through resources in your AWS account.

### Discussion

You may find yourself often working in new AWS accounts as part of your job. If you are in a lab environment or a hackathon, using Global View is a great way for you to get an idea of what an AWS account contains. Before you begin to make changes in an account, it is important to take stock of what is already deployed across all Regions. Having this knowledge will allow you to ensure that you don’t mistakenly cause any outages.

While you should use tools like [AWS Config](https://aws.amazon.com/config) and the AWS Billing console for inspecting the configuration of resources inside your AWS account and keeping an eye on your bill, respectively, it is good to know that you can easily generate a list of EC2 and VPC resources. Routine CSV exports at different times can be used to provide a point-in-time snapshot.

#### Challenge

Use the Global search functionality to list all default VPC security groups. You can then double-check inbound rules for extraneous permissions.

## 9.2 Modifying Tags for Many Resources at One Time with Tag Editor

### Problem

You need to add a tag to multiple resources in your AWS account where it doesn’t already exist.

### Solution

Launch the Tag Editor in the AWS Console. Find all resources that have an `Environment` key with nonexisting value. Add the tag (see [Figure 9-5](#ch09.html#adding_a_tag_to_multiple_resources)).

![](assets/awsc_0905.png)

#### Prerequisite

*   AWS account with resources deployed and tagged

#### Steps

1.  In the AWS Console, search for Tag Editor and then click Resource Groups & Tag Editor, as shown in [Figure 9-6](#ch09.html#searching_for_tag_editor_in_the_aws_con).

    ![](assets/awsc_0906.png)

2.  From the lefthand menu, click Tag Editor under the Tagging heading (see [Figure 9-7](#ch09.html#launching_tag_editor-id0000013)).

    ![](assets/awsc_0907.png)

3.  Set “Resource types” to “All supported resource types,” enter a Tag key named `Environment`, use the drop-down menu to select “(not tagged)” for the value, and then click “Search resources” (see [Figure 9-8](#ch09.html#launching_tag_editor-id0000039)).

    ![](assets/awsc_0908.png)

4.  Wait a few moments for the search to complete. Then view the results, select the resources that you wish to tag, and click “Manage tags of selected resources” (see [Figure 9-9](#ch09.html#viewing_search_results_in_tag_editor)).

    ![](assets/awsc_0909.png)
