---
author: JnHs
Description: Target specific segments of your customers with personalized content to increase engagement, retention, and monetization.
title: Utilisez des offres ciblées pour optimiser l’engagement et les conversions
ms.author: wdg-dev-content
ms.date: 11/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, offres ciblées, offres, notifications
ms.localizationpriority: medium
ms.openlocfilehash: 727c438bacf51fd2ead03df72421363116c4701b
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2834337"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Utilisez des offres ciblées pour optimiser l’engagement et les conversions

Ciblez des segments de clients spécifiques avec un contenu attractif et personnalisé pour augmenter l’engagement, la rétention et la monétisation.

> [!IMPORTANT]
> Les offres ciblées ne sont utilisables qu’avec les applicationsUWP qui comprennent des extensions.

## <a name="targeted-offer-overview"></a>Vue d’ensemble des offres ciblées

De façon générale, vous devez effectuer trois opérations pour utiliser des offres ciblées:

1. **Créez l’offre dans votre tableau de bord.** Accédez à la page **Engager > Offres ciblées** pour créer des offres. Vous trouverez d’autres informations sur ce processus ci-dessous.
2. **Mettre en place les offres intégrées aux applications.** Utiliser *banque Microsoft ciblés offre des API* dans le code de votre application pour récupérer les offres disponibles pour un utilisateur donné. Vous devez également créer une expérience interne à l’application pour l’offre ciblée. Pour plus d’informations, consultez l’article [Gérer les offres ciblées à l’aide des services du WindowsStore](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Soumettez votre application au WindowsStore.** Votre application doit être publiée avec la mise en place des offres dans l’application afin que les offres soit mises à la disposition des clients.

Une fois que vous avez exécuté ces étapes, les clients qui utilisent votre application voient s’afficher les offres qui leur sont disponibles à ce stade, en fonction de leur appartenance aux segments associés à vos offres. Veuillez noter que, bien que nous nous efforcions d’afficher toutes les offres disponibles pour vos clients, certains problèmes susceptibles d’affecter la disponibilité des offres peuvent parfois survenir.


## <a name="to-create-and-send-a-targeted-offer"></a>Pour créer et envoyer une offre ciblée

Suivez la procédure ci-après pour créer une offre ciblée dans le tableau de bord.

1.  Dans le tableau de bord du Centre de développement Windows, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Offres ciblées**.
2.  Sur la page **Offres ciblées**, examinez les offres disponibles. Sélectionnez **Créer une nouvelle offre** pour toute offre que vous souhaitez implémenter.

    > [!NOTE]
    > Les offres disponibles que vous verrez s’afficher peuvent varier au fil du temps et selon les critères du compte.

3.  Dans la nouvelle ligne qui s’affiche sous les offres disponibles, sélectionnez le produit (application) dans lequel l’offre sera disponible. Ensuite, sélectionnez l’extension que vous souhaitez associer à l’offre.
4.  Si vous souhaitez créer des offres supplémentaires, répétez les étapes2 et 3. Vous pouvez implémenter le même type d’offre plusieurs fois pour la même application, tant que vous sélectionnez des extensions différentes pour chaque offre. En outre, vous pouvez associer la même extension à plusieurs types d’offres.
5.  Lorsque vous avez fini de créer les offres, cliquez sur **Enregistrer**.

Une fois que vous avez implémenté vos offres, vous pouvez revenir à la page **Offres ciblées** de votre tableau de bord pour visualiser le nombre total de conversions pour chaque offre.

Si vous décidez de ne pas utiliser une offre (ou que vous ne voulez plus l’utiliser), cliquez sur **Supprimer**.

> [!IMPORTANT]
> Assurez-vous que vous avez publié le code pour récupérer les offres disponibles pour un utilisateur donné et pour créer l’expérience interne à l’application. Pour plus d’informations, consultez l’article [Gérer les offres ciblées à l’aide des services du WindowsStore](../monetize/manage-targeted-offers-using-windows-store-services.md).
>
> Lorsque vous étudiez le contenu de vos offres ciblées, n’oubliez pas que, comme pour tous les contenus d’application, le contenu de vos offres doit respecter les [Politiques relatives au contenu](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies) du WindowsStore.
>
> Notez également que si un client qui utilise votre application (et qui est connecté à son compte Microsoft au moment où son appartenance au segment est déterminée) confie ultérieurement son appareil à un tiers pour que celui-ci l’utilise, ce tiers peut voir les offres destinées au client initial.