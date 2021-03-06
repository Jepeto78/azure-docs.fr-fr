---
title: "Utilisation de PowerShell pour gérer le Stockage Fichier Azure | Microsoft Docs"
description: "Découvrez comment utiliser PowerShell pour gérer le Stockage Fichier Azure."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 5d825c1139d49fb8d37c719d8e0ab8ba2da2ed8d
ms.contentlocale: fr-fr
ms.lasthandoff: 07/11/2017

---
# <a name="how-to-use-powershell-to-manage-azure-file-storage"></a>Utilisation de PowerShell pour gérer le Stockage Fichier Azure
Vous pouvez utiliser Azure PowerShell pour créer et gérer des partages de fichiers.

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a>Installation des applets de commande PowerShell pour Azure Storage
Pour vous préparer à utiliser PowerShell, téléchargez et installez les applets de commande PowerShell Azure. Consultez la rubrique [Installation et configuration d’Azure PowerShell](/powershell/azureps-cmdlets-docs) pour des instructions sur l’installation et le point d’installation.

> [!NOTE]
> Il est recommandé de télécharger et d’installer le dernier module Azure PowerShell ou d’effectuer une mise à niveau vers celui-ci.
> 
> 

Ouvrez une fenêtre Azure PowerShell en cliquant sur **Démarrer** et en saisissant **Windows PowerShell**. La fenêtre PowerShell charge automatiquement le module Azure PowerShell.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Création d'un contexte pour votre compte de stockage et votre clé
Créez le contexte du compte de stockage. Celui-ci encapsule le nom et la clé du compte de stockage. Pour obtenir des instructions sur la copie de votre clé de compte à partir du [portail Azure](https://portal.azure.com), voir [Afficher et copier les clés d’accès de stockage](storage-create-storage-account.md#view-and-copy-storage-access-keys).

Remplacez `storage-account-name` et `storage-account-key` par le nom et la clé de votre compte de stockage dans l’exemple suivant.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Création d’un partage de fichiers
Créez le nouveau partage nommé `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Vous disposez désormais d’un partage de fichier dans le stockage de fichiers. Nous allons maintenant ajouter un répertoire et un fichier.

> [!IMPORTANT]
> Le nom de votre partage de fichiers doit être en minuscules. Pour plus d’informations sur la façon de nommer des partages de fichiers et des fichiers, consultez la rubrique [Affectation de noms et références aux partages, répertoires, fichiers et métadonnées](https://msdn.microsoft.com/library/azure/dn167011.aspx).
> 
> 

## <a name="create-a-directory-in-the-file-share"></a>Création d’un répertoire dans le partage de fichiers
Créez un répertoire dans le partage. Dans l’exemple suivant, le répertoire est nommé `CustomLogs`.

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a>Chargement d’un fichier local vers le répertoire
Chargez un fichier local vers le répertoire. L’exemple suivant charge un fichier à partir de `C:\temp\Log1.txt`. Modifiez le chemin d’accès du fichier de façon à ce qu’il pointe vers un fichier valide sur votre ordinateur local.

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a>Affichage de la liste des fichiers du répertoire
Pour voir le fichier dans le répertoire, vous pouvez afficher la liste de tous fichiers du répertoire. Cette commande renvoie les fichiers et les sous-répertoires (le cas échéant) dans le répertoire CustomLogs.

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Get-AzureStorageFile renvoie une liste de fichiers et de répertoires pour répertoire vers lequel l’objet est transmis. « Get-AzureStorageFile-partager $s » renvoie une liste de fichiers et de répertoires dans le répertoire racine. Pour obtenir une liste des fichiers dans un sous-répertoire, vous devez transmettre le sous-répertoire à Get-AzureStorageFile. Le processus est le suivant : la première partie de la commande jusqu’à la barre renvoie une instance de répertoire du sous-répertoire CustomLogs. Elle est ensuite transmise à Get-AzureStorageFile, qui renvoie les répertoires et fichiers dans CustomLogs.

## <a name="copy-files"></a>Copie des fichiers
Depuis la version 0.9.7 d’Azure PowerShell, vous pouvez copier un fichier dans un autre fichier, un fichier dans un objet blob ou un objet blob dans un fichier. Nous montrons ci-dessous comment effectuer ces opérations en utilisant des applets de commande PowerShell.

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur le stockage de fichiers Azure, consultez ces liens.

* [FAQ](storage-files-faq.md)
* [Dépannage](storage-troubleshoot-file-connection-problems.md)
