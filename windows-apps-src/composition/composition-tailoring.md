---
title: Personnalisation de la composition
description: Utilisez les API de composition pour adapter votre interface utilisateur, optimiser les performances et prendre en charge les paramètres utilisateur et les caractéristiques des appareils.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d4aa82f70e9bad7a60a97b6b28f28f3dfd008c9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166403"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Personnaliser les effets & expériences à l’aide de l’interface utilisateur Windows

L’interface utilisateur Windows fournit de nombreux effets, animations et moyens magnifiques pour la différenciation. Toutefois, la satisfaction des attentes des utilisateurs en matière de performances et de personnalisation est toujours un élément indispensable à la création d’applications réussies. Le plateforme Windows universelle prend en charge une large gamme d’appareils variés, qui ont des fonctionnalités et des fonctionnalités différentes. Pour fournir une expérience inclusive à tous vos utilisateurs, vous devez vous assurer que vos applications sont mises à l’échelle sur les appareils et respectent les préférences de l’utilisateur. La personnalisation de l’interface utilisateur peut fournir un moyen efficace de tirer parti des fonctionnalités d’un appareil et de garantir une expérience utilisateur agréable et inclusive.

La personnalisation de l’interface utilisateur est une catégorie étendue qui englobe le travail pour une interface utilisateur performante et performante pour les domaines suivants :

- Respect et adaptation aux paramètres utilisateur pour les effets
- Prise en accueillir des paramètres utilisateur pour les animations
- Optimisation de l’interface utilisateur pour les fonctionnalités matérielles données

Ici, nous allons aborder l’adaptation de vos effets et animations à la couche visuelle dans les zones ci-dessus, mais il existe de nombreux autres moyens de personnaliser votre application pour garantir une expérience utilisateur optimale. Des documents d’aide sont disponibles sur la façon d' [adapter votre interface utilisateur](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) à différents appareils et de [créer une interface utilisateur réactive](../design/layout/responsive-design.md).

## <a name="user-effects-settings"></a>Paramètres des effets utilisateur

Les utilisateurs peuvent personnaliser leur expérience Windows pour diverses raisons, quelles applications doivent respecter et s’adapter à. Un utilisateur final peut contrôler la modification des types d’effets qu’ils voient utilisés dans tout le système.

### <a name="transparency-effects-settings"></a>Paramètres des effets de transparence

L’un des paramètres d’effet que les utilisateurs peuvent personnaliser est l’activation ou la désactivation des effets de transparence. Cela se trouve dans l’application paramètres, sous personnalisation > couleurs, ou via l’application paramètres > facilité d’accès > afficher.

![Option de transparence dans les paramètres](images/tailoring-transparency-setting.png)

Lorsqu’il est activé, tout effet qui utilise la transparence s’affiche comme prévu. Cela s’applique à acrylique, HostBackdropBrush ou n’importe quel graphique d’effet personnalisé qui n’est pas entièrement opaque.

Lorsque cette option est désactivée, le matériau acrylique repasse automatiquement à une couleur unie, car le pinceau acrylique de XAML a écouté cet événement par défaut. Ici, nous voyons que l’application de calculatrice revient à une couleur unie lorsque les effets de transparence ne sont pas activés :

![Calculatrice avec ](images/tailoring-acrylic.png)
 ![ calculatrice acrylique avec acrylique répondant aux paramètres de transparence](images/tailoring-acrylic-fallback.png)

Toutefois, pour les effets personnalisés, l’application doit répondre à la propriété [uisettings. AdvancedEffectsEnabled](/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled) ou à l’événement [AdvancedEffectsEnabledChanged](/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) et faire basculer le graphique effet/effet pour utiliser un effet sans transparence. Voici un exemple :

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>Paramètres des animations

De même, les applications doivent écouter et répondre à la propriété [uisettings. AnimationsEnabled](/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) pour s’assurer que les animations sont activées ou désactivées en fonction des paramètres utilisateur dans paramètres > la facilité d’accès > afficher.

![Option animations dans les paramètres](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Tirer parti de l’API des fonctionnalités

En tirant parti des API [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) , vous pouvez détecter les fonctionnalités de composition disponibles et performantes sur le matériel donné et adapter la conception pour s’assurer que les utilisateurs finaux bénéficient d’une expérience performante et complète sur n’importe quel appareil. Les API offrent un moyen de vérifier les capacités du système matériel afin d’implémenter une mise à l’échelle de l’effet normal sur divers facteurs de forme. Cela facilite la personnalisation de l’application afin de créer une expérience utilisateur de bout parfaite et transparente.

Cette API fournit des méthodes et un écouteur d’événements qui peuvent être utilisés pour prendre des décisions en matière de mise à l’échelle de l’interface utilisateur de l’application. La fonctionnalité détecte la manière dont le système peut gérer les opérations complexes de composition et de rendu, puis renvoie les informations dans un modèle facile à utiliser, que les développeurs peuvent utiliser.

### <a name="using-composition-capabilities"></a>Utilisation des fonctionnalités de composition

La fonctionnalité CompositionCapabilities est déjà exploitée pour des fonctionnalités telles que le matériel acrylique, où le matériel revient à un effet plus performant en fonction du scénario et du matériel.

L’API peut être ajoutée au code existant en quelques étapes simples.

1. Obtenez l’objet de fonctionnalités dans le constructeur de votre application.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Inscrit un écouteur d’événements de fonctionnalités modifiées pour votre application.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Ajoutez du contenu à la méthode de rappel d’événement pour gérer différents niveaux de fonctionnalités. Cela peut ou non être similaire à l’étape suivante.
1. Lorsque vous utilisez Effects, vérifiez d’abord l’objet Capabilities. Envisagez d’utiliser des vérifications conditionnelles ou des instructions de contrôle de commutateur, en fonction de la façon dont vous souhaitez adapter les effets.

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

Vous trouverez un exemple de code complet dans l' [interface utilisateur Windows GitHub référentiel](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Effets rapides et lents

En fonction des commentaires des méthodes [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) et [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) fournies dans l’API CompositionCapabilities, l’application peut décider d’échanger des effets coûteux ou non pris en charge pour d’autres effets de leur choix qui sont optimisés pour l’appareil. Certains effets sont connus pour être plus gourmands en ressources que d’autres et doivent être utilisés avec modération, et d’autres effets peuvent être utilisés plus librement. Toutefois, pour tous les effets, vous devez être prudent lorsque vous utilisez le chaînage et l’animation, car certains scénarios ou combinaisons peuvent modifier les caractéristiques de performances du graphique d’effet. Voici quelques-unes des caractéristiques de performances de la règle de curseur pour des effets individuels :

- Les effets connus pour avoir un impact élevé sur les performances sont les suivants : Flou gaussien, masque Shadow, BackDropBrush, HostBackDropBrush et visuel de couche. Celles-ci ne sont pas recommandées pour les appareils bas de gamme [(niveau de fonctionnalité 9.1-9.3)](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)et doivent être utilisées judicieusement sur les appareils haut de gamme.
- Les effets avec impact moyen sur les performances incluent la matrice de couleurs, certains effets de fusion BlendModes (luminosité, couleur, saturation et teinte), SpotLight, SceneLightingEffect et (selon le scénario) BorderEffect. Ces effets peuvent fonctionner avec certains scénarios sur des appareils de bas niveau, mais la prudence doit être utilisée lors du Chaînage et de l’animation. Recommander la restriction d’utilisation à deux ou moins et l’animation sur les transitions uniquement.
- Tous les autres effets ont un impact faible sur les performances et fonctionnent dans tous les scénarios raisonnables lors de l’animation et du Chaînage.

## <a name="related-articles"></a>Articles connexes

- [Techniques de conception réactive UWP](../design/layout/responsive-design.md)
- [Personnalisation de l’appareil UWP](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)