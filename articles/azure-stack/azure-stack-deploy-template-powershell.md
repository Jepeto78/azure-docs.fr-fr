---
title: Deploy templates with PowerShell in Azure Stack | Microsoft Docs
description: Learn how to deploy a virtual machine using a Resource Manager template and PowerShell.
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: e32856f678f5e0fd5472f035b61981bc214e1ac3
ms.contentlocale: fr-fr
ms.lasthandoff: 07/10/2017


---
# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Deploy templates in Azure Stack using PowerShell
Use PowerShell to deploy Azure Resource Manager templates to the Azure Stack Development Kit.  Resource Manager templates deploy and provision all resources for your application in a single, coordinated operation.

## <a name="run-azurerm-powershell-cmdlets"></a>Run AzureRM PowerShell cmdlets
In this example, you run a script to deploy a virtual machine to Azure Stack Development Kit using a Resource Manager template.  Before proceeding, ensure you have [configured PowerShell](azure-stack-powershell-configure.md)  

The VHD used in this example template is WindowsServer-2012-R2-Datacenter.

1. Go to <http://aka.ms/AzureStackGitHub>, search for the **101-simple-windows-vm** template, and save it to the following location: c:\\templates\\azuredeploy-101-simple-windows-vm.json.
2. In PowerShell, run the following deployment script. Replace *username* and *password* with your username and password. On subsequent uses, increment the value for the *$myNum* parameter to prevent overwriting your deployment.
   
   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```
3. Open the Azure Stack portal, click **Browse**, click **Virtual machines**, and look for your new virtual machine (*myDeployment001*).


## <a name="next-steps"></a>Next steps
[Deploy templates with Visual Studio](azure-stack-deploy-template-visual-studio.md)


