---
title: Forum aux questions sur les machines virtuelles Linux dans Azure | Microsoft Docs
description: "Fournit des réponses à certaines questions courantes sur les machines virtuelles Linux créées avec le modèle de déploiement Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 5a0092481cb461f26ba463f4c9bbaf114ecb1248
ms.contentlocale: fr-fr
ms.lasthandoff: 05/11/2017


---
# Forum aux questions sur les machines virtuelles Linux
<a id="frequently-asked-question-about-linux-virtual-machines" class="xliff"></a>
Cet article traite certaines questions courantes concernant les machines virtuelles Linux créées dans Azure avec le modèle de déploiement Resource Manager. Pour la version Windows de cette rubrique, consultez les [Questions fréquences sur les machines virtuelles Windows](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## Qu’est-il possible d’exécuter sur une machine virtuelle Azure ?
<a id="what-can-i-run-on-an-azure-vm" class="xliff"></a>
Tous les abonnés peuvent exécuter des logiciels serveur sur une machine virtuelle Azure. Pour plus d’informations, consultez [Linux dans des distributions prises en charge par Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## Quelle quantité de stockage puis-je utiliser avec une machine virtuelle ?
<a id="how-much-storage-can-i-use-with-a-virtual-machine" class="xliff"></a>
Chaque disque de données peut avoir une capacité allant jusqu’à 1 To Le nombre de disques de données que vous pouvez utiliser dépend de la taille de la machine virtuelle. Pour en savoir plus, voir la rubrique [Tailles de machines virtuelles](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Un compte de stockage Azure fournit le stockage pour le disque du système d’exploitation et tout disque de données. Chaque disque est un fichier .vhd stocké sous la forme d’un objet blob de pages. Pour plus d’informations sur la tarification, voir [Tarification – Stockage](https://azure.microsoft.com/pricing/details/storage/).

## Comment puis-je accéder à ma machine virtuelle ?
<a id="how-can-i-access-my-virtual-machine" class="xliff"></a>
Établissez une connexion à distance pour vous connecter à la machine virtuelle à l’aide de SSH (Secure Shell). Consultez les instructions sur la connexion [à partir de Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ou [à partir de Linux et Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Par défaut, SSH autorise un maximum de 10 connexions simultanées. Vous pouvez augmenter ce nombre en modifiant le fichier de configuration.

Si vous rencontrez des problèmes, consultez [Dépanner les connexions Secure Shell (SSH)](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## Est-ce que je peux utiliser le disque temporaire (/dev/sdb1) pour stocker les données ?
<a id="can-i-use-the-temporary-disk-devsdb1-to-store-data" class="xliff"></a>
N’utilisez pas le disque temporaire (/dev/sdb1) pour stocker des données. Il s’agit uniquement d’un stockage temporaire. Vous risquez de perdre des données qui ne pourront pas être récupérées.

## Puis-je copier ou cloner une machine virtuelle Azure ?
<a id="can-i-copy-or-clone-an-existing-azure-vm" class="xliff"></a>
Oui. Pour obtenir des instructions, consultez [Création d’une copie d’une machine virtuelle Linux dans le modèle de déploiement Resource Manager](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## Pourquoi ne vois-je pas les régions Centre et Est du Canada dans Azure Resource Manager ?
<a id="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager" class="xliff"></a>
Les deux régions Centre et Est du Canada ne sont pas enregistrées automatiquement lors de la création de machines virtuelles pour des abonnements Azure existants. Cet enregistrement s’effectue automatiquement lorsqu’une machine virtuelle est déployée par le biais du portail Azure dans n’importe quelle autre région à l’aide d’Azure Resource Manager. Une fois une machine virtuelle déployée dans toute autre région Azure, les nouvelles régions doivent être disponibles pour les machines virtuelles suivantes.

## Puis-je ajouter une carte réseau à ma machine virtuelle après sa création ?
<a id="can-i-add-a-nic-to-my-vm-after-its-created" class="xliff"></a>
Oui, c’est maintenant possible. La machine virtuelle doit d’abord être arrêtée et libérée. Ensuite, vous pouvez ajouter ou supprimer une carte réseau (sauf si elle est la dernière carte réseau sur la machine virtuelle). 

## Existe-t-il des exigences en matière de nom d’ordinateur ?
<a id="are-there-any-computer-name-requirements" class="xliff"></a>
Oui. Le nom d’ordinateur peut avoir une longueur maximale de 64 caractères. Consultez la rubrique [Instructions de dénomination d’infrastructure](infrastructure-naming-guidelines.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pour plus d’informations sur la dénomination de ressources.

## Existe-t-il des exigences pour le nom du groupe de ressources ?
<a id="are-there-any-resource-group-name-requirements" class="xliff"></a>
Oui. Le nom du groupe de ressources peut avoir une longueur maximale de 90 caractères. Consultez [Instructions pour les groupes de ressources d’infrastructure](infrastructure-resource-groups-guidelines.md) pour plus d’informations sur les groupes de ressources.

## Quelles sont les exigences en matière de nom d’utilisateur lors de la création d’une machine virtuelle ?
<a id="what-are-the-username-requirements-when-creating-a-vm" class="xliff"></a>
Les noms d’utilisateur doivent avoir une longueur de 1 à 64 caractères.

Les noms d’utilisateur suivants ne sont pas autorisés :

<table>
    <tr>
        <td style="text-align:center">administrator </td><td style="text-align:center"> admin </td><td style="text-align:center"> user </td><td style="text-align:center"> user1</td>
    </tr>
    <tr>
        <td style="text-align:center">test </td><td style="text-align:center"> user2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> user3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> a</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> aspnet</td>
    </tr>
    <tr>
        <td style="text-align:center">backup </td><td style="text-align:center"> console </td><td style="text-align:center"> david </td><td style="text-align:center"> guest</td>
    </tr>
    <tr>
        <td style="text-align:center">john </td><td style="text-align:center"> propriétaire </td><td style="text-align:center"> root </td><td style="text-align:center"> server</td>
    </tr>
    <tr>
        <td style="text-align:center">sql </td><td style="text-align:center"> support </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">test2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> user4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## Quelles sont les exigences en matière de mot de passe lors de la création d’une machine virtuelle ?
<a id="what-are-the-password-requirements-when-creating-a-vm" class="xliff"></a>
Les mots de passe doivent comporter de 6 à 72 caractères et répondre à 3 des 4 exigences de complexité suivantes :

* Avoir des minuscules
* Avoir des majuscules
* Avoir un chiffre
* Avoir un caractère spécial (correspondances Regex [\W_])

Les noms mots de passe suivants ne sont pas autorisés :

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$$word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Password!</td>
        <td style="text-align:center">Password1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">iloveyou!</td>
    </tr>
</table>

