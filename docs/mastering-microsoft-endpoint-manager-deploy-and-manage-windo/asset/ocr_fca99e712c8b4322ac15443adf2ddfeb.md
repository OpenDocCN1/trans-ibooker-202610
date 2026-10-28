Newer Windows 10 versions have a 4K hardware hash that is used in the Windows Autopilot service. Examples include where a new hardware hash is needed if the TPM or motherboard is replaced. Some ways that a device is known in the Windows Autopilot service are based on the SMBIOS UUID, **Media Access Control (MAC)**, or the disk serial number – the reason for this is that there is no unique identifier for a Windows device. If we look at what information is contained in the Autopilot hardware hash, then we also get an idea of why we need a new hardware hash following a motherboard replacement.

The minimum requirements for unique values in the SMBIOS are as follows:

+   - `ProductKeyID`
+   - `Sm biosSystemManufacturer`
+   - `Sm biosSystemProductName`
+   - `Sm biosSystemSerialNumber`
+   - `Sm biosSkuNumber`
+   - `Sm biosSystemFamily`
+   - `MacAddress`
+   - `Sm biosUuid`
+   - `DiskSerialNumber`
+   - `TPM EkPub`

Where is Windows Autopilot device information stored?

Windows Autopilot data is stored in the **United States (US)**, and not in the region where your AAD tenant is located. It is not customer data that is stored, but business data, which enables Microsoft to provide the Windows Autopilot service.

First, you need to download the Windows **Assessment and Deployment Kit (ADK)** from https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install:

> **Important Note**
> You need to use at least ADK version 1703.