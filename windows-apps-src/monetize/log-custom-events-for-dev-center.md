---
description: Vous pouvez consigner des événements personnalisés à partir de votre application UWP et consulter ces événements dans le rapport d’utilisation dans l’espace partenaires.
title: Journaliser des événements personnalisés pour l’Espace partenaires
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store services SDK, journaux d’événements
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 3cc89272984ffdda47c488e7bb2f24b4f37d3a41
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033452"
---
# <a name="log-custom-events-for-partner-center"></a>Journaliser des événements personnalisés pour l’Espace partenaires

Le [rapport d’utilisation](../publish/usage-report.md) de l’espace partenaires vous permet d’obtenir des informations sur les événements personnalisés que vous avez définis dans votre application plateforme Windows universelle (UWP). Un événement personnalisé est une chaîne arbitraire qui représente un événement ou une activité dans votre application. Par exemple, un jeu peut définir des événements personnalisés nommés *firstLevelPassed* , *secondLevelPassed* , etc., qui sont consignés lors de chaque passage au niveau supérieur de l’utilisateur.

Pour consigner un événement à partir de votre application, passez la chaîne d’événement à la méthode [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) fournie par le Microsoft Store Services SDK. Vous pouvez consulter le nombre total d’occurrences de vos événements personnalisés dans la section **événements personnalisés** du [rapport d’utilisation](../publish/usage-report.md) dans l’espace partenaires.

> [!NOTE]
> Les événements personnalisés que vous enregistrez dans l’espace partenaires ne sont pas liés aux [événements Windows](/windows/desktop/Events/windows-events)et ils n’apparaissent pas dans **Observateur d’événements** .

## <a name="prerequisites"></a>Prérequis

Avant de pouvoir consulter les événements de journalisation personnalisés dans le **rapport d’utilisation** de votre application dans l’espace partenaires, votre application doit être publiée dans le Windows Store.

## <a name="how-to-log-custom-events"></a>Comment consigner des événements personnalisés

1. Si vous ne l’avez pas encore fait, [Installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) sur votre ordinateur de développement.

2. Ouvrez votre projet dans Visual Studio.

3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud **Références** , puis sélectionnez **Ajouter une référence** .

4. Dans le **Gestionnaire de références** , développez **Windows universel** , puis cliquez sur **Extensions** .

5. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK** .

6. Ajoutez l’instruction suivante en haut de chaque fichier de code dans lequel vous souhaitez consigner des événements personnalisés.
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="EngagementNamespace":::

7. Dans chaque section de votre code où vous souhaitez consigner un événement personnalisé, récupérez un objet [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log), puis appelez la méthode [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log). Transmettez votre chaîne d’événements personnalisés à la méthode.
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="Log":::

    > [!NOTE]
    > Le chargement du [rapport d’utilisation](../publish/usage-report.md) peut prendre beaucoup de temps si votre application journalise de nombreux événements personnalisés avec des noms longs. Nous vous recommandons d’utiliser des noms courts pour vos événements personnalisés. 

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’utilisation](../publish/usage-report.md)
* [Méthode log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
