---
title: "Guide pratique pour retirer l’accès d’un utilisateur à une application | Microsoft Docs"
description: "Comprendre comment retirer l’accès d’un utilisateur à une application"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: 8d4f2cec35a8edfec9b8830a077b8aa65ca0c229
ms.contentlocale: fr-fr
ms.lasthandoff: 04/11/2017

---

<a id="how-to-remove-a-users-access-to-an-application" class="xliff"></a>

# Guide pratique pour retirer l’accès d’un utilisateur à une application

Cet article vous aide à comprendre comment retirer l’accès d’un utilisateur à une application.

<a id="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application" class="xliff"></a>

## Je souhaite supprimer l’attribution d’une application à un utilisateur ou groupe spécifique

Pour supprimer l’attribution d’une application à un utilisateur ou groupe, suivez les étapes répertoriées dans l’article [Supprimer l’attribution d’une application à un utilisateur ou groupe à partir d’une application d’entreprise dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal).

. ## je souhaite désactiver tous les accès à une application pour chaque utilisateur

Pour désactiver toutes les connexions des utilisateurs à une application, suivez les étapes répertoriées dans l’article [Désactiver les connexions des utilisateurs à une application d’entreprise dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal).

<a id="i-want-to-delete-an-application-entirely" class="xliff"></a>

## Je veux complètement supprimer une passerelle d’application

Pour **supprimer une application**, suivez les instructions ci-dessous :

1.  Ouvrez le [**portail Azure**](https://portal.azure.com/) et connectez-vous en tant qu’**administrateur général** ou **coadministrateur.**

2.  Ouvrez l’**extension Azure Active Directory** en cliquant sur **Plus de services** en bas du menu de navigation principal de gauche.

3.  Tapez « **Azure Active Directory** » dans la zone de recherche du filtre et sélectionnez l’élément **Azure Active Directory**.

4.  Cliquez sur **Applications d’entreprise** dans le menu de navigation de gauche d’Azure Active Directory.

5.  Cliquez sur **Toutes les applications** pour afficher une liste de toutes vos applications.

   * Si l’application que vous recherchez n’apparaît pas, utilisez la commande **Filtre** en haut de la **liste de toutes les applications** et définissez l’option **Afficher** sur **Toutes les applications.**

6.  Sélectionnez l’application que vous voulez supprimer.

7.  Une l’application chargée, cliquez sur l’icône **Supprimer** dans le panneau supérieur **Vue d’ensemble** de l’application.

<a id="i-want-to-disable-all-future-user-consent-operations-to-any-application" class="xliff"></a>

## Je veux désactiver toutes les futures opérations de consentement de l’utilisateur pour n’importe quelle application

La désactivation du consentement de l’utilisateur pour votre annuaire entier empêche les utilisateurs finaux de donner leur consentement pour n’importe quelle application. Les administrateurs peuvent toujours donner leur consentement au nom de l’utilisateur. Pour en savoir plus sur le consentement de l’application et les conditions pour donner ou refuser ce consentement, lisez la rubrique [Comprendre le consentement de l’utilisateur et de l’administrateur](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

Pour **désactiver toutes les futures opérations de consentement de l’utilisateur dans votre annuaire entier**, suivez les instructions ci-dessous :

1.  Ouvrez le [**portail Azure**](https://portal.azure.com/) et connectez-vous en tant qu’**administrateur général.**

2.  Ouvrez l’**extension Azure Active Directory** en cliquant sur **Plus de services** en bas du menu de navigation principal de gauche.

3.  Tapez « **Azure Active Directory** » dans la zone de recherche du filtre et sélectionnez l’élément **Azure Active Directory**.

4.  Cliquez sur **Utilisateurs et groupes** dans le menu de navigation.

5.  Cliquez sur **Paramètres utilisateur**.

6.  Désactivez toutes les futures opérations de consentement de l’utilisateur en définissant l’option **Les utilisateurs peuvent autoriser les applications à accéder à leurs données** sur **Non**, puis cliquez sur le bouton **Enregistrer**.


<a id="next-steps" class="xliff"></a>

# Étapes suivantes
[Gestion de l’accès aux applications](active-directory-managing-access-to-apps.md)

