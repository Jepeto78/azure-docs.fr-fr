---
title: Delegating offers in Azure Stack | Microsoft Docs
description: Learn how to put other people in charge of creating offers and signing up users for you.
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2016
ms.author: alfredop
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: a082e17ef6bdbc6ffe475a59c613d684583530f2
ms.contentlocale: fr-fr
ms.lasthandoff: 07/10/2017


---
# <a name="delegating-offers-in-azure-stack"></a>Delegating offers in Azure Stack

As the Azure Stack cloud operator, you often want to put other people in charge of creating offers and signing up users for you. For example, this can happen if you are a service provider and you want resellers to sign up customers and manage them on your behalf. It can also happen in an enterprise if you are part of a central IT group and want divisions or subsidiaries to sign up users without your intervention.

Delegation helps you with these tasks, helping you to reach and manage more users than you would be able to do directly. The following illustration shows one level of delegation, but Azure Stack supports multiple levels. Delegated providers can in turn delegate to other providers, up to five levels.

![](media/azure-stack-delegated-provider/image1.png)

Azure Stack cloud operators can delegate the creation of offers and tenants to other users by using the delegation functionality.

## <a name="roles-and-steps-in-delegation"></a>Roles and steps in delegation
To understand delegation, keep in mind that there are three roles involved:

* The **cloud operator** manages the Azure Stack infrastructure, creates an offer template, and delegates others to offer it to their users.
* The delegated cloud administrators are called **delegated providers**. They can belong to other organizations (such as other Azure Active Directory tenants).
* **Users** sign up for the offers and use them for managing their workloads, creating VMs, storing data, etc.

As shown in the following graphic, there are two steps in setting up delegation.

1. Identify the delegated providers. Do this by subscribing them to an offer based on a plan that contains only the subscriptions service.
   Users who subscribe to this offer acquire some of the cloud administrator’s capabilities, including the ability to extend offers and sign users up for them.
2. Delegate an offer to the delegated provider. This offer functions as a template for what the delegated provider can offer. The delegated provider is now able to take the offer, choose a name for it (but not change its services and quotas), and offer it to customers.

![](media/azure-stack-delegated-provider/image2.png)

To act as delegated providers, users need to establish a relationship with the main provider; in other words, they need to create a subscription. In this scenario, this subscription identifies the delegated providers as having the right to present offers on behalf of the main provider.

Once this relationship is established, the cloud operator can delegate an offer to the delegated provider. The delegated provider is now able to take the offer, rename it (but not change its substance), and offer it to its customers.

To establish a delegated provider, delegate an offer, and verify that users can sign up for it, carry out the instructions in the following sections.

## <a name="set-up-roles"></a>Set up roles

To see a delegated provider at work, you need additional Azure Active Directory accounts in addition to your cloud operator account. If you do not have them, create the two accounts. The accounts can belong to any AAD tenant. We will refer to them as the delegated provider (DP) and the user.

| **Role** | **Organizational rights** |
| --- | --- |
| Delegated Provider |User |
| User |User |

## <a name="identify-the-delegated-providers"></a>Identify the delegated providers
1. Sign in as cloud operator.
2. Create the offer that will enable users to become delegated providers. This requires that you create a plan and an offer based on it:
   
   a.  [Create a plan](azure-stack-create-plan.md).
       This plan should include only the subscriptions service. In this article, we use a plan called PlanForDelegation.
   
   b.  [Create an offer](azure-stack-create-offer.md) based on this plan. In this article, we use an offer called OfferToDP.
   
   c.  Once the creation of the offer is complete, add the delegated provider as a subscriber to this offer by clicking **Subscriptions** &gt; **Add** &gt; **New Tenant Subscription**.
   
   ![](media/azure-stack-delegated-provider/image3.png)

> [!NOTE]
> As with all Azure Stack offers, you have the option of making the offer public and letting users sign up for it, or keeping it private and have the cloud operator manage the sign-up. Delegated providers are usually a small group and you want to control who is admitted to it, so keeping this offer private will make sense in most cases.
> 
> 

## <a name="cloud-operator-creates-the-delegated-offer"></a>Cloud operator creates the delegated offer

You have now established your delegated provider. The next step is to create the plan and offer that you are going to delegate, and which your customers will use. You should define this offer exactly as you want the customers to see it, because the delegated provider will not be able to change the plans and quotas it includes.

1. As cloud operator, [create a plan](azure-stack-create-plan.md) and [an offer](azure-stack-create-offer.md) based on it. For this article, we use an offer called DelegatedOffer.
   
   > [!NOTE]
   > This offer does not need to be made public. It can be made public if you choose, but, in most cases, you only want delegated providers to have access to it. Once you delegate a private offer as described in the following steps, the delegated provider will have access to it.
   > 
   > 
2. Delegate the offer. Go to DelegatedOffer, and in the Settings pane, click **Delegated Providers** &gt; **Add**.
3. Select the delegated provider’s subscription from the drop-down list box and click **Delegate**.

> ![](media/azure-stack-delegated-provider/image4.png)
> 
> 

## <a name="delegated-provider-customizes-the-offer"></a>Delegated provider customizes the offer

Sign in to the **tenant portal** as the delegated provider and create a new offer using the delegated offer as a template.

1. Click **New** &gt; **Tenant Offers + Plans** &gt; **Offer**.

    ![](media/azure-stack-delegated-provider/image5.png)


1. Assign a name to the offer. Here we choose ResellerOffer. Select the delegated offer to base it on and then click **Create**.
   
   ![](media/azure-stack-delegated-provider/image6.png)

    >[!NOTE] 
    > Note the difference compared to offer creation as experienced by the cloud operator. The delegated provider does not construct the offer from base plans and add-on plans; they can only choose from offers that have been delegated to them, and can't make changes to those offers.

1. Make the offer public by clicking **Browse** &gt; **Offers**, selecting the offer, and clicking **Change State**.
2. The delegated provider exposes these offers through his or her own portal URL. Note that these offers are visible only through this    delegated portal. To find and change this URL:
   
    a.  Click **Browse**&gt; **Provider Settings** &gt; **Portal URL**.
   
    b.  Change the Provider ID if desired.
   
    c.  Copy the portal URL to a separate location, such as Notepad.
   
    ![](media/azure-stack-delegated-provider/image7.png)
   
   <!-- -->
   You have now completed the creation of a delegated offer as a delegated provider. Sign out as the delegated provider. Close the browser tab you have been using.

## <a name="sign-up-for-the-offer"></a>Sign up for the offer
1. In a new browser window, go to the delegated portal URL you saved in the previous step. Sign in to the portal as user. Note: you must use the delegated portal for this step. The delegated offer will not be visible otherwise.
2. In the dashboard, click **Get a subscription**. You will see that only the delegated offers created by the delegated provider are presented to the user:

> ![](media/azure-stack-delegated-provider/image8.png)
> 
> 

This concludes the process of offer delegation. The user can now sign up for this offer by getting a subscription for it.

## <a name="multiple-tier-delegation"></a>Multiple-tier delegation

Multiple-tier delegation allows the delegated provider to delegate the offer to other entities. This allows, for example, the creation of deeper reseller channels, in which the provider managing Azure Stack delegates an offer to a distributor, who in turn delegates to reseller.
Azure Stack supports up to five levels of delegation.

To create multiple tiers of offer delegation, the delegated provider in turn delegates the offer to the next provider. The process is the same for the delegated provider as it was for the cloud operator (see [Cloud operator creates the delegated offer](#cloud-operator-creates-the-delegated-offer)).

## <a name="next-steps"></a>Next steps
[Provision a VM](azure-stack-provision-vm.md)


