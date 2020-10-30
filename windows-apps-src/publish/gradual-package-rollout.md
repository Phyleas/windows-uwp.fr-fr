---
description: Lorsque vous publiez une mise à jour d’une soumission, vous pouvez choisir de déployer progressivement les packages de mise à jour pour un pourcentage des clients de votre application sur Windows 10.
title: Lancement de package progressif
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 65d578a6-4e26-484c-90af-b2cd916f3634
ms.localizationpriority: medium
ms.openlocfilehash: 17e9ecbf7744e1713ac81ec4bbc58848e44046eb
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034762"
---
# <a name="gradual-package-rollout"></a>Lancement de package progressif

Lorsque vous publiez une mise à jour d’une soumission, vous pouvez choisir de déployer progressivement les packages mis à jour sur un pourcentage des clients de votre application sur Windows 10 (y compris la Xbox). Cela vous permet de surveiller les commentaires et les données d’analyse des packages spécifiques et de vérifier l’adéquation de votre mise à jour avant de la déployer plus largement. Vous pouvez augmenter le pourcentage (ou arrêter la mise à jour) à tout moment sans avoir à créer une nouvelle soumission. 

> [!IMPORTANT]
> Vos sélections de déploiement s’appliquent à tous vos packages, mais s’appliquent uniquement à vos clients exécutant des versions de système d’exploitation qui prennent en charge les vols de packages (Windows. Desktop Build 10586 ou version ultérieure). Windows. mobile Build 10586,63 ou version ultérieure et Xbox), y compris les clients qui obtiennent l’application via une [licence gérée par le magasin (en ligne)](organizational-licensing.md) par le biais [de Microsoft Store for Business](https://businessstore.microsoft.com/store) ou [Microsoft Store for Education](https://educationstore.microsoft.com/store). Lorsque vous utilisez le déploiement progressif de packages, les clients utilisant des versions antérieures du système d’exploitation n’obtiennent pas les packages de la dernière soumission tant que vous n’avez pas finalisé le déploiement de packages comme décrit ci-dessous.

Notez que les informations de description du Windows Store que vous avez entrées lors de votre dernière soumission sont visibles de tous les clients. Les paramètres de déploiement s’appliquent uniquement aux packages reçus par les clients. Cela concerne les nouvelles acquisitions et les mises à jour reçues par les clients existants.

> [!TIP]
> Le déploiement de package distribue les packages à une sélection aléatoire de clients dans les pourcentages que vous spécifiez. Pour distribuer des packages spécifiques à des clients sélectionnés par vous, vous pouvez utiliser des versions d’évaluation de package. Vous pouvez également combiner le lancement avec vos versions d’évaluation de package si vous souhaitez distribuer progressivement une mise à jour à l’un de vos groupes de versions d’évaluation.


## <a name="setting-the-rollout-percentage"></a>Définition du pourcentage de déploiement

Vous pouvez choisir de déployer votre mise à jour sur la page **Packages** d’une soumission mise à jour. Pour ce faire, activez la case à cocher **Déployer progressivement la mise à jour après la publication de cette soumission (pour les clients Windows 10 uniquement)** . Ensuite, entrez le pourcentage de clients qui devront obtenir la mise à jour lors de la première publication de la soumission. Par exemple, vous pouvez entrer 5 si vous souhaitez commencer par déployer la mise à jour pour un faible pourcentage de clients de votre application.

Cliquez sur **Mettre à jour** pour enregistrer vos choix. Une fois que votre application a terminé le processus de certification, les packages sont distribués aux clients conformément au pourcentage que vous avez spécifié. Cela concerne les nouvelles acquisitions et les mises à jour effectuées par les clients existants.


## <a name="adjusting-the-rollout-after-the-submission-is-published"></a>Réglage du déploiement après la publication de la soumission

Pour ajuster le déploiement après la publication de la soumission, accédez à la page de vue d’ensemble de votre application. Vous pouvez faire glisser le sélecteur pour modifier le pourcentage de clients qui obtiendront les packages de la dernière soumission. Cliquez sur **Mettre à jour** pour enregistrer vos choix. Les packages commencent ensuite à être distribués aux clients conformément au pourcentage que vous avez spécifié. Cela concerne les nouvelles acquisitions et les mises à jour effectuées par des clients existants.


## <a name="completing-the-rollout"></a>Fin du déploiement

Avant de pouvoir créer une nouvelle soumission, vous devez mettre un terme au déploiement de packages. Vous pouvez **finaliser** le déploiement et distribuer les derniers packages à tous vos clients, ou **arrêter** le déploiement pour mettre un terme à la distribution des derniers packages.

Si vous avez confiance dans la mise à jour et que vous voulez la rendre disponible pour tous vos clients, cliquez sur **Finaliser le déploiement de package** pour distribuer les derniers packages à tous vos clients.

> [!TIP]
> La modification du pourcentage de déploiement sur 100% ne garantit pas que tous vos clients obtiendront les packages à partir des dernières soumissions, car certains clients peuvent se trouver sur des versions de système d’exploitation qui ne prennent pas en charge le déploiement. Vous devez finaliser le déploiement pour arrêter la distribution des packages plus anciens et mettre à jour les clients existants avec les nouvelles versions.

Si vous constatez des problèmes avec la mise à jour et que vous ne souhaitez plus la distribuer, vous pouvez cliquer sur **Arrêter le déploiement de l’application** pour arrêter la distribution des packages de la dernière soumission. Une fois que vous arrêtez un déploiement de packages, ces packages ne sont plus distribués aux clients. Seuls les packages de la soumission précédente sont utilisés pour les nouveaux clients ou ceux qui effectuent une mise à jour. Toutefois, les clients qui disposaient déjà des packages les plus récents conservent ces packages. La version précédente ne sera pas rétablie pour eux. Pour fournir une mise à jour à ces clients, vous devez créer une nouvelle soumission avec les packages que vous souhaitez qu’ils obtiennent. Notez que si vous utilisez un déploiement progressif lors de votre soumission suivante, les clients qui disposaient du package que vous avez interrompu pourront s’ils le souhaitent obtenir la mise à jour comme ils pouvaient obtenir le package interrompu. Le nouveau déploiement se situera entre votre dernière soumission finalisée et votre soumission la plus récente. Une fois que vous interrompez un déploiement de packages, ces packages ne sont plus distribués à aucun client.
