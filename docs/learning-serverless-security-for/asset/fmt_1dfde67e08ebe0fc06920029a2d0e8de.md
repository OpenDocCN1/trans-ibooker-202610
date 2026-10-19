
More specifically, you’ll start by setting a lower request timeout to limit the attacker’s window for running malicious commands inside a reverse shell session. After simulating how an attacker would adjust their techniques to circumvent the constraints, you’ll further reduce the request timeout setting of the vulnerable Cloud Run service to 10 seconds and reduce the permissions of the associated service account to avoid having an attacker immediately gain owner-level privileges when the vulnerable service is compromised. After implementing these changes to the serverless lab environment, you’ll simulate an attacker bypassing the new restrictions and configuring a backdoor service account to maintain persistent access. You’ll then explore multiple strategies and approaches that an attacker could use to abuse misconfigurations and escalate privileges within a Google Cloud account.

This chapter is divided into the following sections:

*   [“Setting a Lower Cloud Run Service Timeout”](#ch10.html#setting_a_lower_cloud_run_service_timeout_ch10-escalating-privileges-in-google-cloud_1762889870469)
*   [“Generating a Service Account Key Associated with the Compromised Cloud Run Service”](#ch10.html#generating_a_service_account_key_associated_with_the_compromised_cloud_run_service_ch10-escalating-privileges-in-google-cloud_1762889882735)
*   [“Configuring a Backdoor Service Account”](#ch10.html#configuring_a_backdoor_service_account_ch10-escalating-privileges-in-google-cloud_1762889895813)
*   [“Further Reducing the Cloud Run Timeout and Restricting the Service Account Permissions”](#ch10.html#further_reducing_the_cloud_run_timeout_and_restricting_the_service_account_permissions_ch10-escalating-privileges-in-google-cloud_1762889907077)
*   [“Exploiting a Cloud Run Service Without a Reverse Shell”](#ch10.html#exploiting_a_cloud_run_service_without_a_reverse_shell_ch10-escalating-privileges-in-google-cloud_1762889924152)
*   [“Escalating Privileges Through a Cloud Run Function”](#ch10.html#privilege_escalation_through_a_cloud_run_function_ch10-escalating-privileges-in-google-cloud_1762889936285)
*   [“Escalating Privileges Through a Compute Engine VM Instance”](#ch10.html#privilege_escalation_through_a_compute_engine_vm_instance_ch10-escalating-privileges-in-google-cloud_1762889947595)

By the end of this chapter, you’ll see why enforcing the principle of least privilege is critical to minimizing the impact of a compromised cloud resource. You’ll also be better equipped to recognize misconfigurations and weak security controls that could be exploited by attackers.

# Reviewing Technical Prerequisites

For this chapter, you’ll continue using the two Google Cloud accounts from [Chapter 9](#ch09.html#chap-09-title): the Serverless Lab account to host the vulnerable-by-design lab environment, and the Attacker account for simulating attacks and testing security controls. Keep both accounts open in separate browser sessions (or incognito/private windows) to avoid session conflicts.

###### Note

To help you work through the exercises and simulations, a copy of the code and commands used in this chapter is available in a [GitHub gist (see `ch10.md`)](https://oreil.ly/0mSPU).

# Setting a Lower Cloud Run Service Timeout

In the preceding chapter, you were able to establish a reverse shell connection and run various commands to inspect the cloud resources in the serverless lab environment. Since the compromised Cloud Run service is configured with a 20-minute execution timeout, you had enough time to execute malicious commands, enumerate resources, and exfiltrate sensitive data over the established connection. In practice, the execution timeout setting may be much lower, which limits the window for executing commands or exploring resources within an established reverse shell session. In this section, you will configure the Cloud Run service with a lower execution timeout to explore how this would limit what an attacker can do during a reverse shell session.

###### Note

This section assumes you are using your Serverless Lab account, which hosts the vulnerable-by-design serverless lab environment.

Let’s now update the timeout setting of the deployed Cloud Run service:

1. Switch back to the browser window where the Google Cloud console of your Serverless Lab account is open.
2. Click the Activate Cloud Shell button in the upper-right corner of the Google Cloud console to open the Cloud Shell terminal.
3. Retrieve and check the current Google Cloud project ID:

```
cd ~
PROJECT_ID=$(gcloud config get-value project)
[[ -z "$PROJECT_ID" ]] && echo "Error: PROJECT_ID is not set."
```

If you do not get an error after running these commands, this means that your `PROJECT_ID` is correctly configured in your environment. Otherwise, run `gcloud auth login` and make sure the correct project is configured via `gcloud config set project [PROJECT_ID]` before proceeding to the next steps.

4. List all Cloud Run services in the current project:

```
gcloud run services list
```

This yields the following output:

```
✔
SERVICE: html-to-pdf-service
REGION: us-east1
URL: https://html-to-pdf-service-....us-east1.run.app
LAST DEPLOYED BY: user@email.com
LAST DEPLOYED AT: 2025-00-00T00:00:00
```

5. Retrieve the name of the deployed Cloud Run service and store it in a variable:

```
read RUN_SERVICE RUN_REGION < <(gcloud run services list \
  --format="value(name,region)" | grep '^html-to-pdf')
```

6. Display the configuration and status of the Cloud Run service:

```
gcloud run services describe $RUN_SERVICE --region=$RUN_REGION
```

This returns the following:

```
✔ Service html-to-pdf-service in region us-east1

URL:     https://html-to-pdf-service-....us-east1.run.app
Ingress: all
Traffic:
  100% LATEST (currently html-to-pdf-service-00001-...)

Scaling: Auto (Min: 0)

Last updated on 2025-00-00T00:00:00.000000Z by user@email.com:
  Revision html-to-pdf-service-00001-abc
  Container None
    Image:           .../html-to-pdf-service
    Port:            8080
    Memory:          512Mi
    CPU:             1000m
    Env vars:
      OUTPUT_BUCKET  ss-output-bucket-...
    Startup Probe:
      TCP every 240s
      Port:          8080
      Initial delay: 0s
      Timeout:       240s
      Failure threshold: 1
      Type:          Default
  Service account:   html-to-pdf@....iam.gserviceaccount.com
  Concurrency:       1
  Max instances:     1
  Timeout:           1200s
```

7. Update the specified Cloud Run service’s timeout to 120 seconds:

```
gcloud run services update $RUN_SERVICE \
  --region=$RUN_REGION \
  --timeout=120
```

This returns the following:

```
OK Deploying... Done.
  OK Creating Revision...
  OK Routing traffic...
Done.
Service [html-to-pdf-service] revision [html-to-pdf-service-...]
has been deployed and is serving 100 percent of traffic.
Service URL: https://html-to-pdf-service-....us-east1.run.app
```

8. Verify that the Cloud Run service has been updated with the new timeout setting:

```
gcloud run services describe $RUN_SERVICE \
  --region=$RUN_REGION
```

You should get the following output response:

```
✔ Service html-to-pdf-service in region us-east1

URL:     https://html-to-pdf-service-....us-east1.run.app

...

Last updated on 2025-00-00T00:00:00.000000Z by user@email.com:
  Revision html-to-pdf-service-00002-def
  Container None
    ...
  Max instances:     1
  Timeout:           120s
```
