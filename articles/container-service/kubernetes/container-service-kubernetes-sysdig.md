---
title: Surveiller le cluster Azure Kubernetes - Sysdig | Microsoft Docs
description: "Surveillance du cluster Kubernetes dans Azure Container Service à l’aide de Sysdig"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 64aff391b4fc358fac8622d4448bbb981e330e33
ms.contentlocale: fr-fr
ms.lasthandoff: 07/25/2017

---

# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Surveiller un cluster Kubernetes Azure Container Service avec Sysdig

## <a name="prerequisites"></a>Conditions préalables
Cette procédure pas à pas suppose que vous avez [créé un cluster Kubernetes à l’aide d’Azure Container Service](container-service-kubernetes-walkthrough.md).

Elle suppose également que vous avez installé les outils azure cli et kubectl.

Vous pouvez tester si l’outil `az` est installé en exécutant :

```console
$ az --version
```

Si l’outil `az` n’est pas installé, suivez les instructions figurant [ici](https://github.com/azure/azure-cli#installation).

Vous pouvez tester si l’outil `kubectl` est installé en exécutant :

```console
$ kubectl version
```

Si `kubectl` n’est pas installé, vous pouvez exécuter :

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a>Sysdig
Sysdig est une société de surveillance en tant que service pouvant analyser les conteneurs de votre cluster Kubernetes exécuté dans Azure. L’utilisation de Sysdig nécessite un compte Sysdig actif.
Vous pouvez créer un compte Azure sur leur [site](https://app.sysdigcloud.com).

Une fois connecté au site web du cloud Sysdig, cliquez sur votre nom d’utilisateur. La page qui s’affiche comporte votre clé d’accès (Access Key). 

![Clé d’API Sysdig](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a>Installation des agents Sysdig sur Kubernetes
Pour analyser vos conteneurs, Sysdig exécute un processus sur chaque machine à l’aide d’un `DaemonSet` Kubernetes.
Les DaemonSets sont des objets API Kubernetes qui exécutent une instance unique d’un conteneur par machine.
Ils sont parfaits pour installer des outils, tels que l’agent de surveillance de Sysdig.

Pour installer le daemonset Sysdig, vous devez d’abord télécharger [le modèle](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) à partir de sysdig. Enregistrez ce fichier sous `sysdig-daemonset.yaml`.

Sur Linux et OS X, vous pouvez exécuter :

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

Dans PowerShell :

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Modifiez ensuite ce fichier pour insérer votre clé d’accès que vous avez obtenue à partir de votre compte Sysdig.

Enfin, créez le DaemonSet :

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>Afficher votre analyse
Une fois installés et en cours d’exécution, les agents doivent extraire des données depuis Sysdig.  Revenez au [tableau de bord](https://app.sysdigcloud.com) et vous verrez des informations sur vos conteneurs.

Vous pouvez également installer des tableaux de bords spécifiques de Kubernetes en utilisant [l’Assistant Nouveau tableau de bord](https://app.sysdigcloud.com/#/dashboards/new).

