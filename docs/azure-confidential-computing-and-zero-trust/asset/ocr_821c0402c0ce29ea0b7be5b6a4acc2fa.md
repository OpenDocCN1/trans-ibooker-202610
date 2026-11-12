# # Supporting Services and Features for ACC

Azure offers several features and services that support TEE attestation and other aspects of Azure’s defense-in-depth strategy. All these features can be used with ACC VMs, while some are useful for confidential containers and other PaaS. Attestation-related services and features include the Microsoft Azure Attestation service, the guest attestation feature for confidential VMs, and Secure Key Release. The other services and features that support other aspects of defense in depth include trusted launch, confidential disk encryption using customer-managed keys, and Azure Key Vault Managed HSM (Hardware Security Module) service.

# ## Microsoft Azure Attestation

Microsoft Azure Attestation is a service that verifies the trustworthiness of evidence that a hardware platform and the software operating on it are as expected. This service enables both attestation of platforms supported by Trusted Platform Modules (TPMs) and attestation of the state of TEEs. This provides relying parties, such as a secrets manager, with assurance that the intended software is executing on trusted hardware before secrets are released and sensitive workloads are processed.

Attestation can follow one of two patterns:

+   * In the passport model, the application in the TEE (attester) passes evidence directly to the verifier (Azure Attestation or a third-party attestation service) and receives verification back from the verifier before requesting secrets from a relying party (Figure 3-4).
+   * In the background check model, the application in the TEE first requests secrets from a relying party by passing the evidence to that party, and it lets the relying party verify that the evidence is trustworthy before secrets are released to the TEE (Figure 3-5).