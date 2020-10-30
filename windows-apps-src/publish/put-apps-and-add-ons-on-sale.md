---
description: Vous pouvez promouvoir votre application ou le module complémentaire dans le Microsoft Store en le plaçant en vente pendant une période limitée.
title: Commercialiser des applications et composants additionnels
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5bd1a5f6961e3b1caaab29a6a3fb9ecef2e9d6ef
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035002"
---
# <a name="put-apps-and-add-ons-on-sale"></a>Commercialiser des applications et composants additionnels

Vous pouvez promouvoir votre application ou le module complémentaire dans le Microsoft Store en le plaçant en vente pendant une période limitée. Vous pouvez choisir de proposer le produit à un niveau de prix inférieur ou avec une remise basée sur un pourcentage. Vous pouvez choisir de proposer la vente à tout le monde ou d’en faire une offre exclusive pour les clients qui possèdent l’un de vos autres produits.

> [!NOTE]
> La tarification des ventes n’est pas prise en charge pour les modules complémentaires d’abonnement.

Lorsque vous utilisez la section tarification de la **vente** de la page **tarification et disponibilité** d’une soumission pour réduire temporairement le prix de votre application ou de votre module complémentaire, les clients qui consultent le contenu de votre boutique voient la tarification en barré indiquant que le prix a été réduit (par opposition à un [changement de prix planifié](set-and-schedule-app-pricing.md#schedule-price-changes), ce qui peut réduire ou augmenter le prix sans l’afficher en tant que modification du 

Pendant la période pendant laquelle votre produit est en vente, les clients peuvent l’acheter au tarif le plus bas au cours de la période que vous avez sélectionnée. Si vous définissez le prix sur **Gratuit** , les clients peuvent télécharger l’application sans rien payer pendant la période de vente.

> [!IMPORTANT]
> La tarification de la vente est présentée uniquement à vos clients sur les appareils Windows 10, y compris Xbox One. Les ventes que vous offrez uniquement aux propriétaires de l’un de vos autres produits s’affichent uniquement pour les clients sur Windows 10, version 1607 ou ultérieure.
> 
> Sur les autres systèmes d’exploitation, les clients verront le prix normal de votre application ou de votre module complémentaire, et ne pourront pas l’acheter au prix de vente. Vous pouvez toujours modifier un prix en choisissant un autre niveau de prix dans une nouvelle soumission, mais il ne sera pas affiché en tant que vente à prix réduit limitée dans le temps.


## <a name="scheduling-a-sale"></a>Planification d’une vente à prix réduit

Les ventes à prix réduit sont planifiées dans le cadre de la soumission d’une application ou d’un module complémentaire. Si vous souhaitez planifier une vente à prix réduit pour une application ou un module complémentaire déjà publié(e), vous devez créer une nouvelle soumission, même s’il s’agit de la seule modification que vous voulez apporter.

**Pour planifier une vente**

1. Sur la page **Tarification et disponibilité** d’une soumission d’application ou de module complémentaire en cours, accédez à la section **Prix de vente** .
2. Sélectionnez **afficher les options** , puis sélectionnez **nouvelle vente** .
3. La fenêtre contextuelle de **sélection du marché** s’affiche, ce qui vous permet de créer un *groupe de marché* qui spécifie le ou les marchés dans lesquels la vente doit être proposée. Vous pouvez cliquer sur **Sélectionner tout** pour offrir la vente à chaque marché dans lequel votre application est disponible, sélectionner un marché individuel ou sélectionner plusieurs marchés. Vous pouvez éventuellement entrer un nom pour votre groupe de marché. Une fois vos sélections effectuées, cliquez sur **créer** . (Pour modifier ultérieurement les marchés du groupe, cliquez sur son nom.)

   > [!NOTE]
   > Les sélections de marché que vous effectuez dans la section sur la tarification de la vente n’affecteront pas les marchés dans lesquels l’application est proposée ; ces sélections déterminent uniquement si un prix de vente est proposé et dans quels marchés. Si vous définissez un prix de vente pour un marché dans lequel votre application n’est pas disponible, cela n’a pas pour effet de la rendre disponible sur ce marché.
4. Choisissez l’une des options suivantes pour spécifier le type de remise :
   - **Prix** : utilisez cette option pour sélectionner un niveau de prix inférieur auquel votre application sera proposée. Vous pouvez modifier la liste déroulante devise pour sélectionner le prix dans la devise de votre choix. (Le prix sera converti au niveau correspondant pour chaque devise. Pour plus d’informations, consultez [tarification](set-app-pricing-and-availability.md).)
   - **Pourcentage** : utilisez cette option pour sélectionner le pourcentage d’une remise qui sera appliquée à votre application. Le même pourcentage de remise est utilisé pour toutes les devises.
5. Dans la ligne **proposé à** , choisissez l’une des options disponibles, notamment :
   - **Tout le monde** : la vente sera proposée à tous les clients.
   - **Propriétaires de** : la vente sera proposée aux clients qui possèdent déjà l’une de vos applications. Vous pouvez sélectionner l’une des applications publiées dans la liste déroulante qui s’affiche. Vous devez disposer d’une ou de plusieurs applications publiées pour que cette option soit disponible.

  > [!IMPORTANT]
  > Si vous sélectionnez **propriétaires de** , la vente sera visible uniquement pour les clients sur Windows 10, version 1607 ou ultérieure.

   - **Groupe d’utilisateurs connu** : la vente sera proposée aux personnes dans le [groupe d’utilisateurs connu](create-known-user-groups.md) que vous sélectionnez. Vous devez avoir déjà créé le groupe d’utilisateurs connu pour que cette option soit disponible.
   - **Segment** : la vente sera proposée aux personnes du segment de client que vous sélectionnez. Vous pouvez utiliser un  [segment que vous avez déjà créé](create-customer-segments.md) ici. Vous pouvez également choisir pour la **première fois** des concessions pour offrir la vente uniquement aux clients qui n’ont jamais acheté quoi que ce soit dans le magasin. Nous proposons ici ce segment, car nous avons constaté qu’après l’achat d’un client, il continue de faire des achats supplémentaires, ce qui peut être un excellent groupe pour inciter au tarif de la vente.
6. Définissez les dates et heures de début et de fin de la période de vente à prix réduit. Choisissez l’une des options de fuseau horaire suivantes :
   - **UTC** : l’heure que vous sélectionnez sera l’heure UTC (Universal Coordinated Time), afin que la vente se produise en même temps, partout.
   - **Local** : l’heure que vous sélectionnez sera utilisée dans chaque fuseau horaire associé à un marché. (Notez que pour les marchés qui incluent plus d’un fuseau horaire, un seul fuseau horaire de ce marché sera utilisé. Pour le États-Unis, le fuseau horaire est utilisé.)
7. Pour planifier une vente supplémentaire, sélectionnez **nouvelle vente** . Dans le cas contraire, sélectionnez **Enregistrer** en bas de la **page tarification et disponibilité** , puis sélectionnez **Envoyer au Store** à partir de la page présentation de l’envoi.

> [!NOTE]
> Il est possible de sélectionner un niveau de prix supérieur au prix de base de votre application. Toutefois, le prix de vente s’affiche aux clients uniquement s’il est inférieur au prix normal de l’application sur ce marché.
>
> La sélection d’un prix supérieur au prix de base de votre application peut être judicieuse pour votre vente si vous avez déjà défini des prix personnalisés pour certains marchés, qui sont supérieurs au prix de base de votre application, et souhaitez réduire temporairement le prix sur ces marchés (tout en conservant un prix de vente supérieur au prix de base de l’application). Si vos sélections ont pour effet d’augmenter le prix de l’application sur un certain marché, nous ne reflétons pas ce prix (supérieur) aux clients de ce marché, qui continuent à voir l’application proposée à son prix antérieur (inférieur). Nous montrons également aux clients le prix le plus bas disponible si vous planifiez des ventes à prix réduit qui se chevauchent avec des prix différents.

## <a name="changing-or-canceling-a-scheduled-sale"></a>Modification ou annulation d’une vente à prix réduit planifiée

Pour réviser ou annuler une vente à prix réduit que vous avez précédemment planifiée pour une application ou un module complémentaire, vous devez créer une nouvelle soumission et la soumettre au Store.

**Pour modifier une vente planifiée**

1.  Dans la page **tarification et disponibilité** d’une soumission d’application ou de module complémentaire en cours, accédez à la section sur la tarification de la **vente** .
2.  Recherchez la vente que vous souhaitez mettre à jour, puis apportez vos modifications.
3.  Cliquez sur **Enregistrer** en bas de la page **Tarification et disponibilité** , puis cliquez sur **Envoyer au Store** dans l’aperçu de la soumission.

Une fois que votre envoi passe par le processus de certification, les modifications prennent effet.

> [!IMPORTANT]
> Si une vente a déjà démarré, vous ne pourrez pas modifier la date de début. Si vous pouvez modifier la date de fin, nous vous recommandons de ne pas modifier une vente pour qu’elle se termine avant la date de fin d’origine. Il peut être frustrant pour vos clients potentiels si vous terminez une vente avant la date de publication initiale (puisque les clients voient la date de fin planifiée lors de l’affichage de la liste des magasins de votre application).

 **Pour annuler une vente qui n’a pas encore commencé**

1.  Sur la page **Tarification et disponibilité** d’une soumission d’application ou de module complémentaire en cours, accédez à la section **Prix de vente** .
2.  Recherchez la vente que vous souhaitez annuler, puis cliquez sur **supprimer** .
3.  Cliquez sur **Enregistrer** en bas de la page **Tarification et disponibilité** , puis cliquez sur **Envoyer au Store** dans l’aperçu de la soumission. Tant que la vente n’a pas commencé avant la fin du processus de certification, la vente supprimée ne s’exécutera pas du tout.




