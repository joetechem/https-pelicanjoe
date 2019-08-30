---
title: "Azure Data Encryption"
date: 2019-07-05T15:51:54-04:00
---

# Azure Disk Encryption Prerequisites  

This article explains items that need to be in place before you can use Azure Disk Encrytpion.  

* Azure Disk Encryption is integrated with [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) to help manage encryption keys.  
* You can use Azure Powershell, Azure CLI, or the Azure Portal to configure Azure Disk Encryption.  

Before you enable Azure Disk Encryption on Azure IaaS VMs for the supported scenarios that were discussed in the [Azure Disk Encryption Overview](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-overview) article, be sure to have the prerequisites in place.  

## Supported VM sizes  

Azure Disk Encryption is available on VMs that meet these minimum memory requirements:  

| Virtual Machine | Minimum Memory Requirement |  
|-----------------|----------------------------|  
| Windows VMs     | 2GB                        |  
| Linux VMs, encrypting data volumes only | 2 GB |  
| Linux VMs, encrypting both data and OS volumes, and where the root file system usage is 4GB or less | 8 GB |  
| Linux VMs, encrypting both data & OS volumes; where the root file system usage greater than 4GB | The file system usage * 2. For instance, a 16 GB of root file system usage requires at least 32GB of RAM |  git 

Once the OS disk encryption process is complete on Linux virtual machines, the VM can be configured to run with less memory.  

Azure Disk Encryption is also available for VMs with premium storage.  

## Supported Operating Systems  

* Windows Server versions: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2012 R2 Server Core and Windows Server 2016 Server core. For Windows Server 2008 R2, you must have .NET Framework 4.5 installed before you enable encryption in Azure. Install it from Windows Update with the optional update Microsoft .NET Framework 4.5.2 for Windows Server 2008 R2 x64-based systems (KB2901983).  
* Windows Server 2012 R2 Core and Windows Server 2016 Core are supported by Azure Disk Encryption once the bdehdcfg component is installed on the VM.  
* Windows client versions: Windows 8 client and Windows 10 client.  

## Networking and Group Policy  

**To enable the Azure Disk Encryption feature, the IaaS VMs must meet the following network endpoint configuration requirements:**  

* To get a token to connect to your key vault, the IaaS VM must be able to aconnect to an Azure Active Directory endpoint, [login.microsoftonline.com].  
* To write the encryption keys to your key vault, the IaaS VM must be able to connect to the key vault endpoint.  
* The IaaS VM must be able to connect to an Azure storage endpoint that hosts the Azure extension repository and an Azure storage account that hosts the VHD files.  

* If your security policy limits access from Azure VM s to the Internet, you can resolve the preceding URI and configure a specific rule to allow outbound connectivity to the IPs.  
    - see [Azure Key Vault behind a firewall](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)  

***  

#### Group Policy:  

* The Azure Disk Encryption solution uses the BitLocker external key protecter for Windows IaaS VMs.  
* **For domain joined VMs**, don't push any group policies that enforce TPM protectors.  
    - For more about the group oolicy for "Allow BitLocker w/out a compatible TPM", see [Bitlocker Group Policy Reference](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup)  

* BitLocker policy on domain joined virtual machines with custom group policy must include the following setting: [Configure user storage of BitLocker recovery information -> Allow 256-bit recovery key](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings).  
    - Azure Disk Encryption will fail when custom group policy settings for BitLocker are incompatible.  
    - On machines that don't have the correct policy setting, apply the new policy,  
        - force the new policy to update (`gpupdate.exe/force`)  
        - restart may be required  

* Azure Disk Encryption will fail if the domain level group policy blocks the AES-CBC algorithm, which is used by BitLocker.  

***  

## Prereq Workflow for Key Vault  

If you're already familiar with the Key Vault and Azure AD prerequisites for Azure Disk Encryption, you can use the [Azure Disk Encryption prerequisites PowerShell script](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).  

For more information on using the prereqs script, see the [Encrypt a VM Quickstart](https://docs.microsoft.com/en-us/azure/security/quick-encrypt-vm-powershell) and the [Azure Disk Encryption Appendix](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-appendix#bkmk_prereq-script). 

1. If needed, create a resource group.  
2. Create a key vault.  
3. Set key vault advanced access policies.  

## Create a Key Vault  

Azure Disk Encryption is integrated with Azure Key Vault to help you control and manage the disk-encryption keys and secrets in your key vault subscription. You can create a key vault or use an existing one for Azure Disk Encryption. For more information about key vaults, see [Get started with Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started) and [Secure your key vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault). You can use a Resource Manager template, Azure PowerShell, or the Azure CLI to create a key vault.  

## Set Key Vault Advanced Access Policies  

The Azure platform needs access to the encryption keys or secrets in your key vault to make them avilable to the VM for booting and ecrypting the volumes. Enable disk encryption on the key vault or deployments will fail.  

#### Set key vault advanced access policies with Azure PowerShell  

Use the key vault PowerShell cmdlet [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/en-us/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) to enable disk encryption for the key vault.  

* **Enable Key Vault for disk encryption:** EnabledForDiskEncryption is required for Azure Disk encryption.  

    ```bash  
    Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDiskEncryption
    ```  

* **Enable Key Vault for deployment, if needed:** Enables the Microsoft.Compute resource provider to retrieve secrets from this key vault when this key vault is referenced in resource creation, for example when creating a virtual machine.  
    ```bash  
    Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDeployment
    ```  

* **Enable Key Vault for template deployment, if needed:** Enables Azure Resource Manager to get secrets from this key vault when this key vault is referenced in a template deployment.  
    ```bash  
    Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForTemplateDeployment
    ```  

#### Set key vault advanced access policies with Azure CLI  

Use [az keyvault update](https://docs.microsoft.com/en-us/cli/azure/keyvault#az-keyvault-update) to enable disk encryption for the key vault.

* **Enable Key Vault for disk encryption:** Enabled-for-disk-encryption is required.    

    ```bash  
    az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-disk-encryption "true"
    ```  

* **Enable Key Vault for deployment, if needed:** Enables the Microsoft.Compute resource provider to retrieve secrets from this key vault when this key vault is referenced in resource creation, for example when creating a virtual machine.  
    ```bash  
    az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-deployment "true"
    ```  

* **Enable Key Vault for template deployment, if needed:** Allow Resource Manager to retrieve secrets from the vault.  
    ```bash  
    az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-template-deployment "true"
    ```  

#### Set key vault advanced access policies through the Azure portal  

1. Select your keyvault, go to Access Policies, and Click to show advanced access policies.  

2. Select the box labeled Enable access to Azure Disk Encryption for volume encryption.  

3. Select Enable access to Azure Virtual Machines for deployment and/or Enable Access to Azure Resource Manager for template deployment, if needed.  

4. Click Save.  

#### Set up a key encryption key with Azure PowerShell  

The sample script might need changes for your environment. This script creates all Azure Disk Encryption prerequisites and encrypts an existing IaaS VM, wrapping the disk encryption key by using a key encryption key.  

```PowerShell  
# Step 1: Create a new resource group and key vault in the same location.
	 # Fill in 'MyLocation', 'MyKeyVaultResourceGroup', and 'MySecureVault' with your values.
	 # Use Get-AzLocation to get available locations and use the DisplayName.
	 # To use an existing resource group, comment out the line for New-AzResourceGroup
	 
    $Loc = 'MyLocation';
    $KVRGname = 'MyKeyVaultResourceGroup';
    $KeyVaultName = 'MySecureVault'; 
    New-AzResourceGroup –Name $KVRGname –Location $Loc;
    New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc;
    $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
    $KeyVaultResourceId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname).ResourceId;
    $diskEncryptionKeyVaultUrl = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname).VaultUri;
	 
#Step 2: Enable the vault for disk encryption.
    Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption;
	  
#Step 3: Create a new key in the key vault with the Add-AzKeyVaultKey cmdlet.
	 # Fill in 'MyKeyEncryptionKey' with your value.
	 
	 $keyEncryptionKeyName = 'MyKeyEncryptionKey';
    Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'HSM';
    $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
	 
#Step 4: Encrypt the disks of an existing IaaS VM
	 # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values. 
	 
	 $VMName = 'MySecureVM';
    $VMRGName = 'MyVirtualMachineResourceGroup';
    Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```  

# Enable Azure Disk Encryption for Windows IaaS VMs  

* Strongly recommended that you [Create a snapshot](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/snapshot-copy-managed-disk) and/or backup up your disks before encryption.  
    - Backups ensure that a recovery option is possible if an unexpected failure occurs during encryption.  
    - VMs with managed disks require a backup before encryption occurs.  
* Once a backup is made, you can use the [Set-AzVMDiskEncryptionExtension cmdlet](https://docs.microsoft.com/en-us/powershell/module/az.compute/set-azvmdiskencryptionextension) to encrypt managed disks by specifying the `-skipVmBackup` parameter.  
    - more information about how to back up and restore encrypted VMs, see [Back up and restore encrypted Azure VM](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption)  

## Enable encryption on existing or running IaaS Windows VMs  

You can enable encryption by using a template, PowerShell cmdlets, or CLI commands. If you need schema information for the virtual machine extension, see [Azure Disk Encryption for Windows extension](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/azure-disk-enc-windows).  

**It is mandatory to snapshot and/or backup a managed disk based VM instance outside of, and prior to enabling Azure Disk Encryption.**  

**The Set-AzVMDiskEncryptionExtension command will fail against managed disk based VMs until a backup has been made and this parameter has been specified.**  

Encrypting or disabling encryption may cause the VM to reboot.  

### Enable encryption on existing or running VMs with Azure PowerShell  

Use the [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/en-us/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet to enable encryption on a running IaaS virtual machine in Azure.  

#### Encrypt a running VM:  
The script below initializes your variables and runs the Set-AzVMDiskEncryptionExtension cmdlet. The resource group, VM, and key vault should have already been created as prerequisites. Replace MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, and MySecureVault with your values.  

```PowerShell  
$KVRGname = 'MyKeyVaultResourceGroup';
 $VMRGName = 'MyVirtualMachineResourceGroup';
 $vmName = 'MySecureVM';
 $KeyVaultName = 'MySecureVault';
 $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
 $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
 $KeyVaultResourceId = $KeyVault.ResourceId;

 Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
```  

#### Verify the disks are encrypted:  
To check on the encryption status of an IaaS VM, use the [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/en-us/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet.  

```PowerShell  
Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
```  
#### Disable disk encryption:  
To disable the encryption, use the [Disable-AzVMDiskEncryption](https://docs.microsoft.com/en-us/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet. Disabling data disk encryption on Windows VM when both OS and data disks have been encrypted doesn't work as expected. Disable encryption on all disks instead.  

```PowerShell  
Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
```  

### Enable encryption on existing or running VMs with Azure CLI  

Use the [az vm encryption enable](https://docs.microsoft.com/en-us/cli/azure/vm/encryption#az-vm-encryption-enable) command to enable encryption on a running IaaS virtual machine in Azure.  

#### Encrypt a Running VM:  
```PowerShell  
az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
```  

#### Verify the disks are encrypted:  
To check on the encryption status of an IaaS VM, use the [az vm encryption show](https://docs.microsoft.com/en-us/cli/azure/vm/encryption#az-vm-encryption-show) command.  
```PowerShell  
az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
```  

#### Disable encryption:  
To disable encryption, use the az vm encryption disable command. Disabling data disk encryption on Windows VM when both OS and data disks have been encrypted doesn't work as expected. Disable encryption on all disks instead.  

```PowerShell  
az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
```  

### Using the Resource Manager Template  

You can enable disk encryption on existing or running IaaS Windows VMs in Azure by using the [Resource Manager template to encrypt a running Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad).  

1. On the Azure quickstart template, click **Deploy to Azure.**  
2. Select the subscription, resource grouop, location, settings, legal terms, and agreement. Click **Purchase** to enable encryption on the existing or running IaaS VM.  

The following table lists the Resource Manager template parameters for existing or running VMs:  

| **Parameter** | **Description** |  
|---------------|-----------------|  
| vmName | Name of the VM to run the encryption operation |  
| keyVaultName | Name of the key vault that the BitLocker key should be uploaded to. You can get it by using the cmdlet (`Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>`). `Vaultname` or the Azure CLI command `az keyvault list --resource-group "MyKeyVaultResourceGroup"` |  
| KeyVaultResourceGroup | Name of the resource group that contains the key vault |  
| KeyEncryptionKeyURL | URL of the key encryption key that's used to encrypt the generated BitLocker key. This parameter is optional if you select **nokek** in the UseExistingKek drop-down list. If you select **kek** in the UseExistingKek drop-down list, you must enter the *keyEncryptionKeyURL* value. |  
| volumeType | Type of volume that the encryption operation is performed on. Valid values are *OS*, *Data*, and *All*. |  
| forceUpdateTag | Pass in a unique value like a GUID every time the operation needs to be force run. |  
| resizeOSDisk | Should the OS partition be resized to occupy full OS VHD before splitting system volume. |  
| location | Location for all resources |  

