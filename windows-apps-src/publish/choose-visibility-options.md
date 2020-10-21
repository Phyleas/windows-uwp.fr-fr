---
Description: Définissez des restrictions sur la façon dont votre application peut être découverte et acquise, notamment si les gens peuvent trouver votre application dans le Store ou voir la liste de leur boutique.
title: Choisir les options de visibilité
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, visibilité, public privé, disponible, détectable
ms.localizationpriority: medium
ms.openlocfilehash: 8c78b8c7a84c6bdaedb58055d8b36883c6a61607
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253644"
---
# <a name="choose-visibility-options"></a>Choisir les options de visibilité


La section **visibilité** de la [page tarification et disponibilité](set-app-pricing-and-availability.md) vous permet de définir des restrictions sur la façon dont votre application peut être découverte et acquise. Cela vous donne la possibilité de spécifier si les utilisateurs peuvent trouver votre application dans le Windows Store ou de consulter la liste de leur boutique.

Il existe deux sections distinctes dans la section visibilité : **audience** et **détectabilité**. 

## <a name="audience"></a>Public visé

La section audience vous permet de spécifier si vous souhaitez limiter la visibilité de votre envoi à un public spécifique que vous définissez.


### <a name="public-audience"></a>Public public

Par défaut, le Listing de votre application sera visible par un public **public**. Cela est approprié pour la plupart des soumissions, à moins que vous ne souhaitiez limiter les personnes qui peuvent voir la liste de vos applications à des personnes spécifiques. Vous pouvez également utiliser les options de la section [détectabilité](#discoverability) pour limiter la détectabilité si vous le souhaitez.

> [!IMPORTANT]
> Si vous soumettez un produit pour lequel cette option est **définie sur public public,** vous ne pouvez pas choisir d' **audience privée** dans une soumission ultérieure.


### <a name="private-audience"></a>Audience privée

Si vous souhaitez que la liste de votre application soit visible uniquement pour les personnes sélectionnées que vous spécifiez, choisissez **audience privée**. Avec cette option, l’application ne peut pas être détectée ou disponible pour toute personne autre que des personnes appartenant au (x) groupe (s) que vous spécifiez. Cette option est souvent utilisée pour les [tests bêta](beta-testing-and-targeted-distribution.md), car elle vous permet de distribuer votre application à des testeurs sans que quiconque soit en mesure d’obtenir l’application ou même de voir sa liste de magasins (même si elle a pu taper son URL de liste de magasins).

Lorsque vous choisissez **audience privée**, vous devez spécifier au moins un groupe de personnes qui doivent récupérer votre application. Vous pouvez choisir un groupe d' [utilisateurs connu](create-known-user-groups.md)existant, ou vous pouvez sélectionner **créer un nouveau groupe** pour définir un nouveau groupe. Vous devez entrer les adresses de messagerie associées à la compte Microsoft de chaque personne que vous souhaitez inclure dans le groupe. Pour plus d’informations, consultez [créer des groupes d’utilisateurs connus](create-known-user-groups.md).

Une fois votre envoi publié, les personnes du groupe que vous spécifiez peuvent afficher la liste de l’application et télécharger l’application, à condition qu’elles soient connectées avec l’compte Microsoft associée à l’adresse de messagerie que vous avez entrée et qui exécutent Windows 10, version 1607 ou ultérieure (y compris Xbox One). Toutefois, les personnes qui ne sont pas dans votre audience privée ne pourront pas afficher la liste de l’application ou télécharger l’application, quelle que soit la version du système d’exploitation exécutée. Vous pouvez publier des soumissions mises à jour à l’audience privée, qui seront distribuées aux membres de ces audiences de la même manière qu’une mise à jour d’application normale (mais qui ne seront toujours pas disponibles pour quiconque ne figure pas dans votre public privé, sauf si vous modifiez votre sélection d’audience). 

Si vous envisagez de mettre l’application à la disposition d’un public public à une date et une heure spécifiques, vous pouvez activer la case à cocher **rendre ce produit public** au moment de la création de votre envoi. Entrez la date et l’heure (au format UTC) quand vous souhaitez que le produit soit mis à la disposition du public. Gardez à l’esprit les points suivants :

- La date et l’heure que vous sélectionnez s’appliquent à tous les marchés. Si vous souhaitez personnaliser la planification des versions pour différents marchés, n’utilisez pas cette zone. Au lieu de cela, créez une nouvelle soumission qui change votre paramètre en public **public**, puis utilisez les options de [planification](configure-precise-release-scheduling.md) pour spécifier le minutage de la publication.
- La saisie d’une date pour **rendre ce produit public sur** ne s’applique pas aux Microsoft Store pour les entreprises et/ou Microsoft Store pour l’éducation. Pour nous permettre de proposer votre application à ces clients par le biais d’une licence d’organisation, vous devez créer une nouvelle soumission avec l' **audience publique** sélectionnée (et la [licence organisationnelle](organizational-licensing.md) activée).
- Après la date et l’heure que vous sélectionnez, toutes les soumissions ultérieures utilisent un public **public**.

Si vous ne spécifiez pas de date et d’heure pour mettre votre application à la disposition d’un public public, vous pouvez toujours le faire ultérieurement en créant une soumission et en remplaçant votre paramètre d’audience **privé** **par public public.** Dans ce cas, gardez à l’esprit que votre application peut passer par un processus de certification supplémentaire. vous devez donc être prêt à résoudre les problèmes de certification qui peuvent survenir. 

Voici quelques points importants à prendre en compte lorsque vous choisissez de distribuer votre application à une audience privée :
- Les personnes de votre public privé pourront obtenir l’application à l’aide d’un lien spécifique vers la liste des boutiques de votre application, qui leur demande de se connecter avec leur compte Microsoft pour les afficher. Ce lien est fourni lorsque vous sélectionnez **audience privée**. Vous pouvez également le trouver dans la page [identité](view-app-identity-details.md) de votre application sous **URL si votre application n’est visible que par certaines personnes (nécessite une authentification)**. Veillez à fournir à vos testeurs ce lien, et non l’URL standard de votre liste de magasins.  
- À moins que vous ne choisissiez une option dans la **détectabilité** qui l’empêche, les personnes de votre public privé pourront trouver votre application en effectuant une recherche dans l’application Microsoft Store. Toutefois, la liste Web ne sera pas détectable via la recherche, même pour les personnes de cette audience. 
- Vous ne pouvez pas [configurer les dates de publication dans la section planification](configure-precise-release-scheduling.md) de la **page tarification et disponibilité**, puisque votre application n’est pas disponible pour les clients en dehors de votre audience privée.
- Les autres sélections que vous effectuez s’appliquent aux personnes de cette audience. Par exemple, si vous choisissez un prix autre que **gratuit**, les personnes de votre public privé devront payer ce prix afin d’acquérir l’application. 
- Si vous souhaitez distribuer des packages différents à des personnes de votre audience privée, après votre envoi initial, vous pouvez utiliser des [vols](package-flights.md) de packages pour distribuer différentes mises à jour de packages à des sous-ensembles de votre audience privée. Vous pouvez créer des groupes d’utilisateurs connus pour définir qui doit obtenir un vol de package spécifique.
- Vous pouvez modifier l’appartenance au (x) groupe (s) d’utilisateurs connus dans votre audience privée. Toutefois, gardez à l’esprit que si vous supprimez une personne qui se trouvait dans le groupe et que vous avez précédemment téléchargé votre application, elle sera toujours en mesure d’utiliser l’application, mais elle ne recevra pas les mises à jour que vous fournissez (sauf si vous choisissez un public **public** à une date ultérieure).
- Votre application n’est pas disponible par le biais de la Microsoft Store pour l’entreprise et/ou Microsoft Store pour l’éducation, quels que soient les paramètres de votre licence d’organisation, même pour les personnes de votre public public.
- Alors que le Store s’assure que votre application est uniquement visible et disponible pour les personnes connectées avec un compte Microsoft que vous avez ajouté à votre public privé, nous ne pouvons pas empêcher ces personnes de partager des informations ou des captures d’écran en dehors de votre audience privée. Lorsque la confidentialité est essentielle, veillez à ce que votre audience privée n’inclue que les personnes dont vous faites confiance pour ne pas partager les détails de votre application avec d’autres utilisateurs.
- Veillez à laisser vos testeurs savoir comment vous faire part de leurs commentaires. Vous ne souhaiterez probablement pas qu’ils laissent des commentaires dans le hub de commentaires, car tout autre client peut voir ces commentaires. Pensez à inclure un lien leur permettant d’envoyer des courriers électroniques ou de fournir des commentaires d’une autre façon.
- Toutes les révisions rédigées par des personnes de votre public privé seront disponibles. Toutefois, ces évaluations ne seront pas publiées dans le Listing de votre application, même après le déplacement de votre envoi vers **public public**. Vous pouvez lire les avis écrits par votre public privé en consultant le [rapport](reviews-report.md)d’évaluation, mais vous ne pouvez pas télécharger ces données ou utiliser l' [API Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md) pour accéder par programmation à ces analyses.
- Lorsque vous déplacez une application d’un **public privé** vers un public **public**, la **Date de publication** indiquée dans la liste de la boutique sera la date à laquelle elle a été publiée pour la première fois sur le public public.

## <a name="discoverability"></a>Détectabilité

Les sélections de la section **détectabilité** indiquent comment les clients peuvent découvrir et acquérir votre application. 

> [!IMPORTANT]
> Si vous sélectionnez **audience privée**, vos sélections de **détectabilité** s’appliquent uniquement aux personnes de votre audience privée. Les clients qui ne se trouvent pas dans les groupes que vous avez spécifiés ne pourront pas obtenir l’application ou même afficher sa liste. 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>Rendre ce produit disponible et détectable dans le Store

Il s'agit de l'option par défaut. Laissez cette option sélectionnée si vous souhaitez que votre application soit répertoriée dans le Store pour que les clients se trouvent via le lien direct de l’application et/ou par d’autres méthodes, notamment la recherche, la navigation et l’inclusion dans des listes organisées. 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>Rendre ce produit disponible mais non détectable dans le Windows Store

Lorsque vous sélectionnez cette option, votre application est introuvable dans le magasin par les clients effectuant des recherches ou une navigation ; la seule façon d’accéder à la liste de votre application consiste à utiliser un lien direct. 

> [!TIP]
> Si vous ne souhaitez pas que la liste soit visible par le public, même avec un lien direct, choisissez **audience privée** dans la section **audience** , comme décrit ci-dessus.

Vous devez également choisir l’une des options suivantes pour spécifier la façon dont votre application peut être acquise :


>[!IMPORTANT]
> Chacune de ces options limite les versions de système d’exploitation sur lesquelles les clients peuvent acquérir votre application. Veuillez lire attentivement les descriptions pour vous assurer que vous savez quelles sont les versions de système d’exploitation prises en charge. 

- **Lien direct uniquement : tout client disposant d’un lien direct vers le Listing du produit peut le télécharger, sauf sur Windows 8. x.** Tout client qui obtient la liste de votre application via un lien direct peut le télécharger sur des appareils exécutant Windows 10 ou sur des appareils exécutant Windows Phone 8,1 ou version antérieure (mais pas sur les appareils exécutant Windows 8. x).
- **Arrêter l’acquisition : tout client disposant d’un lien direct peut voir la liste des magasins du produit, mais il ne peut le télécharger que s’il en a déjà détenu le produit ou s’il a un code promotionnel et utilise un appareil Windows 10.** Même si un client dispose d’un lien direct, il ne peut pas télécharger l’application, sauf s’il dispose d’un [code promotionnel](generate-promotional-codes.md) et utilise un appareil Windows 10. Si un client dispose d’un code promotionnel, il peut l’utiliser pour obtenir gratuitement votre application (sur Windows 10 uniquement), même si vous ne l’offrez pas à d’autres clients. Outre l’utilisation d’un code promotionnel, il n’est pas possible pour quiconque d’obtenir votre application.

> [!TIP]
> Si vous souhaitez cesser d’offrir une application à tous les nouveaux clients, vous pouvez sélectionner **rendre l’application indisponible** à partir de la page vue d’ensemble. Une fois que vous avez confirmé que vous souhaitez rendre l’application indisponible, dans quelques heures, elle n’est plus visible dans le Store, et aucun nouveau client ne pourra l’obtenir (à moins qu’il n’ait un [code promotionnel](generate-promotional-codes.md) et qu’il se trouve sur un appareil Windows 10). Cette action remplacera les sélections de **visibilité** dans votre envoi. Pour que l’application soit à nouveau disponible pour les nouveaux clients (selon vos sélections de **visibilité** ), vous pouvez cliquer sur **rendre l’application disponible** à tout moment à partir de la page vue d’ensemble. Pour en savoir plus, consultez l’article [Suppression d’une application du Windows Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).




