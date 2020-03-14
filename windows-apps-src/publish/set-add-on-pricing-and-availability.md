---
Description: Lors de la soumission d’un module complémentaire, les options de la page Tarification et disponibilité déterminent le prix et les conditions de disponibilité.
title: Définir le prix et la disponibilité d’un module complémentaire
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, extensions, iap, prix
ms.localizationpriority: medium
ms.openlocfilehash: 803164c395602313bcb84331e30376efd6832731
ms.sourcegitcommit: 912146681b1befc43e6db6e06d1e3317e5987592
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79295722"
---
# <a name="set-add-on-pricing-and-availability"></a>Définir le prix et la disponibilité d’un module complémentaire

Lors de l’envoi d’un module complémentaire dans l' [espace partenaires](https://partner.microsoft.com/dashboard), les options de la page **tarification et disponibilité** déterminent la quantité de facturation des clients pour votre module complémentaire et la façon dont il doit être proposé aux clients.

## <a name="markets"></a>Marchés

Par défaut, votre module complémentaire sera indiqué à son prix de base dans tous les marchés possibles, y compris dans les éventuels marchés que nous ajouterons par la suite.

Toutefois, comme pour les applications, vous avez la possibilité de choisir les marchés dans lesquels vous souhaitez proposer votre module complémentaire. Dans la plupart des cas, vous sélectionnerez le même ensemble de marchés que l’application, mais vous pouvez modifier les marchés sélectionnés à votre convenance. 

Pour en savoir plus et pour obtenir la liste complète des marchés disponibles, consultez l’article [Définir la sélection du marché](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilité

Vous pouvez déterminer si votre module complémentaire doit être proposé à l’achat aux clients. 

L’option par défaut est **Can be displayed in the parent product’s Store listing**. Laissez cette option sélectionnée pour les modules complémentaires qui seront disponibles pour tous les clients. 

Pour les extensions que vous ne voulez pas mettre à la disposition générale, sélectionnez **Masquée dans le Windows Store** et l’une des options suivantes :

-   **Disponible à l’achat dans le produit parent uniquement**: le choix de cette option permet à n’importe quel client d’acheter le module complémentaire à partir de votre application, mais le module complémentaire ne s’affiche pas dans le Listing de votre application ou peut être découvert dans le Windows Store. Utilisez cette option uniquement si l’offre n’est pas largement disponible, par exemple lors des périodes initiales de test interne.
-   **Empêcher l’acquisition : tout client disposant d’un lien direct peut voir la description du produit dans le Windows Store, mais ne peut le télécharger que s’il possède déjà le produit ou qu’il dispose d’un code promotionnel et utilise un appareil Windows 10. This add-on is not displayed in the parent product’s listing** : cette option signifie que l’extension ne s’affiche pas dans la description de votre application, et qu’aucun nouveau client ne peut acheter cette extension. Toutefois, **cette option n’est pas prise en charge pour les clients sur Windows 8.1 ou version antérieure**. Si votre application précédemment publiée est disponible sur Windows 8.1 ou une version antérieure, le module complémentaire sera toujours disponible pour achat à ces clients. Pour cesser d’offrir le module complémentaire aux clients sur Windows 8.1 ou version antérieure, vous devez mettre à jour votre application pour supprimer le code qui propose le module complémentaire, puis publier une nouvelle soumission pour l’application. Cela est recommandé même si votre application ne cible pas Windows 8.1 ou une version antérieure ; Il s’agit d’une meilleure expérience pour vos clients si vous ne leur offrez pas de module complémentaire que vous avez choisi de rendre indisponible.
    
 > [!NOTE] 
 > Le choix de l’option **arrêter l’acquisition** et/ou de l’envoi d’une mise à jour d’application qui supprime le module complémentaire de votre application n’empêchera pas les clients d’utiliser le module complémentaire s’ils l’ont déjà acheté. Les abonnements existants ne pourront pas être renouvelés et seront ensuite annulés après la fin du terme actuel.


## <a name="schedule"></a>Schedule

Par défaut (sauf si vous avez sélectionné l’une des options **Masquée dans le Windows Store** dans la section **Visibilité**), votre extension deviendra disponible pour les clients dès qu’elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** pour développer cette section. 

Pour plus d’informations, voir [Configurer le calendrier de publication exact](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Tarification

Vous devez sélectionner un prix de base pour votre module complémentaire (sauf si vous avez sélectionné l’option **arrêter l’acquisition** dans la section **visibilité** ). La sélection par défaut est **libre**. par conséquent, si vous souhaitez payer de l’argent pour le module complémentaire, veillez à choisir l’un des niveaux de prix disponibles (à partir de. 99 USD).

Vous pouvez également planifier des modifications de tarifs pour indiquer la date et l’heure auxquelles le prix de l’extension doit changer. En outre, vous pouvez personnaliser ces changements pour des marchés spécifiques. 

> [!TIP]
> Pour les modules complémentaires d’abonnement, vous ne pouvez pas augmenter le prix une fois que vous avez publié le module complémentaire, soit en sélectionnant un prix de base plus élevé dans une soumission ultérieure, soit en planifiant une modification de prix qui augmente le prix. Vous pouvez sélectionner un prix inférieur à l’aide de l’une de ces méthodes, mais une fois que le prix est réduit, vous ne pourrez plus l’augmenter au-delà de ce nouveau prix. Pour cette raison, il est particulièrement important de vous assurer que vous sélectionnez le niveau tarifaire approprié pour les modules complémentaires d’abonnement. 

Pour plus d’informations, voir [Définir et planifier le prix de l’application](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre module complémentaire à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, voir [Vendre des applications et des composants additionnels à prix réduit](put-apps-and-add-ons-on-sale.md).



