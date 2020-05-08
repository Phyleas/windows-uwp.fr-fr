---
Description: Utilisez les commentaires visuels pour montrer aux utilisateurs quand leurs interactions avec une application Windows sont détectées, interprétées et gérées.
title: Retour visuel
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: retour visuel, retour de focus, retour tactile, visualisation de contact, entrées, interaction
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fcb6945c488bc1b715c339fa39949ea62bdb2a12
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970074"
---
# <a name="guidelines-for-visual-feedback"></a>Recommandations en matière de retour visuel

Utilisez le retour visuel pour indiquer aux utilisateurs quand leurs interactions sont détectées, interprétées et gérées. Le retour visuel peut aider les utilisateurs en encourageant l’interaction. Il indique le succès d’une interaction et améliore ainsi le sentiment de contrôle de l’utilisateur. Il transmet également l’état du système et réduit les erreurs.

> **API importantes**: [**Windows. Devices. Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows. UI. Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)

## <a name="recommendations"></a>Recommandations

- Essayez de limiter les modifcations d’un modèle de contrôle à ceux directement liés à votre intention de conception, car des modifications approfondies peuvent avoir un impact sur les performances et l’accessibilité du contrôle et de votre application. 
    - Pour plus d’informations sur la personnalisation des propriétés d’un contrôle, consultez [styles XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) , y compris les propriétés d’état visuel.
    - Pour plus d’informations sur l’apport de modifications à un modèle de contrôle, consultez la [classe UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)
    - Envisagez de créer votre propre contrôle personnalisé basé sur des modèles si vous devez apporter des modifications importantes à un modèle de contrôle. Pour obtenir un exemple de contrôle personnalisé basé sur un modèle, consultez l' [exemple Custom Edit Control](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).
- N’utilisez pas les visualisations tactiles dans des situations où elles risquent d’interférer avec l’utilisation de l’application. Pour plus d’informations, consultez [**ShowGestureFeedback**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback).
- N’affichez pas de retour à moins que ce soit absolument nécessaire. Veillez à ce que l’interface utilisateur soit propre et aérée en n’affichant pas de retour visuel, à moins que cela ajoute une valeur non disponible ailleurs.
- Ne personnalisez pas les comportements de retour visuel des mouvements intégrés de Windows de manière radicale, car cela peut créer une expérience utilisateur incohérente et confuse.

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires

Les visualisations de contact sont essentielles pour les interactions tactiles qui exigent précision et exactitude. Par exemple, votre application doit clairement indiquer l’emplacement d’un appui pour permettre à l’utilisateur de savoir s’il a manqué sa cible, de combien il l’a manquée et quels réglages il doit effectuer.

L’utilisation des contrôles de la plateforme XAML disponibles permet de garantir le fonctionnement correct de votre application sur tous les appareils et dans toutes les situations d’entrée. Si votre application propose des interactions personnalisées qui nécessitent un retour personnalisé, vous devez veiller à ce que le retour soit approprié, qu’il s’étende sur les périphériques d’entrée et qu’il ne risque pas de distraire l’utilisateur de sa tâche. Cela peut s’avérer particulièrement gênant dans les applications de jeu ou de dessin où le retour visuel peut entrer en conflit avec des éléments essentiels de l’interface utilisateur ou les masquer.

> [!Important]
> Nous ne conseillons pas de modifier le comportement d’interaction des mouvements intégrés.

**Retour visuel sur tous les appareils :**

Le retour visuel dépend généralement du périphérique d’entrée (entrée tactile, pavé tactile, souris, stylo/stylet, clavier, etc.). Par exemple, le retour intégré pour une souris implique habituellement le déplacement et le changement du curseur, l’entrée tactile et le stylo nécessitent des visualisations de contact, et l’entrée et la navigation au clavier utilisent la mise en surbrillance et des rectangles de sélection.

Utilisez [**ShowGestureFeedback**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback) pour définir le comportement de commentaires des mouvements de la plateforme.

Si vous personnalisez l’interface utilisateur de retour, veillez à fournir un retour d’interaction prenant en charge tous les modes d’entrée et approprié à tous ces modes.

Voici quelques exemples de visualisations de contact intégrées à Windows :

| ![Retour tactile](images/TouchFeedback.png) | ![Retour de la souris](images/MouseFeedback.png) | ![Retour du stylo](images/PenFeedback.png) | ![Retour du clavier](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Visualisation tactile | Visualisation de souris/pavé tactile | Visualisation de stylo | Visualisation de clavier |

## <a name="high-visibility-focus-visuals"></a>Visuels du focus à haute visibilité

Toutes les applications Windows comportent un visuel du focus plus défini autour des contrôles interactifs de l’application. Ces nouveaux visuels du focus sont entièrement personnalisables, mais peuvent également être désactivés si nécessaire.

Pour l' **expérience de 10 pieds** typique de l’utilisation de la Xbox et de la télévision, Windows prend en charge le **focus**, un effet d’éclairage qui anime la bordure d’éléments pouvant être actifs, tels qu’un bouton, lorsqu’ils se concentrent sur le boîtier ou l’entrée au clavier. Pour plus d’informations, consultez [conception pour la Xbox et la télévision](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv#reveal-focus).

## <a name="color-branding--customizing"></a>Personnalisation des couleurs et des visuels du focus

### <a name="border-properties"></a>Propriétés des bordures

Les visuels du focus à haute visibilité sont composés de deux éléments : la bordure principale et la bordure secondaire. La bordure principale a une épaisseur de **2 px** et apparaît autour de la bordure secondaire *extérieure*. La bordure secondaire a une épaisseur de **1 px** et apparaît autour de la bordure principale *intérieure*.
![Lignes rouges des visuels du focus à haute visibilité](images/FocusRectRedlines.png)

Pour modifier l’épaisseur des bordures (principale ou secondaire), utilisez les propriétés **FocusVisualPrimaryThickness** ou **FocusVisualSecondaryThickness**, respectivement :
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Épaisseurs des marges des visuels du focus à haute visibilité](images/FocusMargin.png)

La marge est une propriété de type [**Thickness**](https://docs.microsoft.com/dotnet/api/system.windows.thickness)et, par conséquent, la marge peut être personnalisée pour s’afficher uniquement sur certains côtés du contrôle. Voir ci- ![dessous : focus de visibilité haute épaisseur marge inférieure uniquement](images/FocusThicknessSide.png)

La marge est l’espace entre les limites visuelles du contrôle et le début de la *bordure secondaire*des éléments visuels du focus. La marge par défaut est **1px** en dehors des limites du contrôle. Vous pouvez modifier cette marge par contrôle, en modifiant la propriété **FocusVisualMargin** :
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![Différences des marges des visuels du focus à haute visibilité](images/FocusPlusMinusMargin.png)

*Une marge négative éloignera la bordure du centre du contrôle ; en revanche, une marge positive rapprochera la bordure du centre du contrôle.*

Pour désactiver les visuels du focus du contrôle, désactivez simplement **UseSystemFocusVisuals** :
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

L’épaisseur, la marge ou la présence souhaitée (ou non) de visuels du focus sont déterminées pour chaque contrôle.

### <a name="color-properties"></a>Propriétés de couleur

Les visuels du focus comportent seulement deux propriétés de couleur : la couleur de la bordure principale et secondaire. Ces couleurs des bordures des visuels du focus peuvent être modifiées pour chaque contrôle au niveau page et, plus globalement, à l’échelle de l’application :

Pour personnaliser les visuels du focus à l’échelle de l’application, remplacez les pinceaux système :
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![Changements de couleurs des visuels du focus à haute visibilité](images/FocusRectColorChanges.png)

Pour modifier les couleurs pour chaque contrôle, modifiez simplement les propriétés visuelles du focus sur le contrôle souhaité :
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## <a name="related-articles"></a>Articles connexes

### <a name="for-designers"></a>Pour les concepteurs

- [Recommandations en matière de mouvement panoramique](guidelines-for-panning.md)

### <a name="for-developers"></a>Pour les développeurs

- [Interactions utilisateur personnalisées](https://docs.microsoft.com/windows/uwp/design/layout/index)

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple d’événements d’entrée utilisateur XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrée : exemple de fonctionnalités de périphériques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrée : exemple de test de positionnement tactile](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemple de défilement XAML, panoramique et zoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrée : exemple d’entrée manuscrite simplifiée](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrée : exemple de mouvements Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrée : manipulations et gestes, exemple](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Exemple d’entrée tactile DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
 

 
