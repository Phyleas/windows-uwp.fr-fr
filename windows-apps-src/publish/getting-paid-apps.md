---
Description: En savoir plus sur la réception de paiements pour vos applications, les modules complémentaires (produits dans l’application) et les revenus de la publicité.
title: Processus de paiement
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 05/29/2020
ms.topic: article
keywords: Windows 10, UWP, paiements, application Sales, application proceeds, paiement, frais de magasin, retenue de paiement, pourcentage
ms.localizationpriority: medium
ms.openlocfilehash: 0d42677aeda694e2fc8924cee1832b62d98b15e5
ms.sourcegitcommit: a937963ce63a14c254420926661b9b68be28a8ee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84746769"
---
# <a name="getting-paid"></a>Processus de paiement
Voici quelques informations importantes sur la réception des paiements pour vos applications, modules complémentaires et revenus publicitaires.

> [!IMPORTANT]
> Avant de pouvoir recevoir des sommes de l’application Sales dans le Microsoft Store, vous devez [configurer votre compte de paiement et remplir les formulaires fiscaux nécessaires](setting-up-your-payout-account-and-tax-forms.md).

> [!NOTE]
> Si vous avez besoin d’aide concernant les paiements, notamment sur la configuration des comptes de paiement, les paiements manquants, la mise en attente des paiements ou d’autres sujets, contactez le support [ici](https://developer.microsoft.com/windows/support).

## <a name="store-fee"></a>Frais de Store

Quand vous [créez un compte de développeur](https://developer.microsoft.com/store/register), vous acceptez le [Contrat développeur d’applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Le présent contrat explique la relation entre vous et Microsoft en ce qui concerne la vente d’applications dans le Microsoft Store, y compris les frais de stockage facturés par Microsoft pour chaque vente.

Ces frais sont officiellement définis dans le [Contrat développeur d’applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Consultez toujours ce document si vous avez des questions.

Les frais de stockage sont appliqués à toutes les ventes d’applications collectées par le Microsoft Store, y compris les modules complémentaires.


## <a name="price-tiers"></a>Niveaux de prix

Le ou les niveaux de prix que vous sélectionnez déterminent le [prix de vente](set-and-schedule-app-pricing.md#base-price) dans tous les pays où vous choisissez de distribuer votre application. Vous pouvez également utiliser des fonctionnalités de tarification supplémentaires telles que le [choix de différents prix pour différents marchés](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) ou [la promotion de votre application](put-apps-and-add-ons-on-sale.md).

Vous pouvez proposer votre application gratuitement, ou vous pouvez choisir un prix que les clients devront verser pour l’acquérir. Les niveaux de prix commencent à 0,99 USD, avec des incréments supplémentaires (1,09 USD, 1,19 USD, etc.). Les incréments entre les niveaux de prix augmentent à mesure que le prix est plus élevé.

> [!NOTE] 
> Ces niveaux tarifaires s’appliquent également aux modules complémentaires que vous offrez dans votre application.

Chaque niveau de prix a une valeur correspondante dans chacune des devises proposées par le Store. Ces valeurs vous aident à vendre vos applications à un prix comparable dans le monde entier. Toutefois, en raison des variations des cours des monnaies étrangères, le montant exact des ventes peut varier légèrement d’une devise à une autre. Les taux de change sont calculés mensuellement. En fonction du moment où votre transaction a eu lieu, le taux de change approprié est appliqué. Le taux de change et la plage de dates pour lesquels il était en vigueur sont indiqués sur votre rapport de paiement dans les colonnes exchangeRate et exchangeRateDate, respectivement.

Vous avez également la possibilité de saisir un prix libre de votre choix dans la devise locale d’un marché spécifique. Dans ce cas, le prix n’est pas ajusté (même si le taux de conversion change), sauf si vous soumettez une mise à jour avec un nouveau prix. 

Gardez à l’esprit que le prix que vous sélectionnez peut inclure la taxe sur les ventes ou la valeur ajoutée que vos clients doivent payer. Pour plus d'informations, consultez l'article [Informations fiscales pour les applications payantes](tax-details-for-paid-apps.md).


## <a name="payout-reporting"></a>Rapports de paiement

Vous pouvez accéder aux détails de vos informations de paiement et télécharger des rapports dans la page **Récapitulatif des paiements** de l’[Espace partenaires](https://partner.microsoft.com/dashboard). Pour en savoir plus sur les informations présentées ici et sur la façon dont nous classons l’argent que vous gagnez, consultez [Récapitulatif des paiements](payout-summary.md).


## <a name="payout-timeframe"></a>Délais de paiement

Les paiements sont effectués chaque mois (à condition que le seuil de paiement applicable soit atteint et que vous n’ayez pas mis votre paiement en attente comme indiqué ci-dessous). En général, nous envoyons tout paiement dû le 15 du mois donné. Notez que les paiements prennent généralement entre 3 et 10 jours ouvrables supplémentaires pour parvenir à votre compte de paiement. Pour plus d’informations, consultez [Seuils, modes et délais de paiement](payment-thresholds-methods-and-timeframes.md).


##  <a name="payout-hold-status"></a>État de paiement en attente

Par défaut, nous envoyons les paiements une fois par mois, comme décrit ci-dessus. Toutefois, vous avez la possibilité de mettre les paiements d’un programme en attente, ce qui nous empêche de les envoyer à votre compte. Si vous choisissez de mettre vos paiements en attente, nous continuerons à enregistrer les revenus que vous gagnez et à fournir les détails dans votre **Résumé de paiement**. Toutefois, nous n’enverrons les paiements à votre compte que lorsque vous aurez retiré la mise en attente.

Pour mettre vos paiements en attente, accédez à **Paramètres de développeur**. Sous **Paiement et taxe**, dans la section **Attribution de profil de paiement et de taxe**, recherchez le programme pour lequel vous souhaitez mettre les paiements en attente. Cochez la case **Conserver mon paiement** pour mettre les paiements de ce programme en attente. Vous pouvez modifier votre état de paiement en attente à tout moment, mais n’oubliez pas que votre décision aura un impact sur le paiement mensuel suivant. Par exemple, si vous souhaitez conserver le paiement d’avril, veillez à définir votre état d’attente de paiement **sur activé** avant la fin du mars.

Une fois que vous avez défini votre état de paiement en attente sur **Activé**, tous les paiements de ce programme sont bloqués tant que vous n’avez pas remis le curseur sur **Désactivé**. En procédant ainsi, vous serez inclus dans le prochain cycle de paiement mensuel (à condition que les seuils de paiement applicables aient été atteints). Par exemple, si vous avez des paiements en attente, mais que vous souhaitez que le paiement ait été généré en juin, veillez à désactiver l’état de maintien du paiement à **off** avant la fin de l’opération.

> [!NOTE]
> L’**état de paiement en attente** s’applique à chaque programme individuellement (Microsoft Store, publicité, Place de marché Azure, etc.). Si vous souhaitez mettre les paiements en attente sur tous vos programmes, vous devez le faire sur chaque programme individuellement.


 

 




