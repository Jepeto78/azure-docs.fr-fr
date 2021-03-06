---
title: "Azure AD Connect : Authentification unique transparente - Démarrage rapide | Microsoft Docs"
description: "Cet article explique comment bien démarrer avec l’authentification unique transparente d’Azure Active Directory."
services: active-directory
keywords: "Qu’est-ce qu’Azure AD Connect, Installation d’Active Directory, Composants requis pour Azure AD, SSO, Authentification unique"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 05fb966e3e18b8d5242a2795248b9b72352d894d
ms.contentlocale: fr-fr
ms.lasthandoff: 07/25/2017

---

# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Authentification unique transparente Azure Active Directory - Démarrage rapide

L’authentification unique transparente Azure Active Directory connecte automatiquement les utilisateurs lorsque leurs ordinateurs d’entreprise sont connectés au réseau de l’entreprise. Elle offre à vos utilisateurs un accès facilité à vos applications dans le cloud sans nécessiter de composants locaux supplémentaires.

## <a name="how-to-deploy-azure-ad-seamless-sso"></a>Comment déployer l’authentification unique transparente Azure AD

Pour déployer l’authentification unique transparente, vous devez procéder comme suit :
1. *Vérifier les prérequis* : configurez convenablement votre environnement local et votre locataire avant d’activer la fonctionnalité.
2. *Activer la fonctionnalité* : activez l’authentification unique transparente sur votre locataire au moyen d’Azure AD Connect.
3. *Déployer la fonctionnalité*: utilisez la stratégie de groupe afin d’étendre la fonctionnalité à une partie ou à la totalité de vos utilisateurs.
4. *Tester la fonctionnalité*: testez l’authentification de l’utilisateur à l’aide de l’authentification unique transparente.
5. *{0}{0}{0}{0}{0}{0}{0}{0}{0} clés* : substituer fréquemment les clés de déchiffrement de comptes d’ordinateur Kerberos.

## <a name="step-1-check-prerequisites"></a>Étape 1 : Vérifier les prérequis

Vérifiez que les conditions préalables suivantes sont remplies :

1. Configurez votre serveur Azure AD Connect : si vous utilisez l’[authentification directe](active-directory-aadconnect-pass-through-authentication.md) comme méthode de connexion, aucune action supplémentaire n’est nécessaire. Si vous utilisez la [synchronisation de hachage de mot de passe](active-directory-aadconnectsync-implement-password-synchronization.md) comme méthode de connexion, et s’il existe un pare-feu entre Azure AD Connect et Azure AD, vérifiez les points suivants :
- Vous utilisez la version 1.1.484.0 ou une version ultérieure d’Azure AD Connect.
- Azure AD Connect peut communiquer avec les URL `*.msappproxy.net` et sur le port 443. Cette condition préalable est applicable uniquement lorsque vous activez cette fonctionnalité, et pas pour les connexions d’utilisateur réelles.
- Azure AD Connect peut aussi établir des connexions IP directes vers les [plages d’adresses IP du centre de données Azure](https://www.microsoft.com/download/details.aspx?id=41653). Là encore, cette condition préalable est applicable uniquement lorsque vous activez la fonctionnalité.
2. Vous devez fournir les informations d’identification de l’administrateur de domaine pour chaque forêt AD que vous synchronisez avec Azure AD (au moyen d’Azure AD Connect) et pour les utilisateurs dont vous souhaitez activer l’authentification unique transparente.

## <a name="step-2-enable-the-feature"></a>Étape 2 : Activer la fonctionnalité

L’authentification unique transparente peut être activée à l’aide d’[Azure AD Connect](active-directory-aadconnect.md).

Si vous procédez à une nouvelle installation d’Azure AD Connect, choisissez le [chemin d’installation personnalisé](active-directory-aadconnect-get-started-custom.md). Sur la page Connexion utilisateur, cochez la case Activer l’authentification unique.

![Azure AD Connect - Connexion utilisateur](./media/active-directory-aadconnect-sso/sso8.png)

Si vous disposez déjà d’une installation Azure AD Connect, choisissez Modifier la connexion utilisateur et cliquez sur Suivant. Cochez ensuite la case Activer l’authentification unique.

![Azure AD Connect - Modifier la connexion utilisateur](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Suivez les instructions de l’Assistant jusqu’à ce que vous accédiez à la page Activer l’authentification unique. Fournissez les informations d’identification de l’administrateur de domaine pour chaque forêt AD que vous synchronisez avec Azure AD (au moyen d’Azure AD Connect) et pour les utilisateurs dont vous souhaitez activer l’authentification unique transparente. 

À la fin de l’Assistant, l’authentification unique transparente est activée sur votre locataire.

>[!NOTE]
> Les informations d’identification d’administrateur de domaine ne sont pas stockées dans Azure AD Connect ni dans Azure AD, elles sont uniquement utilisées pour activer la fonctionnalité.

## <a name="step-3-roll-out-the-feature"></a>Étape 3 : Déployer la fonctionnalité

Pour étendre cette fonctionnalité à vos utilisateurs, vous devez ajouter deux URL Azure AD (https://autologon.microsoftazuread-sso.com et https://aadg.windows.net.nsatc.net) aux paramètres de la zone Intranet des utilisateurs via la stratégie de groupe dans Active Directory.

>[!NOTE]
> Les instructions suivantes ne valent que pour les navigateurs Internet Explorer et Google Chrome sur Windows (si ce dernier partage l’ensemble des URL de sites de confiance avec Internet Explorer). Lisez la section suivante pour savoir comment configurer Mozilla Firefox et Google Chrome sur Mac.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Pourquoi devez-vous modifier les paramètres de la zone Intranet des utilisateurs ?

Par défaut, le navigateur calcule automatiquement la zone appropriée (Internet ou Intranet) à partir d’une URL. Par exemple, http://contoso/ est mappée à la zone Intranet, tandis que http://intranet.contoso.com/ est mappée à la zone Internet (car l’URL contient un point). Les navigateurs n’envoient pas de ticket Kerberos à un point de terminaison cloud, comme les deux URL Azure AD, sauf si son URL est ajoutée explicitement à la zone Intranet du navigateur.

### <a name="detailed-steps"></a>Procédure détaillée

1. Ouvrez l’outil de gestion de stratégie de groupe.
2. Modifiez la stratégie de groupe qui est appliquée à certains ou à l’ensemble de vos utilisateurs. Dans cet exemple, nous utilisons la **stratégie de domaine par défaut**.
3. Accédez à **Configuration utilisateur\Modèles d’administration\Composants Windows\Internet Explorer\Panneau de configuration Internet\Page de sécurité** et sélectionnez **Liste des attributions de sites aux zones**.
![Authentification unique](./media/active-directory-aadconnect-sso/sso6.png)  
4. Activez la stratégie, puis entrez les valeurs (les URL Azure AD où les tickets Kerberos sont transférés) et les données suivantes (*1* indique la zone Intranet) dans la boîte de dialogue.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Si vous souhaitez interdire à certains utilisateurs l’utilisation de l’authentification unique transparente - par exemple, si ces utilisateurs se connectent sur des bornes partagées - définissez les valeurs précédentes sur *4*. Cette action ajoute les URL Azure AD aux Sites sensibles et rend l’authentification unique transparente inutilisable en permanence.

5. Cliquez sur **OK**, puis de nouveau sur **OK**.

![Authentification unique](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a>Considérations sur le navigateur

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox ne procède pas automatiquement à l’authentification Kerberos. Chaque utilisateur doit ajouter manuellement les URL Azure AD à ses paramètres Firefox en procédant comme suit :
1. Exécutez Firefox et entrez `about:config` dans la barre d’adresses. Ignorez les notifications que vous voyez.
2. Recherchez la préférence **network.negotiate-auth.trusted-URI**. Cette préférence répertorie les sites de confiance de Firefox pour l’authentification Kerberos.
3. Cliquez avec le bouton droit et sélectionnez Modifier.
4. Entrez https://autologon.microsoftazuread-sso.com et https://aadg.windows.net.nsatc.net dans le champ.
5. Cliquez sur OK, puis rouvrez le navigateur.

#### <a name="safari-on-mac-os"></a>Safari sur Mac OS

Assurez-vous que l’ordinateur utilisant Mac OS est connecté à AD. Retrouvez des instructions sur la procédure [ici](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Google Chrome sur Mac

Pour ce qui est de Google Chrome sur Mac OS et sur les autres plateformes autres que Windows, reportez-vous à [cet article](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) pour obtenir plus d’informations sur l’ajout des URL Azure AD à la liste approuvée pour l’authentification intégrée.

L’utilisation des extensions de stratégie de groupe Active Directory tierces permettant de déployer les URL Azure AD pour les utilisateurs de Firefox et de Google Chrome sur Mac dépasse la portée de cet article.

#### <a name="known-limitations"></a>Limites connues

L’authentification unique transparente ne fonctionne pas en mode de navigation privée sur Firefox et Edge. Par ailleurs, il ne fonctionne pas sur Internet Explorer si le navigateur en cours d’utilisation est en mode Protection améliorée.

## <a name="step-4-test-the-feature"></a>Étape 4 : Tester la fonctionnalité

Pour tester la fonctionnalité d’un utilisateur spécifique, assurez-vous que _toutes_ les conditions suivantes sont en place :
  - L’utilisateur se connecte à un appareil d’entreprise.
  - L’appareil a déjà été joint à votre domaine Active Directory (AD).
  - L’appareil dispose d’une connexion directe à votre contrôleur de domaine, soit sur le réseau câblé ou sans fil de l’entreprise, soit par le biais d’une connexion d’accès à distance, comme une connexion VPN.
  - Vous avez [déployé la fonctionnalité](##step-3-roll-out-the-feature) pour cet utilisateur à l’aide de la stratégie de groupe.

Pour tester le scénario dans lequel l’utilisateur entre uniquement le nom d’utilisateur, mais pas le mot de passe :
- Connectez-vous à *https://myapps.microsoft.com/* dans une nouvelle session de navigation privée.

Pour tester le scénario dans lequel l’utilisateur n’a pas à entrer le nom d’utilisateur ou le mot de passe : 
- Connectez-vous à *https://myapps.microsoft.com/contoso.onmicrosoft.com* dans une nouvelle session de navigation privée. Remplacez « *contoso* » par le nom de votre locataire.
- Ou connectez-vous à *https://myapps.microsoft.com/contoso.com* dans une nouvelle session de navigation privée. Remplacez « *contoso.com* » par un domaine vérifié (pas un domaine fédéré) dans votre locataire.

## <a name="step-5-roll-over-keys"></a>Étape 5 : substituer les clés

À l’étape 2, Azure AD Connect crée des comptes d’ordinateurs (représentant Azure AD) dans toutes les forêts AD sur lesquelles vous avez activé l’authentification unique transparente. Obtenez plus de détails [ici](active-directory-aadconnect-sso-how-it-works.md). Pour améliorer la sécurité, il vous est recommandé de fréquemment substituer les clés de déchiffrement Kerberos de ces comptes d’ordinateur.

>[!IMPORTANT]
>Vous n’avez pas besoin d’effectuer cette étape _immédiatement_ après avoir activé la fonctionnalité. Substituez les clés de déchiffrement Kerberos au moins tous les 30 jours.

## <a name="next-steps"></a>Étapes suivantes

- [**Immersion technique**](active-directory-aadconnect-sso-how-it-works.md) : découvrez comment fonctionne cette fonctionnalité.
- [**Forum aux questions**](active-directory-aadconnect-sso-faq.md) : réponses aux questions fréquentes.
- [**Résolution des problèmes**](active-directory-aadconnect-troubleshoot-sso.md) : découvrez comment résoudre les problèmes courants susceptibles de survenir avec cette fonctionnalité.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) : pour le dépôt de nouvelles demandes de fonctionnalités.

