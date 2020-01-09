---
Description: Ciblez des segments de clients spécifiques avec un contenu personnalisé pour augmenter l’engagement, la rétention et la monétisation.
title: Utiliser des offres ciblées pour optimiser l’engagement et les conversions
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, offres ciblées, offres, notifications
ms.localizationpriority: medium
ms.openlocfilehash: fd4cf135e5129ed4f3dbdbcebfb2003e7573182a
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685101"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Utiliser des offres ciblées pour optimiser l’engagement et les conversions

Ciblez des segments de clients spécifiques avec un contenu attractif et personnalisé pour augmenter l’engagement, la rétention et la monétisation.

> [!IMPORTANT]
> Les offres ciblées ne sont utilisables qu’avec les applications UWP qui comprennent des extensions.

## <a name="targeted-offer-overview"></a>Vue d’ensemble des offres ciblées

De façon générale, vous devez effectuer trois opérations pour utiliser des offres ciblées :

1. **Créez l’offre dans l' [espace partenaires](https://partner.microsoft.com/dashboard).** Accédez à la page **Engager > Offres ciblées** pour créer des offres. Vous trouverez d’autres informations sur ce processus ci-dessous.
2. **Implémentez l’expérience d’offre dans l’application.** Utilisez l' *API Microsoft Store ciblée offres* dans le code de votre application pour récupérer les offres disponibles pour un utilisateur donné. Vous devez également créer une expérience interne à l’application pour l’offre ciblée. Pour plus d’informations, consultez l’article [Gérer les offres ciblées à l’aide des services du Windows Store](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Envoyez votre application au Windows Store.** Votre application doit être publiée avec la mise en place des offres dans l’application afin que les offres soit mises à la disposition des clients.

Une fois que vous avez exécuté ces étapes, les clients qui utilisent votre application voient s’afficher les offres qui leur sont disponibles à ce stade, en fonction de leur appartenance aux segments associés à vos offres. Veuillez noter que, bien que nous nous efforcions d’afficher toutes les offres disponibles pour vos clients, certains problèmes susceptibles d’affecter la disponibilité des offres peuvent parfois survenir.


## <a name="to-create-and-send-a-targeted-offer"></a>Pour créer et envoyer une offre ciblée

1.  Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), développez **engagement** dans le menu de navigation gauche, puis sélectionnez **offres ciblées**.
2.  Sur la page **Offres ciblées**, examinez les offres disponibles. Sélectionnez **Créer une nouvelle offre** pour toute offre que vous souhaitez implémenter.

    > [!NOTE]
    > Les offres disponibles que vous verrez s’afficher peuvent varier au fil du temps et selon les critères du compte.

3.  Dans la nouvelle ligne qui s’affiche sous les offres disponibles, sélectionnez le produit (application) dans lequel l’offre sera disponible. Ensuite, sélectionnez l’extension que vous souhaitez associer à l’offre.
4.  Si vous souhaitez créer des offres supplémentaires, répétez les étapes 2 et 3. Vous pouvez implémenter le même type d’offre plusieurs fois pour la même application, tant que vous sélectionnez des extensions différentes pour chaque offre. En outre, vous pouvez associer la même extension à plusieurs types d’offres.
5.  Lorsque vous avez fini de créer les offres, cliquez sur **Enregistrer**.

Une fois que vous avez implémenté vos offres, vous pouvez revenir à la page **offres ciblées** dans l’espace partenaires pour afficher les conversions totales pour chaque offre.

Si vous décidez de ne pas utiliser une offre (ou que vous ne voulez plus l’utiliser), cliquez sur **Supprimer**.

> [!IMPORTANT]
> Assurez-vous que vous avez publié le code pour récupérer les offres disponibles pour un utilisateur donné et pour créer l’expérience interne à l’application. Pour plus d’informations, consultez l’article [Gérer les offres ciblées à l’aide des services du Windows Store](../monetize/manage-targeted-offers-using-windows-store-services.md).
>
> Lorsque vous étudiez le contenu de vos offres ciblées, n’oubliez pas que, comme pour tous les contenus d’application, le contenu de vos offres doit respecter les [Politiques relatives au contenu](https://docs.microsoft.com/legal/windows/agreements/store-policies) du Windows Store.
>
> Notez également que si un client qui utilise votre application (et qui est connecté à son compte Microsoft au moment où son appartenance au segment est déterminée) confie ultérieurement son appareil à un tiers pour que celui-ci l’utilise, ce tiers peut voir les offres destinées au client initial.
