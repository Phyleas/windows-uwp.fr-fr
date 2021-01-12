---
description: Sélectionnez le prix de base pour une application et planifiez les modifications de prix. Vous pouvez également personnaliser ces options pour des marchés spécifiques.
title: Définir et planifier les prix d’une application
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, tarification, tarification des applications, prix de l’application, vendre des applications, changement de prix, prix personnalisé, prix, prix, coût, remplacer le prix de base, prix de forme libre, forme libre
ms.localizationpriority: medium
ms.openlocfilehash: a01e5ea295889dfc6c288cfe899750729a816601
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104410"
---
# <a name="set-and-schedule-app-pricing"></a>Définir et planifier les prix d’une application

La section relative à la **tarification** de la page [tarification et disponibilité](set-app-pricing-and-availability.md) vous permet de sélectionner le prix de base d’une application. Vous pouvez également [planifier des changements de prix](#schedule-price-changes) pour indiquer la date et l’heure auxquelles le prix de votre application doit changer. En outre, vous avez la possibilité de [remplacer le prix de base pour des marchés spécifiques](#override-base-price-for-specific-markets), soit en sélectionnant un nouveau niveau de prix, soit en entrant un prix de forme libre dans la devise locale du marché.

> [!NOTE]
> Bien que cette rubrique fasse référence aux applications, la sélection du prix pour les envois de module complémentaire utilise le même processus. Notez que pour les [modules complémentaires d’abonnement](../monetize/enable-subscription-add-ons-for-your-app.md), le prix de base que vous sélectionnez ne peut jamais être augmenté (que ce soit en modifiant le prix de base ou en planifiant un changement de prix), bien qu’il puisse être réduit.

## <a name="base-price"></a>Prix de base

Lorsque vous sélectionnez le prix de **base** de votre application, ce prix est utilisé sur tous les marchés où votre application est vendue, sauf si vous remplacez le prix de base sur un ou plusieurs marchés.

Vous pouvez définir le **prix de base** sur **gratuit**, ou vous pouvez choisir un niveau de tarification disponible, qui définit le prix dans tous les pays où vous choisissez de distribuer votre application. Les niveaux de prix commencent à 0,99 USD, avec des niveaux supplémentaires disponibles à des incréments de croissance (1,09 USD, 1,19 USD, etc.). En général, les incréments augmentent à mesure que le prix est plus élevé. 

> [!NOTE]
> Ces niveaux tarifaires s’appliquent également aux modules complémentaires. 

Chaque niveau tarifaire a une valeur correspondante dans chacune des plus de 60 devises offertes par le magasin. Ces valeurs vous aident à vendre vos applications à un prix comparable dans le monde entier. Vous pouvez sélectionner votre prix de base dans n’importe quelle devise et nous utiliserons automatiquement la valeur correspondante pour différents marchés. Notez que, parfois, nous pouvons ajuster la valeur correspondante dans un certain marché pour tenir compte des modifications apportées aux taux de conversion monétaire.

Dans la section **tarification** , cliquez sur **afficher la table de conversion** pour afficher les prix correspondants dans toutes les devises. Il affiche également un numéro d’identification associé à chaque niveau tarifaire, dont vous aurez besoin si vous utilisez l' [API de soumission Microsoft Store](../monetize/manage-app-submissions.md#price-tiers) pour entrer des prix. Vous pouvez cliquer sur **Télécharger** pour télécharger une copie de la table de niveau de tarification sous forme de fichier. csv.

N'oubliez pas que le niveau de prix que vous sélectionnez peut inclure la taxe de vente ou la taxe sur la valeur ajoutée que vos clients doivent payer. Pour plus d’informations sur les implications fiscales de votre application dans les marchés sélectionnés, voir l’article [Informations fiscales pour les applications payantes](/partner-center/tax-details-marketplace). Vous devez également examiner les [Considérations sur les prix pour des marchés spécifiques](define-market-selection.md#price-considerations-for-specific-markets).

> [!NOTE]
> Si vous choisissez l’option **arrêter l’acquisition** sous **rendre ce produit disponible mais non détectable dans le Store** dans la section [visibilité](choose-visibility-options.md#discoverability) ), vous ne pourrez pas définir la tarification de votre soumission (puisque personne ne pourra acquérir l’application, sauf si elle utilise un code promotionnel pour obtenir gratuitement l’application).

## <a name="schedule-price-changes"></a>Planifier les changements de prix

Vous pouvez éventuellement planifier une ou plusieurs modifications de prix si vous souhaitez que le prix de base de votre application change à une date et une heure spécifiques. 

> [!IMPORTANT]
> Les changements de prix sont affichés uniquement aux clients sur les appareils Windows 10 (y compris la Xbox). Si votre application précédemment publiée prend en charge des versions de système d’exploitation antérieures, les changements de prix ne s’appliquent pas à ces clients. Pour les clients sur Windows 8, l’application sera toujours proposée à son **prix de base** (et non à un prix spécifique au marché), même si vous planifiez des changements de prix supplémentaires. Pour les clients sur Windows 8.1, et sur Windows Phone 8,1 et versions antérieures, l’application sera toujours proposée au premier niveau de prix pour le marché du client.

Cliquez sur **planifier un changement de prix** pour voir les options de modification du prix. Choisissez le niveau tarifaire que vous souhaitez utiliser (ou entrez un prix de forme libre pour les remplacements de prix de base sur un seul marché), puis sélectionnez la date, l’heure et le fuseau horaire.

Vous pouvez cliquer à nouveau sur **planifier une modification de prix** pour planifier autant de modifications ultérieures que vous le souhaitez.

> [!NOTE]
> Les changements de prix planifiés fonctionnent différemment du tarif de la [vente](put-apps-and-add-ons-on-sale.md). Lorsque vous mettez une application en vente, le prix s’affiche avec un barré dans le Store, et les clients peuvent acheter l’application au tarif de la vente au cours de la période que vous avez sélectionnée. Une fois la période de vente terminée, le prix de vente ne s’applique plus et l’application est disponible à son prix de base (ou un autre prix que vous avez spécifié pour ce marché, le cas échéant).
>
> Avec un changement de prix planifié, vous pouvez ajuster le prix pour qu’il soit plus ou moins élevé. La modification aura lieu à la date que vous spécifiez, mais elle ne sera pas affichée en tant que vente dans le magasin, ou une mise en forme spéciale sera appliquée. l’application aura juste un nouveau prix. 


## <a name="override-base-price-for-specific-markets"></a>Remplacer le prix de base pour des marchés spécifiques

Par défaut, les options que vous sélectionnez ci-dessus s’appliquent à tous les marchés dans lesquels votre application est proposée. Vous pouvez éventuellement modifier le prix d’un ou de plusieurs marchés, soit en choisissant un niveau de prix différent, soit en entrant un prix de forme libre dans la devise locale du marché.

> [!IMPORTANT]
> Si votre application précédemment publiée prend en charge Windows 8, ces clients verront toujours l’application à son **prix de base**, même si vous sélectionnez un prix différent pour leur marché.

Pour modifier le prix d’un marché spécifique, cliquez sur **Sélectionner des marchés pour remplacer le prix de base**. La fenêtre contextuelle de **sélection du marché** s’affiche, répertoriant tous les marchés dans lesquels vous avez choisi de rendre votre application disponible. (Si vous avez exclu un marché dans la section **Markets** , ces marchés ne seront pas disponibles.) 

Vous pouvez remplacer le prix de base pour un marché à la fois, ou pour un groupe de marchés ensemble. Une fois que vous avez effectué cette opération, vous pouvez remplacer le prix de base d’un autre marché (ou d’un autre groupe de marché) en sélectionnant **Sélectionner des marchés pour le remplacement du prix de base** et en répétant le processus décrit ci-dessous. Pour supprimer la tarification de remplacement que vous avez spécifiée pour un marché (ou un groupe de marché), cliquez sur **supprimer**.


### <a name="override-the-base-price-for-a-single-market"></a>Remplacer le prix de base pour un seul marché

Pour modifier le prix d’un seul marché, sélectionnez-le, puis cliquez sur **créer**. Vous voyez alors le même **prix de base** et **planifiez** les options de changement de prix comme décrit ci-dessus, mais les sélections que vous effectuez sont spécifiques à ce marché. Étant donné que vous remplacez le prix de base pour un seul marché, les niveaux de prix s’affichent dans la devise locale de ce marché. Vous pouvez cliquer sur **afficher la table de conversion** pour afficher les prix correspondants dans toutes les devises. 

Le fait de remplacer le prix de base pour un marché unique vous donne également la possibilité de saisir un prix de forme libre de votre choix dans la devise locale du marché. Vous pouvez entrer le prix de votre choix (dans une plage minimale et maximale), même s’il ne correspond pas à l’un des niveaux de prix standard. Ce prix sera utilisé uniquement pour les clients sur Windows 10 (y compris la Xbox) sur le marché sélectionné. 

> [!IMPORTANT]
> Si vous entrez un prix de forme libre, ce prix ne sera pas ajusté (même si le taux de conversion change), sauf si vous soumettez une mise à jour avec un nouveau prix. 

### <a name="override-the-base-price-for-a-market-group"></a>Remplacer le prix de base d’un groupe de marché

Pour remplacer le prix de base de plusieurs marchés, vous allez créer un *groupe de marché*. Pour ce faire, sélectionnez les marchés à inclure, puis, si vous le souhaitez, entrez un nom pour le groupe. (Ce nom est destiné à votre référence uniquement et n’est pas visible pour les clients.) Lorsque vous avez terminé, cliquez sur **créer**. Vous voyez alors le même **prix de base** et **planifiez** les options de changement de prix comme décrit ci-dessus, mais les sélections que vous effectuez sont spécifiques à ce groupe de marché. Notez que les prix de forme libre ne peuvent pas être utilisés avec les groupes de marché ; vous devez sélectionner un niveau de prix disponible.

Pour modifier les marchés inclus dans un groupe de marché, cliquez sur le nom du groupe de marché et ajoutez ou supprimez tous les marchés que vous souhaitez, puis cliquez sur **OK** pour enregistrer vos modifications. 

> [!NOTE]
> Un marché ne peut pas appartenir à plusieurs groupes de marché dans la section relative à la **tarification** .