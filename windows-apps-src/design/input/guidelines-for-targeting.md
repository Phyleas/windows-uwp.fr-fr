---
Description: Cette rubrique décrit l’utilisation de la géométrie de contact pour le ciblage tactile et indique les meilleures pratiques de ciblage dans les applications Windows Runtime.
title: Ciblage
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b1cac04405f18aaf3c8f39f9bfce2b965577807
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257939"
---
# <a name="guidelines-for-touch-targets"></a>Instructions pour les cibles tactiles

Tous les éléments d’interface utilisateur interactifs dans votre application de plateforme Windows universelle (UWP) doivent être suffisamment volumineux pour permettre aux utilisateurs d’accéder et d’utiliser avec précision, quel que soit le type d’appareil ou la méthode d’entrée.

La prise en charge des entrées tactiles (et la nature relativement imprécise de la zone de contact tactile) requiert une optimisation supplémentaire en ce qui concerne la taille cible et la disposition des contrôles, car le plus grand ensemble plus complexe de données d’entrée signalé par le digitaliseur tactile est utilisé pour déterminer cible prévue de l’utilisateur (ou la plus probable).

Tous les contrôles UWP ont été conçus avec des formats et des mises en page tactiles cibles par défaut qui vous permettent de créer des applications équilibrées et attrayantes qui sont confortables, faciles à utiliser et d’inspirer la confiance.

Dans cette rubrique, nous décrivons ces comportements par défaut afin que vous puissiez concevoir votre application pour une utilisation maximale à l’aide de contrôles de plateforme et de contrôles personnalisés (si votre application en a besoin).

> **API importantes** : [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Taille Fluent Standard

La *taille Fluent Standard* a été créée pour fournir un équilibre entre le confort de l’utilisateur et la densité des informations. Tous les éléments sur l’écran s’alignent sur une cible de pixels effectifs (epx) de 40x40, ce qui permet d’aligner les éléments de l’interface utilisateur sur une grille et de les mettre à l’échelle de manière appropriée en fonction de la mise à l’échelle au niveau du système.

> [!NOTE]
>Pour plus d’informations sur les pixels effectifs et la mise à l’échelle, consultez [Présentation de la conception des applications UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Pour plus d’informations sur la mise à l’échelle au niveau du système, consultez [Alignement, marge, remplissage](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Taille Fluent Compact

Les applications peuvent afficher un plus haut niveau de densité des informations avec un *dimensionnement compact Fluent*. Le dimensionnement compact aligne les éléments d’interface utilisateur sur une cible 32 x 32 EPX, ce qui permet aux éléments d’interface utilisateur de s’aligner sur une grille plus étroite et de les mettre à l’échelle de manière appropriée en fonction de la mise à l’échelle du

### <a name="examples"></a>Exemples

Le dimensionnement compact peut être appliqué au niveau de la page ou de la grille.

### <a name="page-level"></a>Niveau page

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>Niveau grille

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>Taille cible

En général, définissez la taille de la cible tactile sur 7,5 mm de la plage carrée (40 x 40 pixels sur un affichage de 135 PPP à un plateau de mise à l’échelle de 1,0 x). En règle générale, les contrôles UWP s’alignent sur la cible tactile 7,5 mm (cela peut varier en fonction du contrôle spécifique et de tous les modèles d’utilisation courants). Pour plus d’informations, consultez contrôle de la [taille et](../style/spacing.md) de la densité.

Vous pouvez modifier ces recommandations en matière de tailles de cibles en fonction du scénario particulier de votre application. Voici quelques points à prendre en compte :

- Fréquence des touches : envisagez de faire en sorte que les cibles qui sont trop longues ou bien dépassent la taille minimale.
- Conséquence de l’erreur : les cibles qui ont des conséquences graves si touchées en erreur doivent avoir une marge de remplissage supérieure et être placées plus loin à partir du bord de la zone de contenu. Ceci concerne tout particulièrement les cibles fréquemment utilisées.
- Position dans la zone de contenu.
- Taille de l’écran et du facteur de forme.
- Position du doigt.
- Visualisations tactiles.

## <a name="related-articles"></a>Articles associés

- [Présentation de la conception des applications UWP](../basics/design-and-ui-intro.md)
- [Taille de contrôle et densité](../style/spacing.md)
- [Alignement, marge, remplissage](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple d’événements d’entrée d’utilisateur XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
- [Entrée : exemple de fonctionnalités de l’appareil](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
- [Entrée : exemple de test de positionnement tactile](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
- [Exemple de défilement XAML, panoramique et zoom](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
- [Entrée : exemple d’encre simplifiée](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
- [Entrée : exemple de gestes Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrée : manipulations et mouvements (C++), exemple](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
- [Exemple d’entrée tactile DirectX](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
