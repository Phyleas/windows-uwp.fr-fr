---
description: Les vignettes secondaires permettent aux utilisateurs d’épingler du contenu spécifique et des liens détaillés à partir de votre application dans leur menu Démarrer, ce qui facilite l’accès au contenu au sein de votre application.
title: Vignettes secondaires
label: Secondary tiles
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: vignettes Windows 10, UWP et secondaires
ms.localizationpriority: medium
ms.openlocfilehash: 066a6dcb3683e2e55f7452b1f09bb834157aee62
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034502"
---
# <a name="secondary-tiles"></a>Vignettes secondaires


Les vignettes secondaires permettent aux utilisateurs d’épingler du contenu spécifique et des liens détaillés à partir de votre application dans leur menu Démarrer, ce qui facilite l’accès au contenu au sein de votre application.

![Capture d’écran des vignettes secondaires](images/secondarytiles.png)

Par exemple, les utilisateurs peuvent épingler la météo pour de nombreux emplacements spécifiques dans leur menu Démarrer, ce qui fournit (1) des informations plus faciles à visualiser sur la météo actuelle grâce aux vignettes dynamiques et (2) un point d’entrée rapide à la météo de la ville spécifique. Les utilisateurs peuvent également épingler des actions spécifiques, des articles d’actualité et d’autres éléments importants pour eux.

En ajoutant des vignettes secondaires à votre application, vous aidez l’utilisateur à effectuer une nouvelle tentative rapidement et efficacement avec votre application, en l’encourageant à revenir plus souvent grâce à la facilité d’accès fournie par les vignettes secondaires.

**Seuls les utilisateurs peuvent épingler une vignette secondaire ; les applications ne peuvent pas épingler des vignettes secondaires par programmation sans l’approbation** de l’utilisateur. L’utilisateur doit cliquer explicitement sur un bouton « épingler » dans votre application, puis utiliser l’API pour demander la création d’une vignette secondaire, puis le système affiche une boîte de dialogue demandant à l’utilisateur de confirmer si la vignette doit être épinglée.

## <a name="quick-links"></a>Liens rapides

| Article | Description |
| --- | --- |
| [Aide sur les vignettes secondaires](secondary-tiles-guidance.md) | En savoir quand et où vous devez utiliser des vignettes secondaires. |
| [Épingler les vignettes secondaires](secondary-tiles-pinning.md) | Découvrez comment épingler une vignette secondaire. |
| [Épingler à partir d’applications de bureau](secondary-tiles-desktop-pinning.md) | Les applications de bureau peuvent épingler des vignettes secondaires grâce au pont de bureau. |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>Vignettes secondaires par rapport aux vignettes principales

Les vignettes secondaires sont associées à une seule application parente. Elles sont épinglées au menu Démarrer pour fournir à un utilisateur un moyen cohérent et efficace de lancer directement dans une zone fréquemment utilisée de l’application parente. Il peut s’agir d’une sous-section générale de l’application parente qui contient du contenu fréquemment mis à jour ou d’un lien profond vers une zone spécifique de l’application.

Voici quelques exemples de scénarios de vignette secondaire :

* Mises à jour météorologiques pour une ville spécifique dans une application météorologique
* Résumé des événements à venir dans une application de calendrier
* État et mises à jour à partir d’un contact important dans une application sociale
* Flux spécifiques dans un lecteur RSS
* Une sélection musicale
* Un blog

Tout contenu fréquemment modifié qu’un utilisateur souhaite surveiller est un bon candidat pour une vignette secondaire. Une fois que la vignette secondaire est épinglée, les utilisateurs peuvent recevoir des mises à jour en un clin d’œil via la vignette et l’utiliser pour lancer directement dans l’application parente.

Les vignettes secondaires sont similaires aux vignettes principales de plusieurs façons :

* Ils utilisent des notifications par vignette pour afficher du contenu riche.
* Ils doivent inclure un logo de pixel 150 x 150 pour le contenu de la mosaïque par défaut.
* Ils peuvent éventuellement inclure les autres tailles de logo pour permettre des tailles de vignettes plus grandes.
* Ils peuvent afficher des notifications et des badges.
* Ils peuvent être réorganisés dans le menu Démarrer.
* Elles sont automatiquement supprimées lors de la désinstallation de l’application.
* Leurs libellés de badge et de statut détaillé de verrouillage peuvent être affichés sur Lock.

Toutefois, les vignettes secondaires diffèrent des vignettes principales d’une manière visible :

* Les utilisateurs peuvent supprimer leurs vignettes secondaires à tout moment sans supprimer l’application parente.
* Les vignettes secondaires peuvent être créées au moment de l’exécution. Les vignettes d’application ne peuvent être créées qu’au cours de l’installation.
* Un lanceur invite l’utilisateur à confirmer l’ajout d’une vignette secondaire.
* Ils ne peuvent pas être sélectionnés par programmation pour l’écran de verrouillage par le biais d’une demande adressée à l’utilisateur. L’utilisateur doit ajouter manuellement la vignette secondaire via la page Personnaliser dans les paramètres du PC.

Pour envoyer des notifications, des méthodes spécifiques sont fournies pour les mises à jour de vignettes et de badges et les canaux de notification push utilisés avec les vignettes secondaires. Ces versions sont parallèles à celles utilisées avec les vignettes principales. Par exemple, CreateBadgeUpdaterForApplication et CreateBadgeUpdaterForSecondaryTile.


## <a name="guidance-on-secondary-tiles"></a>Aide sur les vignettes secondaires
Pour savoir quand et comment utiliser des vignettes secondaires, ainsi que d’autres instructions d’utilisation, consultez les [conseils sur les vignettes secondaires](secondary-tiles-guidance.md) .


## <a name="pinning-secondary-tiles"></a>Épingler des vignettes secondaires
Pour savoir comment épingler des vignettes secondaires, consultez [Épingler des vignettes secondaires](secondary-tiles-pinning.md).


## <a name="desktop-applications-and-secondary-tiles"></a>Applications de bureau et vignettes secondaires
Pour savoir comment utiliser des vignettes secondaires à partir de votre application de bureau via le pont Desktop Bridge, consultez [Épingler des vignettes secondaires à partir d’applications de bureau](secondary-tiles-desktop-pinning.md).
