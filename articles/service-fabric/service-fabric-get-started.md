---
title: "Configurer un environnement de développement pour les microservices Azure | Microsoft Docs"
description: "Installez le runtime, le kit de développement logiciel et créez un cluster de développement local. Une fois l’installation terminée, vous serez prêt à créer des applications."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/20/2017
ms.author: ryanwi, mikhegn
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 926dfe3de0715f855e6d5b57f10c2366cda8583b
ms.contentlocale: fr-fr
ms.lasthandoff: 06/21/2017


---
<a id="prepare-your-development-environment" class="xliff"></a>

# Préparer votre environnement de développement
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Pour générer et exécuter des [applications Azure Service Fabric][1] sur votre ordinateur de développement, installez le runtime, le Kit de développement logiciel (SDK) et les outils. Vous devez également activer l’exécution des scripts Windows PowerShell inclus dans le Kit de développement logiciel (SDK).

<a id="prerequisites" class="xliff"></a>

## Composants requis
<a id="supported-operating-system-versions" class="xliff"></a>

### Versions du système d’exploitation prises en charge
Les versions de système d’exploitation prises en charge pour le développement sont les suivantes :

* Windows 7
* Windows 8 et Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 inclut uniquement Windows PowerShell 2.0 par défaut. Les applets de commande PowerShell de Service Fabric nécessitent PowerShell 3.0 ou version ultérieure. Vous pouvez [télécharger Windows PowerShell 5.0][powershell5-download] à partir du Centre de téléchargement Microsoft.
> 
> 

<a id="install-the-sdk-and-tools" class="xliff"></a>

## Installer le Kit de développement logiciel (SDK) et les outils
<a id="to-use-visual-studio-2017" class="xliff"></a>

### Pour utiliser Visual Studio 2017
Les outils Service Fabric font partie de la charge de travail de développement et de gestion Azure dans Visual Studio 2017. Activez cette charge de travail dans le cadre de votre installation de Visual Studio.
En outre, vous devez installer le Kit de développement logiciel (SDK) Microsoft Azure Service Fabric, à l’aide de Web Platform Installer.

* [Installer le Kit de développement logiciel (SDK) Microsoft Azure Service Fabric][core-sdk]

<a id="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later" class="xliff"></a>

### Pour utiliser Visual Studio 2015 (requiert Visual Studio 2015 Update 2 ou une version ultérieure)
Pour Visual Studio 2015, les outils Service Fabric sont installés avec le Kit de développement logiciel (SDK), à l’aide de Web Platform Installer :

* [Installer le Kit de développement logiciel (SDK) et les outils de Microsoft Azure Service Fabric][full-bundle-vs2015]

<a id="sdk-installation-only" class="xliff"></a>

### Installation du Kit de développement logiciel (SDK) uniquement
Si vous avez uniquement besoin du SDK, vous pouvez installer ce package :
* [Installer le Kit de développement logiciel (SDK) Microsoft Azure Service Fabric][core-sdk]

Les versions actuelles sont les suivantes :
* Kit de développement logiciel (SDK) Service Fabric 2.6.220
* Runtime Service Fabric 5.6.220
* Outils Visual Studio 2015 1.6.50508.2
* Visual Studio 2017 Mise à jour 2

Les préversions actuelles sont les suivantes :
* Kit de développement logiciel (SDK) Service Fabric 255.255.2718.255
* Runtime Service Fabric 255.255.5718.255
* Outils Visual Studio 2015 1.6.50509.5
* Visual Studio 2017 Mise à jour 3, préversion 1

Pour obtenir la liste des versions prises en charge, consultez l’article [Service Fabric support (Options de prise en charge de Service Fabric)](service-fabric-support.md).

<a id="enable-powershell-script-execution" class="xliff"></a>

## Activer l'exécution du script PowerShell
Service Fabric utilise des scripts Windows PowerShell pour créer un cluster de développement local et déployer des applications à partir de Visual Studio. Par défaut, Windows bloque l’exécution de ces scripts. Pour les activer, vous devez modifier votre stratégie d'exécution PowerShell. Ouvrez PowerShell en tant qu'administrateur et entrez la commande suivante :

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

<a id="next-steps" class="xliff"></a>

## Étapes suivantes
Maintenant que vous avez fini de configurer votre environnement de développement, commencez à créer et à exécuter des applications.

* [Créer votre première application Service Fabric dans Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Découvrez comment déployer et gérer des applications sur votre cluster local](service-fabric-get-started-with-a-local-cluster.md)
* [En savoir plus sur les modèles de programmation : acteurs fiables et services fiables](service-fabric-choose-framework.md)
* [Consulter les exemples de code Service Fabric sur GitHub](https://aka.ms/servicefabricsamples)
* [Visualiser votre cluster à l’aide de l’outil Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Suivre le parcours d’apprentissage Service Fabric pour une introduction générale à la plate-forme](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* En savoir plus sur les [options de prise en charge de Service Fabric](service-fabric-support.md)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Page de campagne Service Fabric"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Lien WebPI VS 2015"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Lien WebPI Dev15"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Lien WebPI du Kit de développement logiciel principal"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395

