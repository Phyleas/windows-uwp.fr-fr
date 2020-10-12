---
title: Utilitaire de résolution de problèmes de compatibilité des programmes sur ARM
description: Aide pour l’ajustement des paramètres de compatibilité si votre application ne fonctionne pas correctement sur ARM
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, Always connected, résolution des problèmes de compatibilité, Windows on ARM
ms.localizationpriority: medium
ms.openlocfilehash: 24ae6e7c12fde1dfbb9e5395b3fa3aa4901adb3d
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933020"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>Utilitaire de résolution de problèmes de compatibilité des programmes sur ARM
L’émulation pour la prise en charge des applications x86 est une nouvelle fonctionnalité créée pour Windows 10 sur ARM64. Parfois, l’émulation effectue des optimisations qui n’entraînent pas la meilleure expérience. Vous pouvez utiliser l’utilitaire de résolution des problèmes de compatibilité des programmes pour activer/désactiver les paramètres d’émulation de votre application x86, ce qui réduit les optimisations par défaut et peut potentiellement améliorer la compatibilité.

## <a name="start-the-program-compatibility-troubleshooter"></a>Démarrer l’utilitaire de résolution des problèmes de compatibilité des programmes
Vous démarrez l' [utilitaire de résolution des problèmes de compatibilité des programmes](https://support.microsoft.com/help/15078/windows-make-older-programs-compatible) manuellement de la même façon sur un PC Windows 10 : cliquez avec le bouton droit sur un fichier exécutable (. exe) et sélectionnez **résoudre les problèmes de compatibilité**. Cet écran s’affiche.

![Capture d’écran des options de résolution des problèmes de compatibilité.](images/arm/Capture4.png)

Si vous cliquez sur **dépanner le programme** , les options suivantes s’affichent.

![Capture d’écran des options quels sont les problèmes que vous pouvez remarquer.](images/arm/Capture5.png)

Toutes les options activent les paramètres applicables et appliqués sur tous les ordinateurs de bureau Windows 10. En outre, les première, deuxième et quatrième options appliquent le [cache de désactivation](#disable-app-cache) de l’application et désactivent les paramètres d’émulation [en mode d’exécution hybride](#disable-hybrid-exec-mode) .

## <a name="toggling-emulation-settings"></a>Basculement des paramètres d’émulation
> [!WARNING]
> La modification des paramètres d’émulation peut entraîner une panne inattendue de votre application ou son lancement.

Vous pouvez activer ou désactiver les paramètres d’émulation en cliquant avec le bouton droit sur le fichier exécutable et en sélectionnant **Propriétés**.

Sur ARM, une section intitulée **Windows 10 on Arm** sera disponible sous l’onglet **compatibilité** . Cliquez sur **modifier les paramètres d’émulation** pour lancer une deuxième fenêtre comme ici.

![Capture d’écran modifier les paramètres d’émulation](images/arm/Capture.png)

Cette fenêtre offre deux façons de modifier les paramètres d’émulation. Vous pouvez sélectionner un groupe prédéfini de paramètres d’émulation, ou vous pouvez cliquer sur l’option **utiliser les paramètres avancés** pour activer le choix des paramètres individuels.

Les paramètres d’émulation groupés réduisent les optimisations de performances en faveur de la qualité. Voici quelques paramètres groupés que vous pouvez sélectionner.

![Modifier les paramètres d’émulation screenshot2](images/arm/Capture2.png)

Sélectionnez **utiliser les paramètres avancés** pour choisir des paramètres individuels comme décrit dans ce tableau.

| Paramètre d’émulation | Résultats |
| ----------------- | ----------- |
| <p id="disable-app-cache">Désactiver le cache d’application</p> | Le système d’exploitation mettra en cache des blocs de code compilés pour réduire la surcharge d’émulation lors des exécutions suivantes. Ce paramètre requiert que l’émulateur RECOMPILE tout le code de l’application au moment de l’exécution. |
| <p id="disable-hybrid-exec-mode">Désactiver le mode d’exécution hybride</p> | L’exécutable portable hybride compilé (CHPE), les fichiers binaires sont des binaires compatibles x86 qui incluent du code ARM64 natif pour améliorer les performances, mais qui ne sont peut-être pas compatibles avec certaines applications. Ce paramètre force l’utilisation de fichiers binaires x86 uniquement. |
| Prise en charge stricte du code de modification automatique | Activez cette valeur pour vous assurer que tout code à modification automatique est correctement pris en charge dans l’émulation. Les scénarios de code à modification automatique les plus courants sont couverts par le comportement de l’émulateur par défaut. L’activation de cette option réduit considérablement les performances du code à modification automatique pendant l’exécution. |
| Désactiver l’optimisation des performances de la page RWX | Cette optimisation améliore les performances du code sur les pages lisibles, accessibles en écriture et exécutables (RWX), mais peut être incompatible avec certaines applications. |

Vous pouvez également sélectionner des paramètres multicœurs, comme illustré ici.

![Capture d’écran des paramètres multicœurs](images/arm/Capture3.png)

Ces paramètres modifient le nombre de barrières de mémoire utilisées pour synchroniser les accès à la mémoire entre les cœurs dans les applications lors de l’émulation. **Fast** est le mode par défaut, mais les options **strict** et **strict** vous permettront d’augmenter le nombre de barrières. Cela ralentit l’application, mais réduit le risque d’erreurs d’application. L’option **Single-Core** supprime toutes les barrières, mais force tous les threads d’application à s’exécuter sur un seul cœur.

Si la modification d’un paramètre spécifique résout votre problème, envoyez *woafeedback@microsoft.com* -nous un e-mail avec les détails pour pouvoir incorporer vos commentaires.