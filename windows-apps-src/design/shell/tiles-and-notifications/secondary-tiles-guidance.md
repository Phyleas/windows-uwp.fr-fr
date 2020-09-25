---
Description: Découvrez quand et où vous devez utiliser des vignettes secondaires dans votre application Windows.
title: Conseils de conception des vignettes secondaires
label: Secondary tiles
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, vignettes secondaires, conseils, recommandations, meilleures pratiques
ms.localizationpriority: medium
ms.openlocfilehash: 5414c9d8437ee77e2a4a584dea26f7bf1fadef4a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220232"
---
# <a name="secondary-tile-guidance"></a>Aide sur les vignettes secondaires


Une vignette secondaire offre une méthode cohérente et efficace pour permettre aux utilisateurs d’accéder directement à des zones spécifiques d’une application à partir du menu Démarrer. Bien qu’un utilisateur décide s’il faut ou non « épingler » une vignette secondaire dans le menu Démarrer, les zones regroupement dans une application sont déterminées par le développeur. Pour obtenir un résumé plus détaillé, consultez [vue d’ensemble des vignettes secondaires](secondary-tiles.md). Tenez compte des instructions suivantes lorsque vous activez les vignettes secondaires et concevez l’interface utilisateur associée dans votre application.

> [!NOTE]
> Seuls les utilisateurs peuvent épingler une vignette secondaire dans le menu Démarrer. les applications ne peuvent pas épingler par programmation des vignettes secondaires. Les utilisateurs contrôlent également la suppression des vignettes et peuvent supprimer une vignette secondaire du menu Démarrer ou de l’application parente.


## <a name="recommendations"></a>Recommandations

Tenez compte des recommandations suivantes lors de l’activation des vignettes secondaires dans votre application :

* Lorsque le contenu est regroupement, la barre de l’application doit contenir un bouton « épingler au début » pour créer une vignette secondaire pour l’utilisateur.
* Quand l’utilisateur clique sur « épingler à démarrer », vous devez immédiatement appeler l’API à partir du thread d’interface utilisateur pour [épingler la vignette secondaire](secondary-tiles-pinning.md).
* Si le contenu dans le focus est déjà épinglé, remplacez le bouton « épingler au début » dans la barre de l’application par un bouton « détacher du début ». Le bouton « détacher du début » doit supprimer la vignette secondaire existante.
* Si le contenu n’est pas regroupement, n’affichez pas le bouton « épingler au début » (ou affichez le bouton « épingler au début » désactivé).
* Utilisez les glyphes fournis par le système pour les boutons « épingler au démarrage » et « détacher du début » (voir le code confidentiel et détacher les membres dans Windows. UI. Xaml. Controls. Symbol ou WinJS. UI. AppBarIcon).
* Utilisez le texte du bouton standard : « épingler au début » et « détacher du début ». Vous devrez remplacer le texte par défaut lorsque vous utilisez le code confidentiel fourni par le système et détachez les glyphes.
* N’utilisez pas de vignette secondaire comme bouton de commande virtuel pour interagir avec l’application parente, par exemple une vignette « passer à la piste suivante ».


## <a name="additional-usage-guidance-for-devs"></a>Conseils d’utilisation supplémentaires pour les développeurs

* Quand une application est lancée, elle doit toujours énumérer ses vignettes secondaires, au cas où des ajouts ou des suppressions n’étaient pas pris en compte. Lorsqu’une vignette secondaire est supprimée via la barre d’application de l’écran de démarrage, Windows supprime simplement la vignette. L’application elle-même est chargée de libérer toutes les ressources qui ont été utilisées par la vignette secondaire. Lorsque des vignettes secondaires sont copiées dans le Cloud, les notifications de vignette ou de badge actuelles sur la vignette secondaire, les notifications planifiées, les canaux de notification push et les URI utilisés avec les notifications périodiques ne sont pas copiées avec la vignette secondaire et doivent être de nouveau configurées.
* Une application doit utiliser des ID uniques explicites, recréés pour les vignettes secondaires. L’utilisation d’ID de vignette secondaires prévisibles qui sont significatives pour une application permet à l’application de comprendre ce qu’il faut faire avec ces vignettes lorsqu’elles sont affichées dans une nouvelle installation sur un nouvel ordinateur.
  * Lors de l’exécution, l’application peut interroger l’existence d’une vignette spécifique.
  * La plateforme de vignette secondaire peut être invitée à retourner l’ensemble de toutes les vignettes secondaires appartenant à une application spécifique. L’utilisation d’ID uniques explicites pour ces vignettes peut aider l’application à examiner l’ensemble des vignettes secondaires et à effectuer les actions appropriées. Par exemple, pour une application de réseau social, les ID peuvent identifier des contacts individuels pour lesquels des vignettes ont été créées.
* Les vignettes secondaires, comme toutes les vignettes de l’écran d’accueil, sont des prises dynamiques qui peuvent être mises à jour fréquemment avec un nouveau contenu. Les vignettes secondaires peuvent exposer des notifications et des mises à jour en utilisant les mêmes mécanismes que n’importe quelle autre vignette. Pour en savoir plus, consultez [choisir une méthode de remise des notifications](choosing-a-notification-delivery-method.md) .


## <a name="related"></a>Rubriques connexes

* [Vue d’ensemble des vignettes secondaires](secondary-tiles.md)
* [Épingler les vignettes secondaires](secondary-tiles-pinning.md)
* [Ressources de vignette](../../style/app-icons-and-logos.md)
* [Documentation sur le contenu de la vignette](create-adaptive-tiles.md)
* [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md)
