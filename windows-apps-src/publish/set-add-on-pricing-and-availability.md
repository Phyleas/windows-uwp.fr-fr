---
Description: Lors de la soumission d’un module complémentaire, les options de la page Tarification et disponibilité déterminent le prix et les conditions de disponibilité.
title: Définir le prix et la disponibilité d’un module complémentaire
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, modules complémentaires, IAP, prix
ms.localizationpriority: medium
ms.openlocfilehash: 0c53977ed26d19bb0860e9fb952cfebbe65c1e2f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174723"
---
# <a name="set-add-on-pricing-and-availability"></a>Définir le prix et la disponibilité d’un module complémentaire

Lors de l’envoi d’un module complémentaire dans l' [espace partenaires](https://partner.microsoft.com/dashboard), les options de la page **tarification et disponibilité** déterminent la quantité de facturation des clients pour votre module complémentaire et la façon dont il doit être proposé aux clients.

## <a name="markets"></a>Marchés

Par défaut, votre module complémentaire sera indiqué à son prix de base dans tous les marchés possibles, y compris dans les éventuels marchés que nous ajouterons par la suite.

Toutefois, comme pour les applications, vous avez la possibilité de choisir les marchés dans lesquels vous souhaitez proposer votre module complémentaire. Dans la plupart des cas, vous sélectionnerez le même ensemble de marchés que l’application, mais vous pouvez modifier les marchés sélectionnés à votre convenance. 

Pour plus d’informations et pour obtenir la liste complète des marchés disponibles, consultez [définir la sélection du marché](./define-market-selection.md).

## <a name="visibility"></a>Visibilité

Vous pouvez déterminer si votre module complémentaire doit être proposé à l’achat aux clients. 

L’option par défaut **peut être affichée dans la liste des magasins du produit parent**. Laissez cette option sélectionnée pour les modules complémentaires qui seront disponibles pour tous les clients. 

Pour les modules complémentaires que vous ne souhaitez pas rendre largement disponibles, sélectionnez **masqué dans le magasin** et l’une des options suivantes :

-   **Disponible à l’achat dans le produit parent uniquement**: le choix de cette option permet à n’importe quel client d’acheter le module complémentaire à partir de votre application, mais le module complémentaire ne s’affiche pas dans le Listing de votre application ou peut être découvert dans le Windows Store. Utilisez cette option uniquement si l’offre n’est pas largement disponible, par exemple lors des périodes initiales de test interne.
-   **Arrêter l’acquisition : tout client disposant d’un lien direct peut voir la liste des magasins du produit, mais il ne peut le télécharger que s’il en a déjà détenu le produit ou s’il a un code promotionnel et utilise un appareil Windows 10. Ce module complémentaire n’est pas affiché dans le Listing du produit parent**: le choix de cette option signifie que le module complémentaire ne sera pas affiché dans la liste de votre application et qu’aucun nouveau client ne pourra acheter le module complémentaire. Toutefois, **cette option n’est pas prise en charge pour les clients qui utilisent Windows 8.1 ou une version antérieure**. Si votre application précédemment publiée est disponible sur Windows 8.1 ou une version antérieure, le module complémentaire sera toujours disponible pour achat à ces clients. Pour arrêter de proposer ce module complémentaire aux clients qui utilisent Windows 8.1 ou une version antérieure, vous devrez mettre à jour votre application en supprimant le code proposant le module complémentaire, puis publier une nouvelle soumission de l’application. Nous vous recommandons de suivre cette procédure, même si votre application ne cible pas Windows 8.1 ou une version antérieure ; en effet, vos clients bénéficieront d’une meilleure expérience si vous ne leur proposez jamais un module complémentaire dont vous avez décidé d’arrêter la mise à disposition.
    
 > [!NOTE] 
 > Le choix de l’option **arrêter l’acquisition** et/ou de l’envoi d’une mise à jour d’application qui supprime le module complémentaire de votre application n’empêchera pas les clients d’utiliser le module complémentaire s’ils l’ont déjà acheté. Les abonnements existants ne pourront pas être renouvelés et seront ensuite annulés après la fin du terme actuel.


## <a name="schedule"></a>Planifier

Par défaut (sauf si vous avez sélectionné l’une des options **masquées dans le Store** dans la section **visibilité** ), votre module complémentaire sera disponible pour les clients dès qu’il passera la certification et terminez le processus de publication. Pour choisir d’autres dates, sélectionnez **afficher les options** pour développer cette section. 

Pour plus d’informations, consultez [configurer une planification de version précise](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Tarifs

Vous devez sélectionner un prix de base pour votre module complémentaire (sauf si vous avez sélectionné l’option **arrêter l’acquisition** dans la section **visibilité** ). La sélection par défaut est **libre**. par conséquent, si vous souhaitez payer de l’argent pour le module complémentaire, veillez à choisir l’un des niveaux de prix disponibles (à partir de. 99 USD).

Vous pouvez également planifier des changements de prix pour indiquer la date et l’heure auxquelles le prix du module complémentaire doit changer. En outre, vous avez la possibilité de personnaliser ces modifications pour des marchés spécifiques. 

> [!TIP]
> Pour les modules complémentaires d’abonnement, vous ne pouvez pas augmenter le prix une fois que vous avez publié le module complémentaire, soit en sélectionnant un prix de base plus élevé dans une soumission ultérieure, soit en planifiant une modification de prix qui augmente le prix. Vous pouvez sélectionner un prix inférieur à l’aide de l’une de ces méthodes, mais une fois que le prix est réduit, vous ne pourrez plus l’augmenter au-delà de ce nouveau prix. Pour cette raison, il est particulièrement important de vous assurer que vous sélectionnez le niveau tarifaire approprié pour les modules complémentaires d’abonnement. 

Pour plus d’informations, voir [Définir et planifier le prix de l’application](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre module complémentaire à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, voir [Vendre des applications et des composants additionnels à prix réduit](put-apps-and-add-ons-on-sale.md).