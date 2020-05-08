---
Description: Notifications Visualizer est une nouvelle application Windows du Store qui permet aux développeurs de concevoir des vignettes dynamiques adaptatives pour Windows 10.
title: Notifications Visualizer
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c8d355570ef7002d1424457bf29f8161680f2c77
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971034"
---
# <a name="notifications-visualizer"></a>Notifications Visualizer

 


Le visualiseur de notifications est une nouvelle application d’application Windows [dans le Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) qui aide les développeurs à concevoir des vignettes dynamiques et des notifications de Toast interactives pour Windows 10.


## <a name="overview"></a>Vue d’ensemble

Le visualiseur de notifications fournit des aperçus visuels instantanés de vos notifications de vignettes et de toast à mesure que vous modifiez la charge utile XML, à l’instar de l’éditeur XAML ou de l’affichage de conception de Visual Studio. L’application recherche également des erreurs, ce qui vous permet de créer une charge utile ou une charge utile de notification Toast valide.

Cette capture d’écran à partir de l’application montre la charge utile XML et la façon dont les tailles de vignette apparaissent sur un appareil sélectionné :

![Capture d’écran de l’éditeur d’application Notifications Visualizer avec le code et les vignettes](images/notif-visualizer-001.png)

 

Avec le visualiseur de notifications, vous pouvez créer et tester des charges utiles de vignette et de Toast adaptatives sans avoir à modifier et déployer votre propre application. Une fois que vous avez créé une charge utile avec des résultats visuels idéaux, vous pouvez l’intégrer à votre application. Consultez [Envoyer une notification de vignette locale](sending-a-local-tile-notification.md) et [Envoyer un toast local](send-local-toast.md) pour en savoir plus.

**Remarque :**    la simulation du visualiseur de notifications du menu Démarrer de Windows et les notifications Toast ne sont pas toujours complètement précises et ne prend pas en charge certaines propriétés de charge utile avancées. Lorsque vous avez la vignette ou le Toast souhaité, testez-le en épinglant la vignette ou en dépilant le toast pour vérifier qu’il s’affiche comme vous le souhaitez.

 

## <a name="features"></a>Fonctionnalités

Le visualiseur de notifications est fourni avec un certain nombre d’exemples de charge utile pour illustrer les possibilités offertes par les vignettes dynamiques adaptatives et les toasts interactifs pour vous aider à commencer. Vous pouvez tester les différentes options de texte, les gorupes/sous-groupes, les images d’arrière-plan, et vous pouvoir voir de quelle façon les vignettes s’adaptent aux différents écrans et appareils. Une fois les modifications effectuées, vous pouvez enregistrer votre charge utile mise à jour dans un fichier pour l’utiliser ultérieurement.

L’éditeur fournit des avertissements et des erreurs en temps réel. Par exemple, si votre charge utile est supérieure à 5 Ko (limite de plateforme), le visualiseur de notifications vous avertit que la charge utile dépasse cette limite. Vous êtes averti en cas de noms ou de valeurs d’attributs incorrects, ce qui vous permet de déboguer les problèmes visuels.

Vous pouvez contrôler les propriétés des vignettes, telles que le nom d’affichage, la couleur, les logos, ShowName et la valeur du badge. Ces options vous aident à comprendre instantanément de quelle façon les propriétés des vignettes et les charges utiles de notification des vignettes interagissent, et quels sont les résultats produits.

Cette capture d’écran de l’application montre l’éditeur de vignettes :

![Capture d’écran de l’éditeur Notifications Visualizer avec les vignettes](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir Notifications Visualizer dans le Windows Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Créer des vignettes adaptatives](create-adaptive-tiles.md)
* [Toasts interactifs](adaptive-interactive-toasts.md)
