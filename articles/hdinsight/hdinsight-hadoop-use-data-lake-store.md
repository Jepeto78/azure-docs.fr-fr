---
title: Utilisez Data Lake Store avec Hadoop dans HDInsight Azure | Microsoft Docs
description: "Apprenez à interroger des données depuis Azure Data Lake Store et à stocker les résultats de votre analyse."
keywords: "stockage d’objets blob, hdfs, données structurées, données non structurées, data lake store"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 993cff63037017e37ff5b0787f50ba002df28d03
ms.contentlocale: fr-fr
ms.lasthandoff: 07/04/2017


---
<a id="use-data-lake-store-with-azure-hdinsight-clusters" class="xliff"></a>

# Utiliser Data Lake Store avec des clusters Azure HDInsight

Pour analyser des données dans un cluster HDInsight, vous pouvez stocker les données dans le [stockage Azure](../storage/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) ou les deux. Les deux options de stockage vous permettent de supprimer des clusters HDInsight servant aux calculs, sans perte de données utilisateur.

Dans cet article, vous découvrez le fonctionnement de Data Lake Store avec des clusters HDInsight. Pour savoir comment le stockage Azure fonctionne avec les clusters HDInsight, consultez [Use Azure Storage with Azure HDInsight clusters](hdinsight-hadoop-use-blob-storage.md) (Utiliser le stockage Azure. avec des clusters HDInsight). Consultez la rubrique [Créer des clusters Hadoop dans HDInsight](hdinsight-hadoop-provision-linux-clusters.md) pour des informations sur la création d’un cluster HDInsight.

> [!NOTE]
> Data Lake Store est toujours accessible par le biais d’un canal sécurisé, donc il y a aucun nom de schéma de système de fichiers `adls`. Vous utilisez toujours `adl`.
> 
> 

<a id="availabilities-for-hdinsight-clusters" class="xliff"></a>

## Disponibilités pour les clusters HDInsight

Hadoop prend en charge une notion de système de fichiers par défaut. Le système de fichiers par défaut implique un schéma et une autorité par défaut. Il peut également être utilisé pour résoudre les chemins d'accès relatifs. Pendant le processus de création du cluster HDInsight, vous pouvez spécifier un conteneur d’objets blob dans le stockage Azure comme système de fichiers par défaut, ou, avec HDInsight 3.5 et des versions plus récentes, vous pouvez sélectionner le stockage Azure ou Azure Data Lake Store en tant que système de fichiers par défaut avec quelques exceptions. 

Les clusters HDInsight peuvent utiliser Data Lake Store de deux manières :

* Comme stockage par défaut
* Comme stockage supplémentaire, avec Azure Storage Blob comme stockage par défaut.

À partir de maintenant, seules certain(e)s versions/types de cluster HDInsight prennent en charge l’utilisation de stockage de Data Lake Store comme stockage par défaut et comptes de stockage supplémentaires :

| Type de cluster HDInsight | Data Lake Store comme stockage par défaut | Data Lake Store comme stockage supplémentaire| Remarques |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight version 3.6 | Oui | Oui | |
| HDInsight version 3.5 | Oui | Oui | À l’exception de HBase|
| HDInsight version 3.4 | Non | Oui | |
| HDInsight version 3.3 | Non | Non | |
| HDInsight version 3.2 | Non | Oui | |
| HDInsight Premium (couche)| Non | Non | |
| Storm | | |Vous pouvez utiliser Data Lake Store pour écrire des données à partir d’une topologie Storm. Vous pouvez également vous en servir pour stocker des données de référence qui peuvent ensuite être lues par une topologie Storm.|

L’utilisation de Data Lake Store en tant que compte de stockage supplémentaire n’affecte pas les performances ni la capacité de lecture ou d’écriture sur le stockage Azure à partir du cluster.


<a id="use-data-lake-store-as-default-storage" class="xliff"></a>

## Utiliser Data Lake Store comme stockage par défaut

Lorsque HDInsight est déployé avec Data Lake Store en tant que stockage par défaut, les fichiers associés au cluster sont stockés dans Data Lake Store à l’emplacement suivant :

    adl://mydatalakestore/<cluster_root_path>/

où `<cluster_root_path>` est le nom d’un dossier que vous créez dans Data Lake Store. En spécifiant un chemin d’accès racine pour chaque cluster, vous pouvez utiliser le même compte Data Lake Store pour plusieurs clusters. Par conséquent, vous pouvez avoir une configuration dans laquelle :

* Cluster1 peut utiliser le chemin d’accès `adl://mydatalakestore/cluster1storage`
* Cluster2 peut utiliser le chemin d’accès `adl://mydatalakestore/cluster2storage`

Notez que les deux clusters utilisent le même compte Data Lake Store **mydatalakestore**. Chaque cluster a accès à son propre système de fichiers racine dans Data Lake Store. L’expérience de déploiement avec le portail Azure en particulier vous invite à utiliser un nom de dossier comme **/clusters/\<nom de cluster>** pour le chemin d’accès racine.

Pour être en mesure d’utiliser Data Lake Store en tant que stockage par défaut, vous devez accorder l’accès du principal de service aux chemins d’accès suivants :

- Racine du compte Data Lake Store.  Par exemple : adl://mydatalakestore/.
- Dossier pour tous les dossiers de cluster.  Par exemple : adl://mydatalakestore/clusters.
- Dossier pour le cluster.  Par exemple : adl://mydatalakestore/clusters/cluster1storage.

Pour plus d’informations sur la création du principal de service et l’octroi de l’accès, consultez [Configure Data Lake store access](#configure-data-lake-store-access) (Configurer l’accès à Data Lake Store).


<a id="use-data-lake-store-as-additional-storage" class="xliff"></a>

## Utiliser Data Lake Store comme stockage supplémentaire

Vous pouvez également utiliser Data Lake Store en tant que stockage supplémentaire pour le cluster. Dans ce cas, le stockage de cluster par défaut peut être un compte Azure Storage Blob ou Data Lake Store. Si vous exécutez des tâches HDInsight sur des données stockées dans Data Lake Store en tant que stockage supplémentaire, vous devez utiliser le chemin d’accès complet aux fichiers. Par exemple :

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Notez qu’il n’y a encore aucun **cluster_root_path** dans l’URL. La raison est que Data Lake Store n’est pas un stockage par défaut dans ce cas et qu’il vous suffit donc de fournir le chemin d’accès aux fichiers.

Pour être en mesure d’utiliser Data Lake Store comme stockage supplémentaire, il suffit d’accorder l’accès du principal de service aux chemins d’accès où sont stockés vos fichiers.  Par exemple :

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Pour plus d’informations sur la création du principal de service et l’octroi de l’accès, consultez [Configurer l’accès à Data Lake Store](#configure-data-lake-store-access).


<a id="use-more-than-one-data-lake-store-accounts" class="xliff"></a>

## Utiliser plusieurs comptes Data Lake Store

L’ajout d’un compte Data Lake Store comme compte supplémentaire et de plusieurs comptes Data Lake Store s’effectue en donnant au cluster HDInsight l’autorisation d’accéder aux données de plusieurs comptes Data Lake Store. Consultez [Configurer l’accès à Data Lake Store](#configure-data-lake-store-access).

<a id="configure-data-lake-store-access" class="xliff"></a>

## Configurer l’accès aux données Data Lake Store

Pour configurer l’accès à Data Lake store à partir de votre cluster HDInsight, vous devez disposer d’un principal de service Azure Active directory (Azure AD). Vous pouvez créer un principal de service uniquement si vous être administrateur Azure AD. Le principal de service doit être créé avec un certificat. Pour plus d’informations, consultez [Configurer l’accès à Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access), et [Create service principal with self-signed-certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate) (Créer un principal du service avec un certificat auto-signé).

> [!NOTE]
> Si vous vous apprêtez à utiliser Azure Data Lake Store en tant que stockage supplémentaire pour le cluster HDInsight, nous vous recommandons vivement de procéder ainsi lorsque vous créez le cluster comme décrit dans cet article. L’ajout d’Azure Data Lake Store en tant que stockage supplémentaire à un cluster HDInsight existant est un processus complexe sujet aux erreurs.
>

<a id="access-files-from-the-cluster" class="xliff"></a>

## Accès aux fichiers à partir du cluster

Il existe plusieurs méthodes pour accéder aux fichiers dans Data Lake Store à partir d’un cluster HDInsight.

* **Utilisation du nom complet**. Avec cette approche, vous fournissez le chemin d’accès complet au fichier auquel vous souhaitez accéder.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Utilisation du format de chemin d’accès raccourci**. Avec cette approche, vous remplacez le chemin d’accès à la racine du cluster par adl:///. Donc dans l’exemple ci-dessus, vous pouvez remplacer `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` par `adl:///`.

        adl:///<file path>

* **Utilisation du chemin d’accès relatif**. Avec cette approche, vous fournissez uniquement le chemin d’accès relatif au fichier auquel vous souhaitez accéder. Par exemple, si le chemin d’accès complet au fichier est :

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Vous pouvez accéder au même exemple de fichier .log à l’aide de ce chemin d’accès relatif à la place.

        /example/data/sample.log

<a id="create-hdinsight-clusters-with-access-to-data-lake-store" class="xliff"></a>

## Créer des clusters HDInsight ayant accès à Data Lake Store

Suivez les liens ci-dessous pour obtenir des instructions détaillées sur la manière de créer des clusters HDInsight avec accès au Data Lake Store.

* [Utilisation du portail](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [Utilisation de PowerShell (avec Data Lake Store en tant que stockage par défaut)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Utilisation de PowerShell (avec Data Lake Store en tant que stockage supplémentaire)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Utilisation de modèles Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


<a id="next-steps" class="xliff"></a>

## Étapes suivantes
Dans cet article, vous avez appris à utiliser Azure Data Lake Store compatible HDFS avec HDInsight. Ceci vous permet de créer des solutions à long terme et évolutives d’acquisition et d’archivage de données et d’utiliser HDInsight pour déverrouiller les informations des données structurées et non structurées stockées.

Pour plus d'informations, consultez les pages suivantes :

* [Prise en main d’Azure HDInsight][hdinsight-get-started]
* [Prise en main d’Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md)
* [Téléchargement de données vers HDInsight][hdinsight-upload-data]
* [Utilisation de Hive avec HDInsight][hdinsight-use-hive]
* [Utilisation de Pig avec HDInsight][hdinsight-use-pig]
* [Utilisation des signatures d’accès partagé Azure Storage pour restreindre l’accès aux données avec HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  

