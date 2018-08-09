---
author: Karl-Bridge-Microsoft
Description: Use visual feedback to show users when their interactions with a UWP app are detected, interpreted, and handled.
title: Retour visuel
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: retour visuel, retour de focus, retour tactile, visualisation de contact, entrées, interaction
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4653e07d6d81f884ee6519206e75350aa47945f8
ms.sourcegitcommit: f91aa1e402f1bc093b48a03fbae583318fc7e05d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2018
ms.locfileid: "1917584"
---
# <a name="guidelines-for-visual-feedback"></a>Recommandations en matière de retour visuel

Utilisez le retour visuel pour indiquer aux utilisateurs quand leurs interactions sont détectées, interprétées et gérées. Le retour visuel peut aider les utilisateurs en encourageant l’interaction. Il indique le succès d’une interaction et améliore ainsi le sentiment de contrôle de l’utilisateur. Il transmet également l’état du système et réduit les erreurs.

> **API importantes**:  [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)

## <a name="recommendations"></a>Recommandations

- Essayez de limiter les modifications d’un modèle de contrôle à celles directement liées à votre intention de conception, car des modifications importantes peuvent avoir un impact sur les performances et l’accessibilité du contrôle et de votre application. 
    - Voir [Styles XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) pour plus d’informations sur la personnalisation des propriétés d’un contrôle, notamment les propriétés de l’état visuel.
    - Voir la [classe UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) pour plus d’informations sur les modifications apportées à un modèle de contrôle
    - Envisagez de créer votre propre contrôle basé sur un modèle personnalisé si vous avez besoin d’apporter des modifications importantes à un modèle de contrôle. Pour obtenir un exemple de contrôle basé sur un modèle personnalisé, voir [Exemple de contrôle d’édition personnalisé](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).
- N’utilisez pas les visualisations tactiles dans des situations où elles risquent d’interférer avec l’utilisation de l’application. Pour plus d’informations, voir [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969).
- N’affichez pas de retour à moins que ce soit absolument nécessaire. Veillez à ce que l’interface utilisateur soit propre et aérée en n’affichant pas de retour visuel, à moins que cela ajoute une valeur non disponible ailleurs.
- Ne personnalisez pas les comportements de retour visuel des mouvements intégrés de Windows de manière radicale, car cela peut créer une expérience utilisateur incohérente et confuse.

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires

Les visualisations de contact sont essentielles pour les interactions tactiles qui exigent précision et exactitude. Par exemple, votre application doit clairement indiquer l’emplacement d’un appui pour permettre à l’utilisateur de savoir s’il a manqué sa cible, de combien il l’a manquée et quels réglages il doit effectuer.

L’utilisation des contrôles de la plateforme XAML disponibles permet de garantir le fonctionnement correct de votre application sur tous les appareils et dans toutes les situations d’entrée. Si votre application propose des interactions personnalisées qui nécessitent un retour personnalisé, vous devez veiller à ce que le retour soit approprié, qu’il s’étende sur les périphériques d’entrée et qu’il ne risque pas de distraire l’utilisateur de sa tâche. Cela peut s’avérer particulièrement gênant dans les applications de jeu ou de dessin où le retour visuel peut entrer en conflit avec des éléments essentiels de l’interface utilisateur ou les masquer.

> [!Important]
> Nous ne conseillons pas de modifier le comportement d’interaction des mouvements intégrés.

**Retour visuel sur tous les appareils:**

Le retour visuel dépend généralement du périphérique d’entrée (entrée tactile, pavé tactile, souris, stylo/stylet, clavier, etc.). Par exemple, le retour intégré pour une souris implique habituellement le déplacement et le changement du curseur, l’entrée tactile et le stylo nécessitent des visualisations de contact, et l’entrée et la navigation au clavier utilisent la mise en surbrillance et des rectangles de sélection.

La propriété [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969) vous sert à définir le comportement de retour pour les mouvements de la plateforme.

Si vous personnalisez l’interface utilisateur de retour, veillez à fournir un retour d’interaction prenant en charge tous les modes d’entrée et approprié à tous ces modes.

Voici quelques exemples de visualisations de contact intégrées à Windows:

| ![Retour tactile](images/TouchFeedback.png) | ![Retour de la souris](images/MouseFeedback.png) | ![Retour du stylo](images/PenFeedback.png) | ![Retour du clavier](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Visualisation tactile | Visualisation de souris/pavé tactile | Visualisation de stylo | Visualisation de clavier |

## <a name="high-visibility-focus-visuals"></a>Visuels du focus à haute visibilité

Toutes les applications Windows comportent un visuel du focus plus défini autour des contrôles interactifs de l’application. Ces nouveaux visuels du focus sont entièrement personnalisables, mais peuvent également être désactivés si nécessaire.

Pour l'**expérience «10-foot»** typique de l'utilisation de Xbox et de la télévision, Windows prend en charge **Révéler focus**, un effet d’éclairage qui anime la bordure d’éléments pouvant être actifs, par exemple un bouton, lorsqu’ils reçoivent le focus via une entrée par un boîtier de commande ou un clavier. Pour plus d’informations, consultez [Conception pour Xbox et télévision](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv#reveal-focus).

## <a name="color-branding--customizing"></a>Personnalisation des couleurs et des visuels du focus

**Propriétés des bordures**

Les visuels du focus à haute visibilité sont composés de deux éléments: la bordure principale et la bordure secondaire. La bordure principale a une épaisseur de **2px** et apparaît autour de la bordure secondaire *extérieure*. La bordure secondaire a une épaisseur de **1px** et apparaît autour de la bordure principale *intérieure*.
![Lignes rouges des visuels du focus à haute visibilité](images/FocusRectRedlines.png)

Pour modifier l’épaisseur des bordures (principale ou secondaire), utilisez les propriétés **FocusVisualPrimaryThickness** ou **FocusVisualSecondaryThickness**, respectivement:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Épaisseurs des marges des visuels du focus à haute visibilité](images/FocusMargin.png)

La marge est une propriété de type [**Thickness**](https://msdn.microsoft.com/library/system.windows.thickness); par conséquent, la marge peut être personnalisée afin d’apparaître uniquement sur certains côtés du contrôle. Voir ci-dessous: ![Épaisseurs des marges des visuels du focus à haute visibilité, bas uniquement](images/FocusThicknessSide.png)

La marge est l’espace entre les limites de l’élément visuel du contrôle et le début de la *bordure secondaire* des visuels du focus. La marge par défaut est à **1px** des limites du contrôle. Vous pouvez modifier cette marge pour chaque contrôle, en modifiant la propriété **FocusVisualMargin**:
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![Différences des marges des visuels du focus à haute visibilité](images/FocusPlusMinusMargin.png)

*Une marge négative éloignera la bordure du centre du contrôle; en revanche, une marge positive rapprochera la bordure du centre du contrôle.*

Pour désactiver les visuels du focus du contrôle, désactivez simplement **UseSystemFocusVisuals**:
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

L’épaisseur, la marge ou la présence souhaitée (ou non) de visuels du focus sont déterminées pour chaque contrôle.

**Propriétés de couleur**

Les visuels du focus comportent seulement deux propriétés de couleur: la couleur de la bordure principale et secondaire. Ces couleurs des bordures des visuels du focus peuvent être modifiées pour chaque contrôle au niveau page et, plus globalement, à l’échelle de l’application:

Pour personnaliser les visuels du focus à l’échelle de l’application, remplacez les pinceaux système:
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![Changements de couleurs des visuels du focus à haute visibilité](images/FocusRectColorChanges.png)

Pour modifier les couleurs pour chaque contrôle, modifiez simplement les propriétés visuelles du focus sur le contrôle souhaité:
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## <a name="related-articles"></a>Articles connexes

**Pour les concepteurs**
* [Recommandations en matière de mouvement panoramique](guidelines-for-panning.md)

**Pour les développeurs**
* [Interactions utilisateur personnalisées](https://msdn.microsoft.com/library/windows/apps/mt185599)

**Exemples**
* [Exemple d’entrée de base](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemples d’archive**
* [Entrée: exemple d’événements d’entrée utilisateurXAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrée : exemple de fonctionnalités d’appareils](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : exemple de test de positionnement tactile](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Exemple de zoom, de panoramique et de défilement XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : exemple d’entrée manuscrite simplifiée](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrée : exemple de mouvements Windows 8](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrée : exemple de manipulations et de mouvements (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Exemple d’entrée tactile DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 