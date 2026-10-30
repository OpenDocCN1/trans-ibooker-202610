# Secret Manager

Secret Manager is a convenient and secure system that's used to store certificates, passwords, or API keys. Storing this sensitive data in a central location allows for a single source of truth to manage, access, and audit secrets across Google Cloud. Secret Manager also allows access policies to be set for each secret. Additionally, audit logs can be configured for each secret access. This service is an alternative for third-party secret management systems such as Hashicorp Vault.

In this section, we looked at some of the other security services offered by GCP. Not all of them will be questioned in the exam, but it is good to have a high-level understanding of what each service offers.

# Summary

There are much bigger, deeper dives into security, but for the exam, we must understand that security is key to all Google's services and was not an afterthought. In this chapter, we looked at many services that GCP offers to make sure our solution is secure. Among other things, we introduced Cloud Identity, covered the IAM model, and looked at encryption. In the next chapter, we will look at Google Cloud Management Options.

> Exam Tip
> Remember that we should never grant a user or service more permissions than what is required. Always use the principle of least privilege.

# Further reading

Read the following articles for more information on what was covered in this chapter:

+   - **Cloud Identity**: https://cloud.google.com/identity/docs/
+   - **IAM**: https://cloud.google.com/iam/docs/
+   - **Encryption**: https://cloud.google.com/storage/docs/encryption/
+   - **Cloud IAP**: https://cloud.google.com/iap/docs/
+   - **Security Command Center**: https://cloud.google.com/security-command-center/docs/