From an application perspective, it depends greatly on your chosen identity provider and how it is implemented. You can, however, add safeguards like network security groups, an Azure Policy that enforces TLS 1.2 and HTTPS, and perhaps Managed Identity. All of these application-level security techniques are helpful in securing your application running on a VM. There is one last technique that we also covered in Chapter 2, and that was the concept of encryption of data at rest. This is important if the contents of the managed disks need to be encrypted due the storage of very sensitive data, intellectual property, or the code that runs the application. To encrypt a managed disk, complete Exercise 4.12.

# EXERCISE 4.12

# Encrypting a Managed Disk

+   1. Remember in Exercise 2.9 when you created the Azure Key Vault key? You will need that again to complete this exercise. Open an Azure PowerShell session and execute the following cmdlet. The output should resemble Figure 4.36.
+   
+   ```
+   Get-AzVmDiskEncryptionStatus -ResourceGroupName <name> -VMName <name>
+   ```

# FIGURE 4.36 Checking the encryption status of a managed disk using PowerShell

![](bbox=[0.13, 0.45, 0.85, 0.53])

+   2. Notice that the OsVolumeEncrypted status is set to NotEncrypted. Execute the following Azure PowerShell cmdlets to encrypt the OS drive:
+   
+   ```
+   $KeyVault = Get-AzKeyVault -VaultName <name> -ResourceGroupName <name>
+   $DAR = Get-AzKeyVaultKey -VaultName <name> -Name <name>
+   
+   Set-AzVmDiskEncryptionExtension -ResourceGroupName <name> \
+       -VMName <name> -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri \
+       -DiskEncryptionKeyVaultId $KeyVault.ResourceId \
+       -KeyEncryptionKeyVaultId $KeyVault.ResourceId \
+       -KeyEncryptionKeyUrl $DAR.Id -SkipVmBackup -VolumeType All
+   ```

+   3. When prompted, read the warning message about a potential reboot and the possible duration of the encryption. Enter Y and then press Enter to encrypt the drive. When the encryption is complete, execute the following cmdlet to check the OsVolumeEncrypted status. Notice the value is now Encrypted, as shown in Figure 4.37.
+   
+   ```
+   Get-AzVmDiskEncryptionStatus -ResourceGroupName <name> -VMName <name>
+   ```