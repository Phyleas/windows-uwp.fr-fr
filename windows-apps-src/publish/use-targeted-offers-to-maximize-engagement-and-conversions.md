---
Description: Ciblez des segments spécifiques de vos clients avec un contenu personnalisé pour augmenter l’engagement, la rétention et la monétisation.
title: Utiliser des offres ciblées pour optimiser l’engagement et les conversions
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, offres ciblées, offres, notifications
ms.localizationpriority: medium
ms.openlocfilehash: ff2c3049154ffcc18164b8084bc9e34a9121905d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155303"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Utiliser des offres ciblées pour optimiser l’engagement et les conversions

Ciblez des segments spécifiques de vos clients avec un contenu attrayant et personnalisé pour augmenter l’engagement, la rétention et la monétisation.

> [!IMPORTANT]
> Les offres ciblées peuvent uniquement être utilisées avec des applications UWP qui incluent des modules complémentaires.

## <a name="targeted-offer-overview"></a>Présentation de l’offre ciblée

À un niveau élevé, vous devez effectuer trois opérations pour utiliser des offres ciblées :

1. **Créez l’offre dans l' [espace partenaires](https://partner.microsoft.com/dashboard).** Accédez à la page **impliquer > offres ciblées** pour créer des offres. Pour plus d’informations sur ce processus, voir ci-dessous.
2. **Implémentez l’expérience d’offre dans l’application.** Utilisez l' *API Microsoft Store ciblée offres* dans le code de votre application pour récupérer les offres disponibles pour un utilisateur donné. Vous devez également créer l’expérience dans l’application pour l’offre ciblée. Pour plus d’informations, consultez [gérer les offres ciblées à l’aide des services de magasin](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Envoyez votre application au Windows Store.** Votre application doit être publiée avec l’expérience d’offre en place dans l’application afin que les offres soient mises à la disposition des clients.

Une fois ces étapes terminées, les clients qui utilisent votre application verront les offres disponibles à ce moment-là, en fonction de leur appartenance au (x) segment (s) associé (s) à vos offres. Notez que même si nous nous efforçons de montrer toutes les offres disponibles à vos clients, il peut y avoir des problèmes susceptibles d’affecter la disponibilité de l’offre.


## <a name="to-create-and-send-a-targeted-offer"></a>Pour créer et envoyer une offre ciblée

1.  Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), développez **engagement** dans le menu de navigation gauche, puis sélectionnez **offres ciblées**.
2.  Sur la page **offres ciblées** , passez en revue les offres disponibles. Sélectionnez **créer une offre** pour toute offre que vous souhaitez implémenter.

    > [!NOTE]
    > Les offres disponibles que vous verrez peuvent varier au fil du temps et en fonction des critères du compte.

3.  Dans la nouvelle ligne qui s’affiche sous les offres disponibles, choisissez le produit (l’application) dans lequel l’offre sera disponible. Sélectionnez ensuite le module complémentaire que vous souhaitez associer à l’offre.
4.  Répétez les étapes 2 et 3 Si vous souhaitez créer des offres supplémentaires. Vous pouvez implémenter le même type d’offre plusieurs fois pour la même application, à condition de sélectionner différents modules complémentaires pour chaque offre. En outre, vous pouvez associer le même module complémentaire à plusieurs types d’offres.
5.  Lorsque vous avez terminé de créer des offres, cliquez sur **Enregistrer**.

Une fois que vous avez implémenté vos offres, vous pouvez revenir à la page **offres ciblées** dans l’espace partenaires pour afficher les conversions totales pour chaque offre.

Si vous décidez de ne pas utiliser une offre (ou si vous ne souhaitez plus continuer à l’utiliser, cliquez sur **Supprimer.**

> [!IMPORTANT]
> Vérifiez que vous avez publié le code pour récupérer les offres disponibles pour un utilisateur donné et pour créer l’expérience dans l’application. Pour plus d’informations, consultez [gérer les offres ciblées à l’aide des services de magasin](../monetize/manage-targeted-offers-using-windows-store-services.md).
>
> Lorsque vous examinez le contenu de vos offres ciblées, gardez à l’esprit que, comme avec tout le contenu de l’application, le contenu de vos offres doit être conforme aux [stratégies de contenu](/legal/windows/agreements/store-policies)du Store.
>
> Sachez également que si un client qui utilise votre application (et qui est connecté avec son compte Microsoft au moment où l’appartenance au segment est déterminée) donne à son appareil une autre personne à utiliser, l’autre personne peut voir les offres qui ont été ciblées au niveau du client d’origine.