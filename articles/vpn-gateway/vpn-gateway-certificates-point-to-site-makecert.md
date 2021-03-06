---
title: "Générer et exporter des certificats pour les connexions de point à site : MakeCert : Azure | Microsoft Docs"
description: "Cet article décrit les étapes permettant de créer un certificat racine auto-signé, d’exporter la clé publique et de générer des certificats clients à l’aide de MakeCert."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 858ce2c748c3383afb6d66ed7b66a5f019902594
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Générer et exporter des certificats pour les connexions de point à site à l’aide de MakeCert

> [!NOTE]
> Suivez les instructions dans cet article pour générer des certificats uniquement lorsque vous n’avez pas accès à un ordinateur exécutant Windows 10. Dans le cas contraire, nous vous recommandons de suivre plutôt les instructions de l’article [Générer et exporter des certificats pour les connexions de point à site à l’aide de PowerShell](vpn-gateway-certificates-point-to-site.md).
>

Les connexions de point à site utilisent des certificats pour l’authentification. Cet article vous explique comment créer un certificat racine auto-signé et générer des certificats clients à l’aide de MakeCert. Si vous recherchez des étapes de la configuration de point à site, telles que le chargement des certificats racines, sélectionnez l’un des articles « Configuration d’une connexion point à site à un réseau virtuel » dans la liste suivante :

> [!div class="op_single_selector"]
> * [Création de certificat auto-signés - PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Création de certificat auto-signés - Makecert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Configuration d’une connexion de point à site à un réseau virtuel - Resource Manager - Portail Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Configuration d’une connexion point à site à un réseau virtuel - Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Configuration d’une connexion de point à site à un réseau virtuel - Classic - Portail Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Bien que nous recommandions de suivre les [étapes Windows 10 PowerShell](vpn-gateway-certificates-point-to-site.md) pour créer vos certificats, nous fournissons ces instructions MakeCert en tant que méthode facultative. Les certificats que vous générez à l’aide de ces deux méthodes peuvent être installés sur [tout système d’exploitation client pris en charge](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). Toutefois, MakeCert présente les limitations suivantes :

* MakeCert ne peut pas générer des certificats SHA-2, mais uniquement des certificats SHA-1. Les certificats SHA-1 restent valides pour les connexions de point à site, mais SHA-1 utilise un hachage de chiffrement moins puissant que celui de SHA-2.
* MakeCert est déconseillé. Cela signifie que cet outil pourrait être supprimé à tout moment. Les certificats que vous aurez déjà générés à l’aide de MakeCert ne seront pas affectés si MakeCert n’est plus disponible. MakeCert est utilisé uniquement pour générer les certificats, pas comme mécanisme de validation.

## <a name="rootcert"></a>Créer un certificat racine auto-signé

Les étapes suivantes vous montrent comment créer un certificat auto-signé à l’aide de MakeCert. Ces étapes ne sont pas spécifiques au modèle de déploiement. Elles sont valides pour le modèle Resource Manager et classique.

1. Téléchargez et installez [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. Après l’installation, vous pouvez généralement trouver l’utilitaire makecert.exe dans le chemin d’accès suivant : C:\Program Files (x86)\Windows Kits\10\bin\<arch>. Toutefois, il est possible qu’il ait été installé dans un autre emplacement. Ouvrez une invite de commandes avec des privilèges d’administrateur et accédez au dossier de l’utilitaire MakeCert. Vous pouvez utiliser l’exemple suivant, en l’ajustant pour l’emplacement approprié :

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Créez et installez un certificat dans le magasin de certificats personnels sur votre ordinateur. L’exemple suivant crée un fichier *.cer* correspondant que vous chargez sur Azure au moment de la configuration P2S. Remplacez 'P2SRootCert' et 'P2SRootCert.cer' par le nom que vous voulez attribuer au certificat. Le certificat se trouve dans votre dossier Certificates : Current User\Personal\Certificates.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha1 -len 2048 -ss My
  ```

## <a name="cer"></a>Exporter la clé publique (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Le fichier exported.cer doit être chargé dans Azure. Pour obtenir des instructions, consultez [Configurer une connexion de point à site](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). Pour ajouter un certificat racine approuvé, voir [cette section](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) de l’article.

### <a name="export-the-self-signed-certificate-and-private-key-to-store-it-optional"></a>Exporter le certificat auto-signé et la clé privée pour la stocker (facultatif)

Vous souhaiterez peut-être exporter le certificat racine auto-signé et le stocker en toute sécurité. Si besoin est, vous pouvez l’installer ultérieurement sur un autre ordinateur et générer davantage de certificats clients ou exporter un autre fichier .cer. Pour exporter le certificat racine auto-signé au format .pfx, sélectionnez le certificat racine et suivez les étapes décrites dans la section [Exporter un certificat client](#clientexport).

## <a name="create-and-install-client-certificates"></a>Créer et installer des certificats clients

Vous n’installez pas le certificat auto-signé directement sur l’ordinateur client. Vous devez générer un certificat client à partir du certificat auto-signé. Ensuite, vous exportez et installez le certificat client sur l’ordinateur client. Les étapes suivantes ne sont pas spécifiques au modèle de déploiement. Elles sont valides pour le modèle Resource Manager et classique.

### <a name="clientcert"></a>Générer un certificat client

Chaque ordinateur client qui se connecte à un réseau virtuel avec une connexion de point à site doit avoir un certificat client installé. Vous générez un certificat client à partir du certificat racine auto-signé, puis l’exportez et l’installez. Si le certificat client n’est pas installé, l’authentification échoue. 

Les étapes suivantes vous guident dans la génération d’un certificat client à partir d’un certificat racine auto-signé. Vous pouvez générer plusieurs certificats clients à partir d’un même certificat racine. Lorsque vous générez des certificats clients suivant les étapes ci-dessous, le certificat client est automatiquement installé sur l’ordinateur que vous avez utilisé pour générer le certificat. Si vous souhaitez installer un certificat client sur un autre ordinateur client, vous pouvez exporter le certificat.
 
1. Sur l’ordinateur que vous avez utilisé pour créer le certificat auto-signé, ouvrez une invite de commandes comme administrateur.
2. Modifiez et exécutez l’exemple pour générer un certificat client.
  * Remplacez *"P2SRootCert"* par le nom du certificat racine auto-signé à partir duquel vous générez le certificat client. Veillez à utiliser le nom du certificat racine, c'est-à-dire la valeur « CN » que vous avez spécifiée lorsque vous avez créé le certificat racine auto-signé.
  * Remplacez *P2SChildCert* par le nom que vous souhaitez pour générer un certificat client.

  Si vous exécutez l’exemple suivant en l’état, cette opération entraîne la création d’un certificat client nommé P2SChildcert dans votre magasin de certificats personnel généré à partir du certificat racine P2SChildcert.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha1
  ```

### <a name="clientexport"></a>Exporter un certificat client

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Installer un certificat client exporté

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Étapes suivantes

Poursuivez votre configuration point à site. 

* Pour connaître les étapes du modèle de déploiement **Resource Manager**, consultez la page [Configurer une connexion de point à site à un réseau virtuel](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Pour connaître les étapes du modèle de déploiement **Classic**, consultez la page [Configurer une connexion VPN de point à site à un réseau virtuel (Classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).

