---
title: Tailles d’écran et points d’arrêt pour la conception réactive
description: Plutôt que d'optimiser votre interface utilisateur pour les nombreux appareils de l’écosystème Windows 10, nous vous recommandons de concevoir une application pour plusieurs catégories de largeurs principales appelées « points d’arrêt ».
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 37d0ca71adf43891628a02d60d6873e7934d749b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79210175"
---
#  <a name="screen-sizes-and-breakpoints"></a>Tailles d’écran et points d’arrêt

Les applications UWP peuvent s’exécuter sur n’importe quel appareil exécutant Windows 10, y compris les téléphones, tablettes, ordinateurs de bureau, téléviseurs, etc. Avec un nombre considérable d’appareils ciblés et de tailles d’écran dans l’écosystème Windows 10, plutôt que d’optimiser votre interface utilisateur pour chaque appareil, nous vous recommandons de concevoir une application pour plusieurs catégories de largeurs principales (également appelées « points d’arrêt ») : 
- Petite (inférieure à 640 pixels)
- Moyenne (641 à 1 007 pixels)
- Grande (supérieure ou égale à 1008 pixels)

> [!TIP]
> Lors de la conception pour des points d’arrêt spécifiques, pensez à la quantité d’espace à l'écran disponible pour votre application (fenêtre de l’application), et non à la taille d’écran. Lorsque l’application s’exécute en mode plein écran, la fenêtre d’application a la même taille que l’écran, mais lorsque l’application n’est pas en mode plein écran, la fenêtre est plus petite.

## <a name="breakpoints"></a>Points d’arrêt
Ce tableau décrit les différentes classes de taille et les points d’arrêt.

![Points d’arrêt de la conception réactive](images/breakpoints/size-classes.svg)

<table>
<thead>
<tr class="header">
<th align="left">Classe de taille</th>
<th align="left">Points d’arrêt</th>
<th align="left">Taille d’écran standard (diagonale)</th>
<th align="left">.</th>
<th align="left">Tailles de fenêtre</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">Petite</td>
<td style="vertical-align:top;">640 px maximum</td>
<td style="vertical-align:top;">4&quot; à 6&quot; ; 20&quot; à 65&quot;</td>
<td style="vertical-align:top;">Téléphones, téléviseurs</td>
<td style="vertical-align:top;">320 x 569, 360 x 640, 480 x 854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Moyen</td>
<td style="vertical-align:top;">641 px à 1 007 px</td>
<td style="vertical-align:top;">7&quot; à 12&quot;</td>
<td style="vertical-align:top;">Phablettes, tablettes</td>
<td style="vertical-align:top;">960 x 540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Grande</td>
<td style="vertical-align:top;">1 008 px ou plus</td>
<td style="vertical-align:top;">13&quot; et supérieur</td>
<td style="vertical-align:top;">PC, ordinateurs portables, Surface Hub</td>
<td style="vertical-align:top;">1024 x 640, 1366 x 768, 1920 x 1080</td>
</tr>
</tbody>
</table>

## <a name="why-are-tvs-considered-small"></a>Pourquoi les téléviseurs sont-ils considérés comme de « petite » taille ? 

Bien que la plupart des écrans de télévision soient physiquement très grands (une taille de 40 à 65 pouces est courante) et offrent des résolutions élevées (HD ou 4K), la conception d'un téléviseur en 1080p que vous regardez à 3 mètres (10 pieds) de distance est différente de celle d'un moniteur en 1080p placé à 30 cm de distance sur votre bureau. Lorsque vous tenez compte de la distance, les téléviseurs 1080 pixels s'apparentent davantage à un moniteur 540 pixels qui est beaucoup plus proche.

Le système de pixels effectifs d'UWP prend automatiquement en compte la distance d’affichage. Lorsque vous spécifiez une taille pour un contrôle ou une plage de point d’arrêt, vous utilisez en fait des pixels « effectifs ». Par exemple, si vous créez un code réactif pour 1080 pixels et versions ultérieures, un moniteur de 1080 utilisera ce code, mais pas un téléviseur en 1080p, car même si un téléviseur en 1080p possède 1080 pixels physiques, il a uniquement 540 pixels effectifs. Cela rend la conception d'un téléviseur similaire à celle d'un téléphone.

## <a name="effective-pixels-and-scale-factor"></a>Pixels effectifs et facteur d’échelle

Les applications UWP mettent automatiquement à l’échelle votre interface utilisateur afin de garantir la lisibilité de votre application sur tous les appareils Windows 10. Windows met automatiquement à l’échelle chaque affichage en fonction de son nombre de PPP (points par pouce) et de la distance de visualisation de l’appareil. Les utilisateurs peuvent remplacer la valeur par défaut en accédant à la page de paramètres**Paramètres** > **Affichage** > **Mise à l'échelle et disposition**. 


## <a name="general-recommendations"></a>Recommandations générales

### <a name="small"></a>Petite
- Définissez une marge de 12 px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.
- Ancrez les [barres de l’application](../controls-and-patterns/app-bars.md) en bas de la fenêtre pour faciliter l’accessibilité.
- Utilisez une colonne/zone à la fois.
- Utilisez une icône pour représenter la recherche (n’affichez pas de zone de recherche).
- Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode Superposition pour économiser l’espace à l’écran.
- Si vous utilisez le [modèle Maître/Détails](../controls-and-patterns/master-details.md), utilisez le mode présentation empilée pour économiser l’espace d’écran.

### <a name="medium"></a>Moyen
- Définissez une marge de 24 px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.
- Placez les éléments de commande comme les [barres de l’application](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.
- Utilisez jusqu’à deux colonnes/zones.
- Affichez la zone de recherche.
- Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode partiel de manière à ce qu’une bande étroite d’icônes soit toujours affichée.
- Pensez à personnaliser davantage votre application pour les [expériences télévisuelles](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv?redirectedfrom=MSDN).

### <a name="large"></a>Grande
- Définissez une marge de 24 px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.
- Placez les éléments de commande comme les [barres de l’application](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.
- Utilisez jusqu’à trois colonnes/zones.
- Affichez la zone de recherche.
- Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode ancré pour qu’il soit toujours affiché.

>[!TIP] 
> Avec [**Continuum pour téléphones**](https://docs.microsoft.com/windows-hardware/design/device-experiences/continuum-phone?redirectedfrom=MSDN), les utilisateurs peuvent connecter des appareils mobiles Windows 10 compatibles à un moniteur, une souris et un clavier pour les utiliser comme des ordinateurs portables. N’oubliez pas cette nouvelle fonctionnalité lorsque vous concevez des points d’arrêt spécifiques : un téléphone mobile ne reste pas toujours dans la classe de taille.


