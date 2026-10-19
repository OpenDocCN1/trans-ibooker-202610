
^([8](#ch08.html#id777-marker)) The bucket name starts with `lss-installers`.

^([9](#ch08.html#id778-marker)) These malicious actions may include exfiltrating secrets or credentials from the system or terminal environment and sending the credentials to a remote server owned by the attacker.

^([10](#ch08.html#id783-marker)) When using Terraform and other IaC tools, make sure to check the licensing details, as this may affect how you can use or distribute the software or application(s) you are managing. Check the [“HashiCorp Licensing FAQ”](https://oreil.ly/VgACz) page for more information.

^([11](#ch08.html#id784-marker)) Terraform saves the plan as a binary file (not meant to be human-readable).

^([12](#ch08.html#id786-marker)) Alternatively, you can open a new Cloud Shell terminal directly in your current browser tab by clicking the Activate Cloud Shell button in the upper-right corner of the Google Cloud console.

^([13](#ch08.html#id790-marker)) Automated monitoring and remediation of configuration drift is not covered in this book.

^([14](#ch08.html#id792-marker)) This chapter assumes you are working with Checkov version 3.2.471. A slightly newer or older version should work without issues, as long as the core functionality remains the same.

^([15](#ch08.html#id793-marker)) This is an in-place update, which means the bucket is modified without being destroyed and re-created. Knowing this is important because it helps you understand the impact of changes and avoid unexpected downtime or data loss.

# Chapter 9. Abusing Google Cloud Storage Event Triggers with Malicious File Uploads

Imagine you’ve developed a serverless video-sharing application where users upload video clips that are automatically transcoded for optimal file size and playback quality on mobile and desktop clients. Each time a user uploads a video clip to a cloud storage bucket, a preconfigured cloud storage event triggers a serverless compute resource that converts the video into multiple formats. A few months after launch, you discover that an attacker has exploited a vulnerability in your application and gained access to uploaded videos, user data, and other resources across your cloud account. Upon closer inspection, you find out that the attacker has exploited a code injection vulnerability in your serverless application through a malicious file upload. This then allows the attacker to compromise other resources inside the cloud account.

In this chapter, you’ll set up a vulnerable-by-design serverless application to see firsthand how attackers can use malicious file uploads as a starting point to gain unauthorized access to confidential data as well as other resources in the cloud account. You’ll then explore how an attacker can exploit event-driven file-processing workflows through a code injection attack that escalates into remote code execution and ultimately results in a reverse shell. By the end of this chapter, you’ll know how to identify vulnerabilities and misconfigurations in event-driven file processing applications. This will help you recognize relevant best practices for securing serverless applications against malicious file uploads.

# Reviewing Technical Prerequisites

To follow along in this chapter, and in [Chapter 10](#ch10.html#chap-10-title) as well, you’ll need two Google Cloud accounts: one to set up and host the vulnerable-by-design serverless lab environment (the Serverless Lab account) and another for running simulated exploits (the Attacker account).^([1](#ch09.html#id799))

While following along in this chapter, make sure to have both accounts ready and open in separate browser sessions (or incognito/private windows) to avoid session conflicts. If you have not set up the corresponding billing accounts in your Google Cloud accounts, make sure to configure your billing information before proceeding so that the lab environment works as expected.

###### Note

To help you work through the exercises and simulations, a copy of the code and commands used in this chapter is available in a [GitHub gist (see `ch09.md`)](https://oreil.ly/0mSPU).

# Preparing the Vulnerable-by-Design Google Cloud Lab Environment

In [Chapter 2](#ch02.html#chap-02-title), you got an overview of how event-driven file processing works as well as some key considerations to keep in mind when implementing this pattern. You learned that failing to validate inputs in the code that is handling cloud storage events can introduce security risks. The vulnerable-by-design lab environment you’ll set up in this chapter lets you explore this pattern in depth, understand the associated risks, and see firsthand how attackers can exploit them.

The serverless lab environment features an input bucket with an event trigger that automatically invokes a vulnerable Cloud Run service responsible for converting uploaded HTML files into PDF documents. When a user uploads an HTML file to the designated input bucket, the Cloud Run service downloads it from the Cloud Storage bucket, performs the HTML-to-PDF conversion, and then stores the resulting PDF in the designated output bucket. The Cloud Run service runs under an overly permissive service account, which, if compromised, could give an attacker owner-level access to the entire Google Cloud project.

The lab setup shown in [Figure 9-1](#ch09.html#gcp-lab-intro) is divided into two parts. The first part includes two Cloud Storage buckets: the designated input bucket, where users can upload HTML files, and the designated output bucket, where users can retrieve the converted PDF files. The second part is made up of backend resources that users and potential attackers cannot directly access, such as the vulnerable Cloud Run service with its overly permissive service account, a Parameter Manager parameter, and a Cloud Storage bucket containing sensitive customer-related information.

![](assets/lsse_0901.png)

To help you dive deeper into the technical details of this vulnerable-by-design setup, this section is composed of the following parts:

*   [“Setting Up the Google Cloud Project”](#ch09.html#setting_up_the_google_cloud_project_ch09-abusing-google-cloud-storage-event-triggers-with-malicious-file-uploads_1762889674651)
*   [“Creating a Parameter Manager Parameter”](#ch09.html#creating_a_parameter_manager_parameter_ch09-abusing-google-cloud-storage-event-triggers-with-malicious-file-uploads_1762889687922)
*   [“Setting Up the Cloud Storage Buckets”](#ch09.html#setting_up_the_cloud_storage_buckets_ch09-abusing-google-cloud-storage-event-triggers-with-malicious-file-uploads_1762889701387)
*   [“Deploying a Vulnerable Cloud Run Service”](#ch09.html#deploying_a_vulnerable_cloud_run_service_ch09-abusing-google-cloud-storage-event-triggers-with-malicious-file-uploads_1762889717242)
*   [“Completing the Vulnerable Event-Driven File Upload Service”](#ch09.html#completing_the_vulnerable_event-driven_file_upload_service_ch09-abusing-google-cloud-storage-event-triggers-with-malicious-file-uploads_1762889741209)
*   [“Validating End-to-End Event-Driven Processing”](#ch09.html#validating_end-to-end_event-driven_processing_ch09-abusing-google-cloud-storage-event-triggers-with-malicious-file-uploads_1762889754315)

By understanding how event-driven file processing is implemented in this vulnerable serverless lab, you’ll be better equipped to recognize similar vulnerabilities in real-world serverless applications.

## Setting Up the Google Cloud Project

Let’s start by setting up and configuring a new Google Cloud project that will contain all the resources for the vulnerable-by-design lab environment. Even if you already have projects in your account, it’s best to create a new, dedicated project so you can manage the serverless application resources more easily and clean up afterward without affecting your other projects.

Before you dive into the hands-on portion, let’s define a few key terms and concepts relevant to this chapter:

### Project ID
