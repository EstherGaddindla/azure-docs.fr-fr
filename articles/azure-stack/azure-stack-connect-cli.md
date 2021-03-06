---
title: Connect to Azure Stack with CLI | Microsoft Docs
description: Learn how to use the cross-platform command-line interface (CLI) to manage and deploy resources on Azure Stack
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 596e8e6d872ab7d3de3f2147931a4f249bfd65b9
ms.contentlocale: fr-fr
ms.lasthandoff: 08/16/2017

---
# <a name="install-and-configure-cli-for-use-with-azure-stack"></a>Install and configure CLI for use with Azure Stack

In this document, we guide you through the process of using Azure Command-line Interface (CLI) to manage Azure Stack resources on Linux and Mac client platforms. You can use the steps described in this article either from the [Azure Stack Development Kit](azure-stack-connect-azure-stack.md#connect-with-remote-desktop) or from an external client if you are [connected through VPN](azure-stack-connect-azure-stack.md#connect-with-vpn).

## <a name="install-azure-stack-cli"></a>Install Azure Stack CLI

Azure Stack requires the 2.0 version of Azure CLI, which you can install by using the steps described in the [Install Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) article. To verify if the installation was successful, open a command prompt window and run the following command:

```azurecli
az --version
```

You should see the version of Azure CLI and other dependent libraries that are installed on your computer.

## <a name="connect-to-azure-stack"></a>Connect to Azure Stack

Use the following steps to connect to Azure Stack:

1. If your Azure Stack deployment doesn't contain a certificate that is issued by a publicly trusted certificate authority, you should add the root certificate to the Python certifi package store. If you are using CLI from a Windows-based OS, use the following script to add the certificate:

   ```powershell
    $label = "AzureStackSelfSignedRootCert"
    Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
    $root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
    if (-not $root)
    {
        Log-Error "Cerficate with subject CN=$label not found"
        return
    }

    Write-Host "Exporting certificate"
    Export-Certificate -Type CERT -FilePath root.cer -Cert $root

    Write-Host "Converting certificate to PEM format"
    certutil -encode root.cer root.pem

    Write-Host "Extracting needed information from the cert file"
    $md5Hash=(Get-FileHash -Path root.pem -Algorithm MD5).Hash.ToLower()
    $sha1Hash=(Get-FileHash -Path root.pem -Algorithm SHA1).Hash.ToLower()
    $sha256Hash=(Get-FileHash -Path root.pem -Algorithm SHA256).Hash.ToLower()

    $issuerEntry = [string]::Format("# Issuer: {0}", $root.Issuer)
    $subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
    $labelEntry = [string]::Format("# Label: {0}", $label)
    $serialEntry = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
    $md5Entry = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
    $sha1Entry  = [string]::Format("# SHA1 Finterprint: {0}", $sha1Hash)
    $sha256Entry = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
    $certText = (Get-Content -Path root.pem -Raw).ToString().Replace("`r`n","`n")

    $rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
    $serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

    Write-Host "Adding the certificate content to Python Cert store"
    Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

    Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate" 
   ```

2. Register your Azure Stack environment by running the az cloud register command. 
   
   In order to create virtual machines by using CLI, the cloud administrator should set up a publicly accessible endpoint that contains virtual machine image aliases and register this endpoint with the cloud. The `endpoint-vm-image-alias-doc` parameter in the `az cloud register` command is used for this purpose. Cloud administrators must download the image to the Azure Stack marketplace before they add it to image aliases endpoint.
   
   For example, Azure contains uses following URI: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. The cloud administrator should set up a similar endpoint for Azure Stack with the images that are available in the Azure Stack marketplace.

   a. To register the **cloud administrative** environment, use:

   ```azurecli
   az cloud register \ 
     -n AzureStackAdmin \ 
     --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
     --suffix-storage-endpoint "local.azurestack.external" \ 
     --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
     --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
     --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
   ```

   b. To register the **user** environment, use:

   ```azurecli
   az cloud register \ 
     -n AzureStackUser \ 
     --endpoint-resource-manager "https://management.local.azurestack.external" \ 
     --suffix-storage-endpoint "local.azurestack.external" \ 
     --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
     --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
     --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
   ```

3. Set the active environment by using the following commands:

   a. For the **cloud administrative** environment, use:

   ```azurecli
   az cloud set \
     -n AzureStackAdmin
   ```

   b. For the **user** environment, use:

   ```azurecli
   az cloud set \
     -n AzureStackUser
   ```

4. Update your environment configuration to use the Azure Stack specific API version profile. To update the configuration, run the following command:

   ```azurecli
   az cloud update \
     --profile 2017-03-09-profile
   ```

5. Sign in to your Azure Stack environment by using the **az login** command. You can sign in to the Azure Stack environment either as a user or as a [service principal](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects). 

   * Log in as a **user**: You can either specify the username and password directly within the az login command or authenticate using a browser. You would have to do the latter, if your account has multi-factor authentication enabled.

   ```azurecli
   az login \
     -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
     --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
   ```

   **Note** If your user account has Multi factor authentication enabled, you can use the az login command without providing the -u parameter. Running the command gives you a URL and a code that you must use to authenticate.
   
   * Log in as a **service principal**: [Create a service principal through the Azure portal](azure-stack-create-service-principals.md) or CLI and assign it to a role for the scope you would like for it to have access to. Af log in using the service principal using the following command:

   ```azurecli
   az login \
     --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
     --service-principal \
     -u <Application Id of the Service Principal> \
     -p <Key generated for the Service Principal>
   ```

## <a name="test-the-connectivity"></a>Test the connectivity

Now that we've got everything setup, let's use CLI to create resources within Azure Stack. For example, you can create a resource group for an application and add a virtual machine. Use the following command to create a resource group named "MyResourceGroup":

```azurecli
az group create \
  -n MyResourceGroup -l local
```

If the resource group is created successfully, the previous command outputs the following properties of the newly created resource:

![resource group create output](media/azure-stack-connect-cli/image1.png)

There are some known issues when using CLI 2.0 in Azure Stack, to learn about these issues, see the [Known issues in Azure Stack CLI](azure-stack-troubleshooting.md#cli) topic. 


## <a name="next-steps"></a>Next steps

[Deploy templates with Azure CLI](azure-stack-deploy-template-command-line.md)

[Connect with PowerShell](azure-stack-connect-powershell.md)

[Manage user permissions](azure-stack-manage-permissions.md)


