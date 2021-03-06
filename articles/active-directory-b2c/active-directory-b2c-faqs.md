---
title: 'Azure Active Directory B2C : FAQ | Microsoft Docs'
description: Forum aux questions sur Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: saeeda
manager: krassk
editor: bryanla
ms.assetid: ed33c2ca-76d0-442a-abb1-8b7b7bb92d6a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: saeeda
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: f06e02cb15532a4e0f6660ca3a88a7031d498632
ms.contentlocale: fr-fr
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C : FAQ
Cette page répond aux questions fréquemment posées sur Azure Active Directory (Azure AD) B2C. N'hésitez pas à la consulter pour vous tenir au courant des mises à jour.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Puis-je utiliser les fonctionnalités d’Azure AD B2C dans mon client Azure AD existant, basé sur les employés ?
Azure AD et Azure AD B2C sont deux offres de produits distinctes qui ne peuvent pas coexister dans le même locataire.  Un locataire Azure AD représente une organisation.  Un locataire Azure AD B2C représente une collection d’identités à utiliser avec des applications par partie de confiance.  Avec les stratégies personnalisées (en préversion publique), Azure AD B2C peut fédérer avec Azure AD, ce qui permet l’authentification des employés dans une organisation.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Puis-je utiliser Azure AD B2C pour activer la connexion à des réseaux sociaux (Facebook et Google+) dans Office 365 ?
Vous ne pouvez pas utiliser Azure AD B2C pour authentifier les utilisateurs pour Microsoft Office 365.  Azure AD est la solution Microsoft permettant de gérer l’accès des employés aux applications SaaS. Elle présente des fonctionnalités conçues à cet effet, telles que l’accès conditionnel et la gestion des licences.  Azure AD B2C fournit une plateforme de gestion des identités et des accès pour la création d’applications web et mobiles.  Quand Azure AD B2C est configuré pour établir la fédération avec un locataire Azure AD, le locataire Azure AD gère l’accès des employés aux applications qui s’appuient sur Azure AD B2C.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Que sont les comptes locaux dans Azure AD B2C ? En quoi sont-ils différents des comptes professionnels ou scolaires dans Azure AD ?
Dans un locataire Azure AD, les utilisateurs qui appartiennent au locataire se connectent avec une adresse e-mail au format `<xyz>@<tenant domain>`.  Le `<tenant domain>` est l’un des domaines vérifiés dans le locataire ou le domaine `<...>.onmicrosoft.com` initial. Ce type de compte est un compte professionnel ou scolaire.

Dans un locataire Azure AD B2C, la plupart des applications demandent que l’utilisateur se connecte avec une adresse e-mail arbitraire (par exemple, joe@comcast.net, bob@gmail.com, sarah@contoso.com ou jim@live.com). Ce type de compte est un compte local.  Nous prenons également en charge les noms d’utilisateur arbitraires en tant que comptes locaux (par exemple joe, bob, sarah ou jim). Vous pouvez choisir l’un de ces deux types de comptes locaux en configurant Azure AD B2C dans le portail Azure.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Quels fournisseurs d’identité sociaux prenez-vous en charge maintenant ? Lesquels envisagez-vous de prendre en charge à l'avenir ?
Nous prenons actuellement en charge Facebook, Google +, LinkedIn, Compte Microsoft, Amazon, Twitter (préversion), WeChat (préversion), Weibo (préversion) et QQ (préversion).  Nous continuerons à ajouter la prise en charge d’autres fournisseurs d’identité sociaux populaires en fonction de la demande des clients.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Puis-je configurer des étendues pour collecter plus d’informations sur les clients depuis différents fournisseurs d’identité sociaux ?
Non, mais cette fonctionnalité est sur notre feuille de route. Les étendues par défaut utilisées pour notre jeu de fournisseurs d'identité sociaux pris en charge sont :

* Facebook : e-mail
* Google+ : e-mail
* Compte Microsoft : profil de messagerie openid
* Amazon : profil
* LinkedIn : r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Mon application doit-elle être exécutée sous Azure pour fonctionner avec Azure AD B2C ?
Non, vous pouvez héberger votre application n'importe où (dans le cloud ou sur site). Pour interagir avec Azure AD B2C, il suffit qu’elle puisse envoyer et recevoir des requêtes HTTP sur les points de terminaison accessibles publiquement.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Je dispose de plusieurs locataires Azure AD B2C. Comment puis-je les gérer sur le portail Azure ?
Chaque locataire Azure AD B2C a son propre panneau de fonctionnalités B2C sur le portail Azure. Consultez [Azure Active Directory B2C : inscription de votre application](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) pour savoir comment accéder au panneau de fonctionnalités B2C d’un client spécifique sur le portail Azure. Le basculement d’un annuaire Azure AD B2C à un autre sur le portail Azure ne garde pas le panneau de fonctionnalités B2C ouvert dans la plupart des navigateurs.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Comment puis-je personnaliser les e-mails de vérification (le contenu et le champ « De: ») envoyés par Azure AD B2C ?
Vous pouvez utiliser la [fonctionnalité de personnalisation de la société](../active-directory/active-directory-add-company-branding.md) pour personnaliser le contenu des e-mails de vérification. Plus précisément, ces deux éléments du message peuvent être personnalisés :

* **Logo de bannière**: affiché en bas à droite.
* **Couleur d’arrière-plan**: affiché en haut.

    ![Capture d’écran d’un e-mail de vérification personnalisée](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

La signature d’e-mail contient le nom du client B2C que vous avez fourni lors de la création du client B2C. Vous pouvez modifier le nom à l’aide de ces instructions :

1. Connectez-vous au [portail Azure Classic](https://manage.windowsazure.com/) en tant qu’administrateur d’abonnements.
1. Accédez à votre client B2C.
1. Cliquez sur l'onglet **Configurer** .
1. Modifiez le champ **Nom** dans la section **Propriétés du répertoire**.
1. Cliquez sur **Enregistrer** au bas de la page.

Il n’existe actuellement aucun moyen de modifier le champ « De : » de l’e-mail. Votez sur [feedback.azure.com](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails) si vous êtes intéressé par la personnalisation du corps de l’e-mail de vérification.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Comment puis-je migrer mes noms d’utilisateur, mots de passe et profils existants à partir de ma base de données vers Azure AD B2C ?
Vous pouvez utiliser l’API Graph Azure AD pour écrire l’outil de migration. Pour plus d’informations, consultez [l’exemple d’API Graph](active-directory-b2c-devquickstarts-graph-dotnet.md) .

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Quelle stratégie de mot de passe est utilisée pour les comptes locaux dans Azure AD B2C ?
La stratégie de mot de passe Azure AD B2C pour les comptes locaux est basée sur la stratégie pour Azure AD. Les stratégies de réinitialisation du mot de passe, d’inscription ou de connexion et d’inscription Azure AD B2C utilisent des mots de passe « forts » et qui n’expirent pas. Pour plus d’informations, consultez [Stratégie de mot de passe dans Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) .

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Puis-je utiliser Azure AD Connect pour migrer les identités de clients stockées dans mon répertoire Active Directory local vers Azure AD B2C ?
Non, Azure AD Connect n'est pas conçu pour fonctionner avec Azure AD B2C. Utilisez l’[API Graph](active-directory-b2c-devquickstarts-graph-dotnet.md) pour la migration des utilisateurs.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Mon application peut-elle ouvrir des pages Azure Active Directory B2C dans un iFrame ?
Non, pour des raisons de sécurité, les pages Azure AD B2C ne peuvent pas être ouvertes dans un iFrame.  Notre service communique avec le navigateur pour interdire les iFrames.  La communauté de sécurité en général et la spécification OAUTH2 déconseillent d’utiliser des iFrames pour les expériences d’identité en raison du risque d’attaques par détournement de clics.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C fonctionne-t-il avec les systèmes CRM, tels que Microsoft Dynamics ?
L’intégration de base avec le portail Microsoft Dynamics 365 sera bientôt disponible.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure AD B2C fonctionne-t-il avec SharePoint localement 2016 ou version antérieure ?
Azure AD B2C n’est pas conçu pour le scénario de partage partenaire externe SharePoint ; consultez plutôt cet article sur [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx).

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Dois-je utiliser Azure AD B2C ou B2B pour gérer les identités externes ?
Lisez cet article sur les [identités externes](../active-directory/active-directory-b2b-compare-external-identities.md) pour en savoir plus sur l’application des fonctionnalités appropriées à vos scénarios d’identités externes.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Quelles sont les fonctionnalités de création de rapports et d’audit proposées par Azure AD B2C ? Sont-elles identiques à celles d’Azure AD Premium ?
Non, Azure AD B2C ne prend pas en charge le même ensemble de rapports qu'Azure AD Premium. Toutefois, de nombreux points communs existent :

* Les rapports de connexion fournissent un enregistrement de chaque connexion avec des détails réduits.
* Les rapports d’audit sont disponibles dans le portail Azure, sous Azure Active Directory > ACTIVITÉ - Journaux d’audit > choisissez B2C et appliquez les filtres souhaités. Les activités d’administration et d’application sont traitées. 
* Un rapport d’utilisation, couvrant le nombre d’utilisateurs, le nombre de connexions et le volume de MFA est disponible via [l’API de création de rapports](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-usage-reporting-api).

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Puis-je localiser l'interface utilisateur des pages présentées par Azure AD B2C ? Quelles sont les langues prises en charge ?
Oui.  Découvrez la [personnalisation linguistique](active-directory-b2c-reference-language-customization.md), qui est en préversion publique.  Nous fournissons des traductions en 36 langues, et vous pouvez remplacer n’importe quelle chaîne pour l’adapter à vos besoins.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Puis-je utiliser mes propres URL dans les pages d’inscription et de connexion présentées par Azure AD B2C ? Par exemple, puis-je remplacer l’URL login.microsoftonline.com par login.contoso.com ?
Pas actuellement. Cette fonctionnalité est sur notre feuille de route. La vérification de votre domaine sous l’onglet **Domaines** sur le portail Azure Classic ne remplit pas cet objectif.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Comment supprimer mon client Azure AD B2C ?
Procédez comme suit pour supprimer votre client Azure AD B2C :

1. Suivez ces étapes pour [accéder au panneau de fonctionnalités B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) sur le portail Azure.
1. Accédez aux panneaux **Applications**, **Fournisseurs d’identité** et **Toutes les stratégies**, puis supprimez toutes les entrées dans chacun d’eux.
1. À présent, connectez-vous au [portail Azure Classic](https://manage.windowsazure.com/) en tant qu’administrateur d’abonnements. (Utilisez le compte professionnel ou scolaire ou le compte Microsoft que vous avez utilisé pour vous inscrire à Azure.)
1. Accédez à l’extension Active Directory sur la gauche, puis cliquez sur votre client B2C.
1. Cliquez sur l’onglet **Users** .
1. Sélectionnez chaque utilisateur tour à tour (excluez l’utilisateur Administrateur d’abonnement sous lequel vous êtes connecté actuellement). Cliquez sur **Supprimer** en bas de la page et sur **OUI** lorsque vous y êtes invité.
1. Cliquez sur l’onglet **Applications** .
1. Sélectionnez **Applications que ma société possède** dans la liste déroulante **Afficher** et cochez la case.
1. Une application a appelé **b2c-extensions-app**. Cliquez sur **Supprimer** en bas de la page et sur **OUI** lorsque vous y êtes invité.
1. Accédez à nouveau à l’extension Active Directory et sélectionnez votre client B2C.
1. Cliquez sur **Supprimer** en bas de la page. Pour terminer le processus, suivez les instructions à l’écran.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Puis-je obtenir Azure AD B2C dans le cadre d’Enterprise Mobility Suite ?
Non, Azure AD B2C est un service Azure avec paiement à l’utilisation et ne fait pas partie d’Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Comment signaler des problèmes avec Azure AD B2C ?
Consultez [Azure Active Directory B2C : dépôt de demandes de support](active-directory-b2c-support.md).

