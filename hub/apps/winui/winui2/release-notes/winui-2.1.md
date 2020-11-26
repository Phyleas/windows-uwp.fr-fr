---
title: Notes de publication de WinUI 2.1
description: Notes de publication de WinUI 2.1, nouvelles fonctionnalités et corrections de bogues incluses.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: d743b5653a824753706cebbfe1f1a60c419debfe
ms.sourcegitcommit: 67c4d4ecda4ffe5f1a233de5e8555ca2228e8489
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94933134"
---
# <a name="windows-ui-library-21"></a>Bibliothèque d’IU Windows 2.1

La dernière version officielle de la bibliothèque d’IU Windows (WinUI 2.1) a été publiée le 8 avril 2019. 

WinUI met à votre disposition une grande partie des fonctionnalités les plus récentes de la plateforme d’expérience utilisateur Windows, notamment des contrôles et des styles Fluent à jour, sous une forme immédiatement exploitable et offrant une compatibilité descendante avec la Mise à jour anniversaire de Windows 10 (14393). La [Galerie de contrôles XAML](/windows/uwp/design/controls-and-patterns/#xaml-controls-gallery) vous donne des exemples pour explorer toutes les nouvelles fonctionnalités intéressantes ajoutées à la bibliothèque.

Téléchargez le [package NuGet WinUI 2.1](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004).

Vous pouvez choisir d’utiliser les packages WinUI dans votre application à l’aide du gestionnaire de package NuGet. Pour plus d’informations, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/getting-started).

WinUI est un projet open source hébergé sur GitHub. Vous pouvez nous faire part de vos rapports de bogues, demandes de fonctionnalités et contributions de code de la Communauté dans le [dépôt de la bibliothèque d’interface utilisateur Windows](https://aka.ms/winui).

## <a name="whats-new-in-this-release"></a>Nouveautés de cette version

### <a name="itemsrepeater"></a>ItemsRepeater

ItemsRepeater permet de créer des expériences de collection personnalisées à l’aide d’un système de disposition flexible, de vues personnalisées et de la virtualisation.
Contrairement à un ListView, le contrôle ItemsRepeater n’offre pas d’expérience d’utilisateur final complète. Il ne propose pas d’interface utilisateur par défaut et n’offre aucune stratégie autour du focus, de la sélection ou de l’interaction utilisateur. Il s’agit d’un bloc de construction dont vous pouvez vous servir pour créer vos propres expériences à base de collections uniques et vos propres contrôles personnalisés. Il prend en charge la création d’expériences plus riches et plus performantes.

![Brève vidéo présentant le comportement du contrôle ItemsRepeater.](../images/ItemsRepeater%20-%20MSN%20News.gif)

[Documentation](/windows/uwp/design/controls-and-patterns/items-repeater)

### <a name="animatedvisualplayer"></a>AnimatedVisualPlayer

AnimatedVisualPlayer héberge et contrôle la lecture des visuels animés, ce qui vous permet d’ajouter à votre application des graphiques de mouvement personnalisés hautes performances. Par exemple, AnimatedVisualPlayer est utilisé pour afficher et contrôler les animations Lottie.

![Brève vidéo présentant le comportement du contrôle AnimatedVisualPlayer.](../images/AnimatedVisualPlayerUpdated.gif)

[Documentation](/windows/communitytoolkit/animations/lottie)

### <a name="teachingtip"></a>TeachingTip

TeachingTip fournit aux applications un moyen attrayant et de type Fluent pour guider et informer les utilisateurs avec des conseils non invasifs et riches en contenu. TeachingTip peut mettre en avant des fonctionnalités nouvelles ou importantes, apprendre aux utilisateurs à effectuer des tâches et améliorer le workflow en fournissant en direct des informations contextuellement pertinentes pour votre tâche.

![Brève vidéo présentant le comportement du contrôle TeachingTip.](../images/TeachingTipUpdated.gif)

[Documentation](/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)

### <a name="radiomenuflyoutitem"></a>RadioMenuFlyoutItem

Offre la possibilité d’avoir des options de style « case d’option » dans une barre de menus. Cela permet de regrouper des options avec des puces qui sont liées comme un groupe de cases d’option. La logique est gérée pour le développeur.

![Capture d’écran montrant le comportement du contrôle RadioMenuFlyoutItem.](../images/RadioMenuFlyoutItem1.png)

[Documentation](/windows/uwp/design/controls-and-patterns/menus#create-a-menu-flyout-or-a-context-menu)

### <a name="compactdensity"></a>CompactDensity

Le mode compact permet aux développeurs de créer des expériences confortables pour un nombre quelconque de scénarios. Il vous suffit d’ajouter un dictionnaire de ressources pour que votre application puisse accueillir en moyenne 33 % d’IU en plus.

![Capture d’écran montrant le comportement du contrôle CompactDensity.](../images/CompactDensityUpdated.png)

[Documentation](/windows/uwp/design/style/spacing)

### <a name="shadows"></a>Ombres

![Exemple](../images/shadow.gif)

La création d’une hiérarchie visuelle d’éléments dans votre interface utilisateur facilite l’analyse de celle-ci et comporte ce sur quoi il est important de se concentrer. L’élévation, qui consiste à mettre en avant des éléments spécifiques de votre interface utilisateur, est souvent utilisée pour obtenir une telle hiérarchie dans le logiciel. 

Avec la Mise à jour de mai 2019 de Windows 10, un grand nombre de nos contrôles courants ajoutent une élévation en utilisant par défaut la profondeur Z et l’ombre. Les contrôles NavigationView et TeachingTip dans WinUI 2.1 ont également des ombres par défaut lors de l’exécution sur un système d’exploitation doté de la Mise à jour de mai 2019 de Windows 10. La liste complète des contrôles qui ont des ombres par défaut et les instructions sur la façon d’utiliser les API supplémentaires seront disponibles dès la publication de la Mise à jour de mai 2019 de Windows 10 par le biais du lien qui sera publié ici.

## <a name="examples"></a>Exemples

L’exemple d’application de galerie de contrôles XAML contient des démonstrations interactives et des exemples de code pour l’utilisation des contrôles WinUI.

* Installez l’application XAML Controls Gallery à partir du [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* XAML Controls Gallery est également [open source sur GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentation

Des articles de procédures pour les contrôles de la bibliothèque d’interface utilisateur Windows sont inclus dans la [documentation sur les contrôles de la plateforme Windows universelle](/windows/uwp/design/controls-and-patterns/).

Les documents de référence sur les API se trouvent ici : [API de la bibliothèque d’interface utilisateur Windows](/windows/winui/api/).

## <a name="microsoftuixaml-21-version-history"></a>Historique des versions de Microsoft.UI.Xaml 2.1

### <a name="microsoftuixaml-21-official-release"></a>Version officielle de Microsoft.UI.Xaml 2.1

Avril 2019

[Page des versions sur GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases)

[Téléchargement du package NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

#### <a name="new-feature-not-included-in-earlier-pre-releases"></a>Nouvelle fonctionnalité (non incluse dans les préversions antérieures)

* [CompactDensity](/windows/uwp/design/style/spacing) : mode compact permettant aux développeurs de créer des expériences confortables pour un nombre quelconque de scénarios. Il vous suffit d’ajouter un dictionnaire de ressources pour que votre application puisse accueillir en moyenne 33 % d’IU en plus.

* Ombres : la création d’une hiérarchie visuelle d’éléments dans votre interface utilisateur facilite l’analyse de celle-ci et comporte ce sur quoi il est important de se concentrer. L’élévation, qui consiste à mettre en avant des éléments spécifiques de votre interface utilisateur, est souvent utilisée pour obtenir une telle hiérarchie dans le logiciel. Un grand nombre de nos contrôles courants ajoutent une élévation en utilisant par défaut la profondeur Z et l’ombre.  

### <a name="microsoftuixaml-21190218001-prerelease"></a>Microsoft.UI.Xaml 2.1.190218001-prerelease

Février 2019

[Page des versions sur GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190219001-prerelease)

[Téléchargement du package NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190218001-prerelease)

Nouvelles fonctionnalités expérimentales :

* [Contrôle TeachingTip](https://github.com/Microsoft/microsoft-ui-xaml/issues/21)  
  Ce nouveau contrôle permet à votre application de guider et d’informer les utilisateurs de celle-ci avec une notification non invasive et riche en contenu. TeachingTip permet de mettre en avant une fonctionnalité nouvelle ou importante, d’apprendre aux utilisateurs à effectuer une tâche ou d’améliorer le workflow de l’utilisateur en fournissant en direct des informations contextuellement pertinentes pour leur tâche.

### <a name="microsoftuixaml-21190131001-prerelease"></a>Microsoft.UI.Xaml 2.1.190131001-prerelease

Février 2019

[Page des versions sur GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190131001-prerelease)

[Téléchargement du package NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190131001-prerelease)

Nouvelles fonctionnalités expérimentales :

* [AnimatedVisualPlayer](/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)  
  Ce nouveau contrôle permet d’exécuter des animations vectorielles hautes performances complexes, y compris des animations [Lottie](https://github.com/airbnb/lottie) créées à l’aide de [Lottie-Windows](/windows/communitytoolkit/animations/lottie).

### <a name="microsoftuixaml-21181217001-prerelease"></a>Microsoft.UI.Xaml 2.1.181217001-prerelease

Décembre 2018

[Page des versions sur GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.181217001-prerelease)

[Téléchargement du package NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.181217001-prerelease)

Nouvelles fonctionnalités expérimentales :

* [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)

* [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)