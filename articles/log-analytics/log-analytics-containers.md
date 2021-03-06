---
title: Solution Conteneurs dans Azure Log Analytics | Microsoft Docs
description: "La solution Conteneurs dans Log Analytics vous aide à afficher et gérer vos hôtes de conteneur Docker et Windows dans un emplacement unique."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: banders
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 524c63d358ce22c10b7a23e5bcf0b33e9f2e5f26
ms.contentlocale: fr-fr
ms.lasthandoff: 08/03/2017

---
# <a name="containers-preview-solution-in-log-analytics"></a>Solution Containers (préversion) dans Log Analytics

![Symbole Containers](./media/log-analytics-containers/containers-symbol.png)

Cet article décrit comment configurer et utiliser la solution Conteneurs dans Log Analytics, vous aide à afficher et gérer vos hôtes de conteneur Docker et Windows dans un emplacement unique. Docker est un système de virtualisation logicielle utilisé pour créer des conteneurs qui automatisent le déploiement de logiciels dans leur infrastructure informatique.

La solution vous permet de voir les conteneurs exécutés sur les hôtes de votre conteneur, et les images exécutées dans les conteneurs. Vous pouvez afficher des informations d’audit détaillées montrant les commandes utilisées avec les conteneurs. Vous pouvez résoudre des problèmes de conteneurs en consultant des journaux centralisés et en y effectuant des recherches sans devoir afficher à distance les hôtes Docker ou Windows. Vous pouvez rechercher des conteneurs bruyants et consommant des ressources excessives sur un ordinateur hôte. Et vous pouvez consulter des informations centralisées sur le processeur, la mémoire, le stockage ainsi que l’utilisation et les performances du réseau. Sur les ordinateurs exécutant Windows, vous pouvez centraliser et comparer les journaux des conteneurs Windows Server, Hyper-V et Docker.

Le schéma suivant illustre les relations entre les différents hôtes de conteneurs et agents dans OMS.

![Schéma des conteneurs](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a>Conditions requises pour le système
Avant de commencer, prenez connaissance des informations suivantes pour vérifier que les conditions préalables sont remplies.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Prise en charge de solution de surveillance de conteneur pour la plateforme Docker Orchestrator et celle du système d’exploitation 
Le tableau suivant présente l’orchestration de Docker et la prise en charge de la surveillance du système d’exploitation pour ce qui est des journaux, de la performance et de l’inventaire du conteneur avec Log Analytics.   

| | ACS | Linux | Windows | Conteneur<br>Inventaire | Image<br>Inventaire | Nœud<br>Inventaire | Conteneur<br>Performances | Conteneur<br>Événement | Événement<br>Journal | Conteneur<br>Journal | 
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| kubernetes | Oui | Oui | | Oui | Oui | Oui | Oui | Oui | Oui | Oui | 
| Mésosphère<br>DC/OS | Oui | Oui | | Oui | Oui | Oui | Oui| Oui | Oui | Oui | 
| Docker<br>Swarm | Oui | Oui | Oui | Oui | Oui | Oui | Oui | Oui | | Oui |
| Service<br>Structure | | | Oui | Oui | Oui | Oui | Oui | Oui | Oui | Oui | 
| Red Hat Open<br>Shift | | Oui | | Oui | Oui| Oui | Oui | Oui | | Oui | 
| Windows Server<br>(autonome) | | | Oui | Oui | Oui | Oui | Oui | Oui | | Oui |
| Serveur Linux<br>(autonome) | | Oui | | Oui | Oui | Oui | Oui | Oui | | Oui |


### <a name="supported-linux-operating-system"></a>Système d’exploitation Linux pris en charge

- Docker 1.11 jusqu’à 1.13
- Docker CE et EE v17.03

Les distributions Linux x64 suivantes sont prises en charge en tant qu’hôtes de conteneur :

- Ubuntu 14.04 LTS, 16.04 LTS, 15.04, 15.10
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE LEAP 42.2
- CentOS 7.2, 7.3
- SLES 12
- RHEL 7.2, 7.3

### <a name="supported-windows-operating-system"></a>Système d’exploitation Windows pris en charge

- Windows Server 2016
- Édition Anniversaire Windows 10 (Professionnel ou Entreprise)

### <a name="docker-versions-supported-on-windows"></a>Versions de docker prises en charge sur Windows

- Docker 1.12 - 1.13
- Docker 17.03.0 

## <a name="installing-and-configuring-the-solution"></a>Installation et configuration de la solution
Utilisez les informations suivantes pour installer et configurer la solution.

Ajoutez la solution Conteneurs à votre espace de travail OMS depuis la [Place de marché Azure](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) ou en procédant de la manière décrite dans [Ajouter des solutions Log Analytics à partir de la galerie de solutions](log-analytics-add-solutions.md).

Il existe différentes façons d’installer et d’utiliser Docker avec OMS :

* Sur les systèmes d’exploitation Linux pris en charge, installez et exécutez Docker, puis installez et configurez l’Agent OMS pour Linux.
* Sur CoreOS, vous ne pouvez pas exécuter l’Agent OMS pour Linux. Au lieu de cela, vous exécutez une version en conteneur de l’Agent OMS pour Linux.
* Sur Windows Server 2016 et Windows 10, installez le moteur et le client Docker, puis connectez un agent afin de collecter les données et les transmettre à Log Analytics.


Sur [GitHub](https://github.com/Microsoft/OMS-docker), vous pouvez passer en revue les versions de Docker et du système d’exploitation Linux prises en charge pour votre hôte de conteneur.

### <a name="container-services"></a>Services de conteneur

- Si vous possédez un cluster Kubernetes qui utilise Azure Container Service, consultez l’article [Surveiller un cluster Azure Container Service avec Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).
- Si vous possédez un cluster DC/OS Azure Container Service, consultez l’article [Surveiller un cluster DC/OS Azure Container Service avec Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).
- Si vous avez un environnement en mode Docker Swarm, apprenez en plus en lisant [Configurer un agent OMS pour Docker Swarm](#configure-an-oms-agent-for-docker-swarm).
- Si vous utilisez des conteneurs avec Service Fabric, reportez-vous à [Vue d’ensemble d’Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Examinez l’article relatif au [moteur Docker sur Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) pour en savoir plus sur l’installation et la configuration de vos moteurs Docker sur les ordinateurs exécutant Windows.

> [!IMPORTANT]
> Docker doit être en cours d’exécution **avant** l’installation de l’[Agent OMS pour Linux](log-analytics-agent-linux.md) sur vos hôtes de conteneur. Si vous avez déjà installé l’agent avant d’installer Docker, vous devez réinstaller l’Agent OMS pour Linux. Pour plus d’informations sur Docker, voir le [site web Docker](https://www.docker.com).
>
>

Pour pouvoir analyser les conteneurs, vous devez avoir préalablement configuré les paramètres suivants sur vos hôtes de conteneur.

## <a name="linux-container-hosts"></a>Hôtes de conteneur Linux

Après avoir installé Docker, utilisez les paramètres suivants pour votre hôte de conteneur afin de configurer l’agent en vue d’une utilisation avec Docker. Vous devez disposer des [ID et clé de votre espace de travail OMS](log-analytics-agent-linux.md).


### <a name="for-all-linux-container-hosts-except-coreos"></a>Pour tous les hôtes de conteneur Linux, à l’exception de CoreOS

- Pour plus d’informations sur la façon d’installer l’Agent OMS sous Linux, consultez [Connecter vos ordinateurs Linux à Operations Management Suite (OMS)](log-analytics-agent-linux.md).

### <a name="for-all-linux-container-hosts-including-coreos"></a>Pour tous les hôtes de conteneur Linux, avec CoreOS

Démarrez le conteneur OMS que vous souhaitez analyser. Modifiez et utilisez l’exemple suivant.

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a>Pour tous les hôtes de conteneur Linux Azure Government, y compris CoreOS

Démarrez le conteneur OMS que vous souhaitez analyser. Modifiez et utilisez l’exemple suivant.

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```


### <a name="switching-from-using-an-installed-linux-agent-to-one-in-a-container"></a>Passage de l’utilisation d’un agent installé Linux à un agent dans un conteneur
Si vous utilisiez précédemment l’agent directement installé et souhaitez désormais utiliser un agent qui s’exécute dans un conteneur, vous devez commencer par supprimer l’agent OMS pour Linux. Voir [Désinstallation de l’Agent OMS pour Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux).

### <a name="configure-an-oms-agent-for-docker-swarm"></a>Configurer un agent OMS pour Docker Swarm

Vous pouvez exécuter l’agent OMS en tant que service global sur Docker Swarm. Utilisez les informations suivantes pour créer un service d’agent OMS. Vous devez insérer votre ID d’espace de travail et votre clé primaire.

- Exécutez la commande suivante sur le nœud principal.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="secure-your-secret-information-for-container-services"></a>Sécuriser vos informations confidentielles pour les services de conteneur

Vous pouvez sécuriser votre ID d’espace de travail OMS et vos clés primaires secrets pour Docker Swarm et Kubernetes.

#### <a name="secure-secrets-for-docker-swarm"></a>Sécuriser les secrets pour Docker Swarm

Pour Docker Swarm, une fois les secrets de l’ID de l’espace de travail et de la clé primaire créés, vous pouvez exécuter le service Docker pour l’agent OMS. Utilisez les informations suivantes pour créer vos informations secrètes.

1. Exécutez la commande suivante sur le nœud principal.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Vérifiez que les secrets ont été créés correctement.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Exécutez la commande suivante pour monter les secrets sur l’agent OMS en conteneur.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a>Sécuriser des secrets pour Kubernetes avec des fichiers yaml

Pour Kubernetes, vous utilisez un script afin de générer le fichier .yaml de secrets pour votre ID d’espace de travail et votre clé primaire. Sur la page [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) (GitHub Kubernetes Docker OMS), il existe des fichiers que vous pouvez utiliser avec ou sans vos informations secrètes.

- Le DaemonSet par défaut de l’agent OMS qui ne dispose pas d’informations secrètes (omsagent.yaml).
- Le fichier yaml DaemonSet de l’agent OMS qui utilise les informations secrètes (omsagent-ds-secrets.yaml) avec des scripts de génération de secrets qui génèrent le fichier yaml de secrets (omsagentsecret.yaml).

Vous pouvez choisir de créer le DaemonSet de l’agent OMS avec ou sans secrets.

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a>Le fichier yaml par défaut DaemonSet de l’agent OMS sans secrets

- Pour le fichier yaml par défaut DaemonSet de l’agent OMS, remplacez `<WSID>` et `<KEY>` par votre ID d’espace de travail et clé. Copiez le fichier sur votre nœud principal et exécutez la commande suivante :

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a>Le fichier yaml par défaut DaemonSet de l’agent OMS avec secrets

1. Pour utiliser le DaemonSet de l’agent OMS à l’aide des informations secrètes, créez d’abord les secrets.
    1. Copiez le fichier de modèle de secret et le script, et assurez-vous qu’ils se trouvent dans le même répertoire.
        - Script de génération de secrets - secret-gen.sh
        - Modèle de secret - secret-template.yaml
    2. Exécutez le script, comme l’exemple suivant. Le script demande l’ID d’espace de travail OMS et la clé primaire, et une fois que vous les entrez, le script crée un fichier .yaml de secrets que vous pouvez exécuter.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Créez le pod de secrets en exécutant la commande suivante :
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Pour vérifier, exécutez la commande suivante :

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        La sortie doit ressembler à :

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        La sortie doit ressembler à :

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Créer votre DaemonsSet de l’agent OMS en exécutant ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. Vérifiez que le DaemonSet de l’agent OMS s’exécute, comme ce qui suit :

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Pour Kubernetes, utilisez un script afin de générer le fichier .yaml de secrets pour l’ID d’espace de travail et la clé primaire. Utilisez les informations de l’exemple suivant avec le [fichier yaml de l’agent OMS](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) pour sécuriser vos informations secrètes.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```


## <a name="windows-container-hosts"></a>Hôtes de conteneur Windows

### <a name="preparation-before-installing-windows-agents"></a>Préparation préalable à l’installation des agents Windows

Avant d’installer les agents sur les ordinateurs exécutant Windows, vous devez configurer le service Docker. La configuration permet à l’agent Windows ou à l’extension de machine virtuelle Log Analytics d’utiliser le socket Docker TCP afin d’autoriser les agents à accéder à distance au démon Docker et de collecter les données pour la surveillance.

#### <a name="to-start-docker-and-verify-its-configuration"></a>Pour démarrer Docker et vérifier sa configuration

Voici les étapes nécessaires à la configuration du canal nommé TCP pour Windows Server :

1. Dans Windows PowerShell, activez les canaux TCP et nommé.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Configurez Docker à l’aide du fichier de configuration pour le canal TCP et le canal nommé. Le fichier de configuration se trouve à l’emplacement suivant : C:\ProgramData\docker\config\daemon.json.

    Dans le fichier daemon.json, vous aurez besoin des éléments suivants :

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Pour plus d’informations sur la configuration du démon Docker utilisée avec les conteneurs de Windows, reportez-vous à [Moteur Docker sur Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


### <a name="install-windows-agents"></a>Installer les agents Windows

Pour activer la surveillance des conteneurs Windows et Hyper-V, installez les agents sur les ordinateurs Windows qui sont des hôtes de conteneurs. Pour les ordinateurs exécutant Windows dans votre environnement local, consultez la page [Connecter des ordinateurs Windows à Log Analytics](log-analytics-windows-agents.md). Connectez les machines virtuelles exécutées dans Azure à Log Analytics à l’aide de l’[extension de machine virtuelle](log-analytics-azure-vm-extension.md).

Vous pouvez surveiller les conteneurs Windows en cours d’exécution sur Service Fabric. Toutefois, seules les [machines virtuelles qui s’exécutent dans Azure](log-analytics-azure-vm-extension.md) et les [ordinateurs exécutant Windows dans votre environnement local](log-analytics-windows-agents.md) sont actuellement pris en charge pour Service Fabric.

Pour vérifier que la solution Conteneurs est correctement configurée :

- Assurez-vous que le pack d’administration a été correctement téléchargé, puis recherchez *ContainerManagement.xxx*.
    - Les fichiers doivent se trouver dans le dossier C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs.
- Vérifiez que l’ID de l’espace de travail est approprié, en accédant à **Panneau de configuration** > **Système et sécurité**.
    - Ouvrez **Microsoft Monitoring Agent**, et assurez-vous de l’exactitude des données de l’espace de travail.


## <a name="containers-data-collection-details"></a>Détails sur la collecte de données des conteneurs
La solution Conteneurs collecte diverses mesures de performances et données de journaux à partir des hôtes de conteneur et des conteneurs utilisant les agents que vous avez activés.

Le tableau suivant présente les méthodes de collecte de données et d’autres détails sur la manière dont les données sont collectées pour la solution Conteneurs.

| plateforme | [Agent OMS pour Linux](log-analytics-linux-agents.md) | Agent SCOM | Azure Storage | SCOM requis ? | Données de l’agent SCOM envoyées via un groupe d’administration | fréquence de collecte |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |![Oui](./media/log-analytics-containers/oms-bullet-green.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |Toutes les 3 minutes. |

| plateforme | [Agent Windows](log-analytics-windows-agents.md) | Agent SCOM | Azure Storage | SCOM requis ? | Données de l’agent SCOM envoyées via un groupe d’administration | fréquence de collecte |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |![Oui](./media/log-analytics-containers/oms-bullet-green.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |Toutes les 3 minutes. |

| plateforme | [Extension de machine virtuelle Log Analytics](log-analytics-azure-vm-extension.md) | Agent SCOM | Azure Storage | SCOM requis ? | Données de l’agent SCOM envoyées via un groupe d’administration | fréquence de collecte |
| --- | --- | --- | --- | --- | --- | --- |
| Microsoft Azure |![Oui](./media/log-analytics-containers/oms-bullet-green.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |![Non](./media/log-analytics-containers/oms-bullet-red.png) |Toutes les 3 minutes. |

Le tableau suivant présente des exemples de types de données collectées par la solution Conteneurs, et les types de données utilisés dans les recherches de journaux et les résultats :

| Type de données | Type de données dans Recherche de journaux | Champs |
| --- | --- | --- |
| Performances des hôtes et des conteneurs | `Type=Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem |
| Inventaire de conteneur | `Type=ContainerInventory` | TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContinerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Inventaire d’image de conteneur | `Type=ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Journal de conteneur | `Type=ContainerLog` | TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Journal de service de conteneur | `Type=ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Analyser les conteneurs
Une fois la solution activée dans le portail OMS, vous voyez la vignette **Conteneurs** affichant des informations récapitulatives sur vos hôtes de conteneur et les conteneurs s’exécutant dans les hôtes.

![Vignette Conteneurs](./media/log-analytics-containers/containers-title.png)

La vignette affiche une vue d’ensemble du nombre de conteneurs présents dans l’environnement, et indique s’ils sont en échec, en cours d’exécution ou arrêtés.

### <a name="using-the-containers-dashboard"></a>Utilisation du tableau de bord Conteneurs
Cliquez sur la vignette **Conteneurs**. À partir de là, vous voyez les vues organisées par :

* Événements de conteneur
* Erreurs
* État des conteneurs
* Inventaire d’image de conteneur
* Performances du processeur et de la mémoire

Chaque volet dans le tableau de bord est une représentation visuelle d’une recherche exécutée sur des données collectées.

![Tableau de bord Conteneurs](./media/log-analytics-containers/containers-dash01.png)

![Tableau de bord Conteneurs](./media/log-analytics-containers/containers-dash02.png)

Dans le panneau **État du conteneur**, cliquez pour la zone supérieure, comme illustré ci-dessous.

![État des conteneurs](./media/log-analytics-containers/containers-status.png)

La fenêtre Recherche de journal s’ouvre, affichant des informations sur les hôtes et les conteneurs en cours d’exécution en leur sein.

![Recherche de journal pour les conteneurs](./media/log-analytics-containers/containers-log-search.png)

À partir d’ici, vous pouvez modifier la requête de recherche de façon à trouver les informations spécifiques qui vous intéressent. Pour plus d’informations sur les recherches dans les journaux, voir [Recherches de journal dans Log Analytics](log-analytics-log-searches.md).

Par exemple, vous pouvez modifier la requête de recherche afin qu’elle affiche tous les conteneurs arrêtés au lieu des conteneurs en cours d’exécution, en changeant **Running** en **Stopped**.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Résoudre des problèmes en recherchant un conteneur en échec
OMS marque un conteneur comme étant en **Échec** s’il a été fermé avec un code de sortie autre que zéro. Vous pouvez consulter un aperçu des erreurs et des échecs de l’environnement dans le panneau **Conteneurs défectueux**.

### <a name="to-find-failed-containers"></a>Pour rechercher les conteneurs défectueux
1. Cliquez sur le panneau **Événements des conteneurs**.  
   ![Événements des conteneurs](./media/log-analytics-containers/containers-events.png)
2. La fenêtre Recherche de journal s’ouvre, affichant l’état des conteneurs, comme ci-dessous.  
   ![État des conteneurs](./media/log-analytics-containers/containers-container-state.png)
3. Ensuite, cliquez sur la valeur défectueuse pour afficher des informations supplémentaires telles que la taille d’image et le nombre d’images arrêtées et défectueuses. Développez **Afficher plus** pour afficher l’ID de l’image.  
   ![Conteneurs défectueux](./media/log-analytics-containers/containers-state-failed.png)
4. Ensuite, recherchez le conteneur qui exécute cette image. Tapez ce qui suit dans la requête de recherche.
   `Type=ContainerInventory <ImageID>` Cette commande affiche les journaux. Vous pouvez faire défiler pour voir le conteneur défectueux.  
   ![Conteneurs défectueux](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Rechercher des données de conteneur dans les journaux
Lorsque vous résolvez une erreur spécifique, il peut être utile de voir l’emplacement où elle se produit dans votre environnement. Les types de journaux suivants vous aident à créer des requêtes qui retournent les informations souhaitées.

* **ContainerInventory** : utilisez ce type de journal lorsque vous recherchez des informations sur l’emplacement des conteneurs, leurs noms et les images qu’ils exécutent.
* **ContainerImageInventory** : utilisez ce type de journal lorsque vous recherchez des informations organisées par image, et de consulter des informations sur les images, telles que leurs ID ou tailles.
* **ContainerLog** : utilisez ce type de journal lorsque vous recherchez des informations et entrées spécifiques du journal des erreurs.
* **ContainerServiceLog** : utilisez ce type de journal lorsque vous recherchez des informations de piste d’audit pour le démon Docker, telles que les commandes de démarrage, d’arrêter, de suppression ou d’extraction.

### <a name="to-search-logs-for-container-data"></a>Pour rechercher des données de conteneur dans les journaux
* Choisissez une image qui a échoué récemment et recherchez-la dans les journaux des erreurs. Commencez par rechercher un nom de conteneur exécutant cette image avec une recherche **ContainerInventory**. Par exemple, recherchez `Type=ContainerInventory ubuntu Failed`  
    ![Recherche de conteneurs Ubuntu](./media/log-analytics-containers/search-ubuntu.png)

  Notez le nom du conteneur en regard de **Name**, puis recherchez ces journaux. Dans cet exemple, il s’agit de `Type=ContainerLog adoring_meitner`.

**Afficher les informations de performances**

Lorsque vous commencez à créer des requêtes, il peut être utile de voir d’abord ce qui est possible. Par exemple, pour afficher toutes les données de performances, essayez d’utiliser une large requête en tapant la requête de recherche suivante.

```
Type=Perf
```

![Performances des conteneurs](./media/log-analytics-containers/containers-perf01.png)

Vous pouvez voir cela sous une forme plus graphique en cliquant sur le mot **Metrics** dans les résultats.

![Performances des conteneurs](./media/log-analytics-containers/containers-perf02.png)

Vous pouvez limiter les données de performances que vous voyez à un conteneur spécifique en tapant le nom de celui-ci à droite de votre requête.

```
Type=Perf <containerName>
```

Cela a pour effet d’afficher la liste des mesures de performances collectées pour un conteneur spécifique.

![Performances des conteneurs](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Exemples de requêtes de recherche de journal
Il est souvent utile de créer des requêtes en commençant par un exemple ou deux, puis en les modifiant afin de les adapter à votre environnement. Comme point de départ, vous pouvez utiliser le panneau **Requêtes notables** pour vous aider à créer des requêtes plus avancées.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Requêtes de conteneurs](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Enregistrement de requêtes de recherche de journal
L’enregistrement des requêtes est une fonctionnalité standard dans Log Analytics. En les enregistrant, vous aurez aisément accès à celles que vous avez trouvées utiles pour une utilisation ultérieure.

Après avoir créé une requête qui vous semble utile, enregistrez-la en cliquant sur **Favorites** en haut de la page Recherche de journal. Vous pourrez ainsi y accéder facilement par la suite à partir de la page **Mon tableau de bord**.

## <a name="next-steps"></a>Étapes suivantes
* [Rechercher dans les journaux](log-analytics-log-searches.md) pour consulter des enregistrements de données de conteneur détaillées.

