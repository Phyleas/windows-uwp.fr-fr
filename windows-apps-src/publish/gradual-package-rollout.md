---
author: JnHs
Description: "Si votre application utilise AdMediatorControl ou AdControl pour afficher des bannières publicitaires, vous pourriez augmenter votre taux de remplissage et vos revenus publicitaires en présentant des annonces des affiliés Microsoft dans votre application."
title: "Déploiement progressif de packages"
translationtype: Human Translation
ms.sourcegitcommit: ac9eed95edba99cdba914ff21b25383f35a20012
ms.openlocfilehash: 3ec642ef0a21d06b20cabb12a47d2abcfd19fe59

---

# Déploiement progressif de packages

Lorsque vous publiez une mise à jour d’une soumission, vous pouvez choisir de déployer progressivement les packages de mise à jour pour un pourcentage des clients de votre application sur Windows10. Cela vous permet de surveiller les commentaires et les données d’analyse des packages spécifiques et de vérifier l’adéquation de votre mise à jour avant de la déployer plus largement. Vous pouvez augmenter le pourcentage (ou arrêter la mise à jour) à tout moment sans avoir à créer une nouvelle soumission. 

> **Important** Vos choix de déploiement s’appliquent à tous vos packages, mais uniquement pour les clients qui exécutent des versions de système d’exploitation prenant en charge des versions d’évaluation de package (Windows.Desktop build10586 ou version ultérieure, Windows.Mobile build10586.63 ou version ultérieure et Xbox), y compris les clients qui obtiennent l’application via le [service de gestion de licences (en ligne) du Windows Store](organizational-licensing.md) dans [Windows Store pour Entreprises](https://www.microsoft.com/business-store). Lorsque vous utilisez le déploiement progressif de packages, les clients utilisant des versions antérieures du système d’exploitation n’obtiennent pas les packages de la dernière soumission tant que vous n’avez pas finalisé le déploiement de packages comme décrit ci-dessous.

Notez que les informations de description du Windows Store que vous avez entrées lors de votre dernière soumission sont visibles de tous les clients. Les paramètres de déploiement s’appliquent uniquement aux packages reçus par les clients. Cela concerne les nouvelles acquisitions et les mises à jour reçues par les clients existants.

> **Conseil** Le déploiement de packages distribue les packages à un ensemble aléatoire de clients conformément au pourcentage que vous avez spécifié. Pour distribuer des packages spécifiques à des clients sélectionnés par vous, vous pouvez utiliser des versions d’évaluation de package.  Vous pouvez également combiner le lancement avec vos versions d’évaluation de package si vous souhaitez distribuer progressivement une mise à jour à l’un de vos groupes de versions d’évaluation.

## Définition du pourcentage de déploiement

Vous pouvez choisir de déployer votre mise à jour sur la page **Packages** d’une soumission mise à jour. Pour ce faire, activez la case à cocher **Déployer progressivement la mise à jour après la publication de cette soumission (pour les clients Windows10 uniquement)**. Ensuite, entrez le pourcentage de clients qui devront obtenir la mise à jour lors de la première publication de la soumission. Par exemple, vous pouvez entrer5 si vous souhaitez commencer par déployer la mise à jour pour un faible pourcentage de clients de votre application.

Cliquez sur **Mettre à jour** pour enregistrer vos choix. Une fois que votre application a terminé le processus de certification, les packages sont distribués aux clients conformément au pourcentage que vous avez spécifié. Cela concerne les nouvelles acquisitions et les mises à jour effectuées par les clients existants.

## Réglage du déploiement après la publication de la soumission

Pour ajuster le déploiement après la publication de la soumission, accédez à la page de vue d’ensemble de votre application. Vous pouvez faire glisser le sélecteur pour modifier le pourcentage de clients qui obtiendront les packages de la dernière soumission. Cliquez sur **Mettre à jour** pour enregistrer vos choix. Les packages commencent ensuite à être distribués aux clients conformément au pourcentage que vous avez spécifié. Cela concerne les nouvelles acquisitions et les mises à jour effectuées par des clients existants.

## Fin du déploiement

Avant de pouvoir créer une nouvelle soumission, vous devez mettre un terme au déploiement de packages. Vous pouvez **finaliser** le déploiement et distribuer les derniers packages à tous vos clients, ou **arrêter** le déploiement pour mettre un terme à la distribution des derniers packages.

Si vous avez confiance dans la mise à jour et que vous voulez la rendre disponible pour tous vos clients, cliquez sur **Finaliser le déploiement de package** pour distribuer les derniers packages à tous vos clients.

> **Conseil** Le fait de redéfinir le pourcentage de déploiement sur 100% ne garantit pas que tous vos clients obtiendront les packages des dernières soumissions, car certains clients utilisent sans doute des versions de système d’exploitation ne prenant pas en charge le déploiement. Vous devez finaliser le déploiement pour arrêter la distribution des packages plus anciens et mettre à jour les clients existants avec les nouvelles versions.

Si vous constatez des problèmes avec la mise à jour et que vous ne souhaitez plus la distribuer, vous pouvez cliquer sur **Arrêter le déploiement de l’application** pour arrêter la distribution des packages de la dernière soumission. Une fois que vous arrêtez un déploiement de packages, ces packages ne sont plus distribués aux clients. Seuls les packages de la soumission précédente sont utilisés pour les nouveaux clients ou ceux qui effectuent une mise à jour. Toutefois, les clients qui disposaient déjà des packages les plus récents conservent ces packages. La version précédente ne sera pas rétablie pour eux. Pour fournir une mise à jour à ces clients, vous devez créer une nouvelle soumission avec les packages que vous souhaitez qu’ils obtiennent. Notez que si vous utilisez un déploiement progressif lors de votre soumission suivante, les clients qui disposaient du package que vous avez interrompu pourront s’ils le souhaitent obtenir la mise à jour comme ils pouvaient obtenir le package interrompu. Le nouveau déploiement se situera entre votre dernière soumission finalisée et votre soumission la plus récente. Une fois que vous interrompez un déploiement de packages, ces packages ne sont plus distribués à aucun client.



<!--HONumber=Aug16_HO5-->

