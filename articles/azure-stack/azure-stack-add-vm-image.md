---
title: Adding a VM image to Azure Stack | Microsoft Docs
description: Add your organization's custom Windows or Linux VM image for tenants to use
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: de8540397b63093457382cf427a65ea0e48b93e0
ms.contentlocale: it-it
ms.lasthandoff: 09/25/2017

---
# <a name="make-a-custom-virtual-machine-image-available-in-azure-stack"></a>Make a custom virtual machine image available in Azure Stack

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

Azure Stack enables operators to make custom virtual machine images available to their users. These images can be referenced by Azure Resource Manager templates or added to the Azure Marketplace UI with the creation of a Marketplace item. 

## <a name="add-a-vm-image-to-marketplace-with-powershell"></a>Add a VM image to marketplace with PowerShell

Run the following prerequisites either from the [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), or from a Windows-based external client if you are [connected through VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)

* [Install PowerShell for Azure Stack](azure-stack-powershell-install.md).  

* Download the [tools required to work with Azure Stack](azure-stack-powershell-download.md).  

* Prepare a Windows or Linux operating system virtual hard disk image in VHD format (not VHDX).
   
   * For Windows images, the article [Upload a Windows VM image to Azure for Resource Manager deployments](../virtual-machines/windows/upload-generalized-managed.md) contains image preparation instructions in the **Prepare the VHD for upload** section.
   * For Linux images, follow the steps to prepare the image or use an existing Azure Stack Linux image as described in the article [Deploy Linux virtual machines on Azure Stack](azure-stack-linux.md).  

Now run the following steps to add the image to the Azure Stack marketplace:

1. Import the Connect and ComputeAdmin modules:
   
   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # import the Connect and ComputeAdmin modules
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1
   ``` 

2. Sign in to your Azure Stack environment. Run the following script depending on if your Azure Stack environment is deployed by using AAD or AD FS (Make sure to replace the AAD tenantName, GraphAudience endpoint and ArmEndpoint values as per your environment configuration): 

   a. **Azure Active Directory**, use the following cmdlet:

   ```PowerShell
   # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
   $ArmEndpoint = "<Resource Manager endpoint for your environment>"

   # For Azure Stack development kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
   $GraphAudience = "<GraphAuidence endpoint for your environment>"

   #Create the Azure Stack operator's AzureRM environment by using the following cmdlet:
   Add-AzureRMEnvironment `
     -Name "AzureStackAdmin" `
     -ArmEndpoint $ArmEndpoint 

   Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience $GraphAudience

   $TenantID = Get-AzsDirectoryTenantId `
     -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
     -EnvironmentName AzureStackAdmin

   Login-AzureRmAccount `
     -EnvironmentName "AzureStackAdmin" `
     -TenantId $TenantID 
   ```

   b. **Active Directory Federation Services**, use the following cmdlet:
    
   ```PowerShell
   # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
   $ArmEndpoint = "<Resource Manager endpoint for your environment>"

   # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
   $GraphAudience = "<GraphAuidence endpoint for your environment>"

   # Create the Azure Stack operator's AzureRM environment by using the following cmdlet:
   Add-AzureRMEnvironment `
     -Name "AzureStackAdmin" `
     -ArmEndpoint $ArmEndpoint

   Set-AzureRmEnvironment `
     -Name "AzureStackAdmin" `
     -GraphAudience $GraphAudience `
     -EnableAdfsAuthentication:$true

   $TenantID = Get-AzsDirectoryTenantId `
     -ADFS 
     -EnvironmentName AzureStackAdmin 

   Login-AzureRmAccount `
     -EnvironmentName "AzureStackAdmin" `
     -TenantId $TenantID 
   ```
    
3. Add the VM image by invoking the `Add-AzsVMImage` cmdlet. In the Add-AzsVMImage cmdlet, specify the osType as Windows or Linux. Include the publisher, offer, SKU, and version for the VM image. See the [Parameters](#parameters) section for information about the allowed parameters. These parameters are used by Azure Resource Manager templates to reference the VM image. Following is an example invocation of the script:
     
     ```powershell
     Add-AzsVMImage `
       -publisher "Canonical" `
       -offer "UbuntuServer" `
       -sku "14.04.3-LTS" `
       -version "1.0.0" `
       -osType Linux `
       -osDiskLocalPath 'C:\Users\AzureStackAdmin\Desktop\UbuntuServer.vhd' `
     ```

The command does the following:

* Authenticates to the Azure Stack environment
* Uploads the local VHD to a newly created temporary storage account
* Adds the VM image to the VM image repository and
* Creates a Marketplace item

To verify that the command ran successfully, go to Marketplace in the portal, and then verify that the VM image is available in the **Virtual Machines** category.

![VM image added successfully](./media/azure-stack-add-vm-image/image5.PNG) 

## <a name="remove-a-vm-image-with-powershell"></a>Remove a VM image with PowerShell

When you no longer need the virtual machine image that you have uploaded earlier, you can delete it from the marketplace by using the following cmdlet:

```powershell
Remove-AzsVMImage `
  -publisher "Canonical" `
  -offer "UbuntuServer" `
  -sku "14.04.3-LTS" `
  -version "1.0.0" `
```

## <a name="parameters"></a>Parameters

| Parameter | Description |
| --- | --- |
| **publisher** |The publisher name segment of the VM image that users use when deploying the image. An example is ‘Microsoft’. Do not include a space or other special characters in this field. |
| **offer** |The offer name segment of the VM Image that users use when deploying the VM image. An example is ‘WindowsServer’. Do not include a space or other special characters in this field. |
| **sku** |The SKU name segment of the VM Image that users use when deploying the VM image. An example is ‘Datacenter2016’. Do not include a space or other special characters in this field. |
| **version** |The version of the VM Image that users use when deploying the VM image. This version is in the format *\#.\#.\#*. An example is ‘1.0.0’. Do not include a space or other special characters in this field. |
| **osType** |The osType of the image must be either ‘Windows’ or ‘Linux’. |
| **osDiskLocalPath** |The local path to the OS disk VHD that you are uploading as a VM image to Azure Stack. |
| **dataDiskLocalPaths** |An optional array of the local paths for data disks that can be uploaded as part of the VM image. |
| **CreateGalleryItem** |A Boolean flag that determines whether to create an item in Marketplace. By default, it is set to true. |
| **title** |The display name of Marketplace item. By default, it is set to the Publisher-Offer-Sku of the VM image. |
| **description** |The description of the Marketplace item. |
| **location** |The location to which the VM image should be published. By default, this value is set to local.|
| **osDiskBlobURI** |Optionally, this script also accepts a Blob storage URI for osDisk. |
| **dataDiskBlobURIs** |Optionally, this script also accepts an array of Blob storage URIs for adding data disks to the image. |

## <a name="add-a-vm-image-through-the-portal"></a>Add a VM image through the portal

> [!NOTE]
> This method requires creating the Marketplace item separately.

One requirement of images is that they can be referenced by a Blob storage URI. Prepare a Windows or Linux operating system image in VHD format (not VHDX), and then upload the image to a storage account in Azure or Azure Stack. If your image is already uploaded to the Blob storage in Azure or Azure Stack, you can skip step1.

1. [Upload a Windows VM image to Azure for Resource Manager deployments](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) or for a Linux image, follow the instructions described in the [Deploy Linux virtual machines on Azure Stack](azure-stack-linux.md) article. You should understand the following considerations before you upload the image:

   * It's more efficient to upload an image to Azure Stack Blob storage than to Azure Blob storage because it takes less time to push the image to the Azure Stack image repository. 
   
   * When uploading the [Windows VM image](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), make sure to substitute the **Login to Azure** step with the [Configure the Azure Stack operator's PowerShell environment](azure-stack-powershell-configure-admin.md)  step.  

   * Make a note of the Blob storage URI where you upload the image, which is in the following format: *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;*.vhd

   * To make the blob anonymously accessible, go to the storage account blob container where the VM image VHD was uploaded to **Blob,** and then select **Access Policy**. If you want, you can instead generate a shared access signature for the container and include it as part of the blob URI.

   ![Navigate to storage account blobs](./media/azure-stack-add-vm-image/image1.png)

   ![Set blob access to public](./media/azure-stack-add-vm-image/image2.png)

2. Sign in to Azure Stack as operator > From the menu, click **More services** > **Resource Providers** > select  **Compute** > **VM images** > **Add**

3. On the **Add a VM Image** blade, enter the publisher, offer, SKU, and version of the virtual machine image. These name segments refer to the VM image in Resource Manager templates. Make sure to select the **osType** correctly. For **OD Disk Blob URI**, enter the Blob URI where the image was uploaded and click **Create** to begin creating the VM Image.
   
   ![Begin to create the image](./media/azure-stack-add-vm-image/image4.png)

   When the image is successfully created, the VM image status changes to ‘Succeeded’.

4. To make the virtual machine image more readily available for user consumption in the UI, it is best to [create a Marketplace item](azure-stack-create-and-publish-marketplace-item.md).

## <a name="next-steps"></a>Next steps

[Provision a virtual machine](azure-stack-provision-vm.md)
