---
description: Concevez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, y compris le sens du déroulement de droite à gauche.
title: Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, adaptabilité, localisation, RTL, LTR
ms.localizationpriority: medium
ms.openlocfilehash: 0e4725f6d26cf1abf42effddd813c31e87926e89
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033792"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG
Concevez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, y compris le sens du déroulement de droite à gauche. Le sens du déroulement est le sens dans lequel le script est écrit et affiché, et les éléments d’interface utilisateur de la page sont analysés à l’oeil.

## <a name="layout-guidelines"></a>Recommandations en matière de disposition
Les langues telles que l’allemand et le finnois utilisent généralement plus de caractères que l’anglais. Les polices d’Extrême-Orient requièrent généralement plus de hauteur. Les langues telles que l’arabe et l’hébreu requièrent que les panneaux de disposition et les éléments de texte soient disposés dans l’ordre de lecture de droite à gauche (RTL).

En raison de ces variations dans les métriques du texte traduit, nous vous recommandons de ne pas intégrer un positionnement absolu, des largeurs fixes ou des hauteurs fixes dans votre interface utilisateur. Au lieu de cela, tirez parti des mécanismes de disposition dynamique intégrés aux éléments d’interface utilisateur de Windows. Par exemple, les contrôles de contenu (tels que les boutons), les contrôles d’éléments (tels que les affichages de grille et les affichages de liste) et les panneaux de disposition (tels que les grilles et les StackPanel) sont redimensionnés et redistribués par défaut pour s’adapter à leur contenu. Pseudo-Localisez votre application pour découvrir les cas problématiques où vos éléments d’interface utilisateur ne sont pas correctement dimensionnés au contenu.

La disposition dynamique est la technique recommandée et vous pouvez l’utiliser dans la majorité des cas. Moins préférable, mais encore mieux que la cuisson de tailles dans le balisage de votre interface utilisateur, consiste à définir des valeurs de disposition en tant que fonction de langage. Voici un exemple de la façon dont vous pouvez exposer une propriété Width dans votre application en tant que ressource que les localisateurs peuvent définir de manière appropriée par langue. Tout d’abord, dans le fichier de ressources de votre application (. resw), ajoutez un identificateur de propriété portant le nom « TitleText. Width » (au lieu de « TitleText », vous pouvez utiliser n’importe quel nom de votre choix). Utilisez ensuite un **x :uid** pour associer un ou plusieurs éléments d’interface utilisateur à cet identificateur de propriété.

```xaml
<TextBlock x:Uid="TitleText">
```

Pour plus d’informations sur les fichiers de ressources (. resw), les identificateurs de propriété et le **x :uid** , consultez [localiser des chaînes dans votre interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md).

## <a name="fonts"></a>Fonts
Utilisez la classe de mappage de police [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) pour l’accès par programmation à la famille de polices, à la taille, au poids et au style recommandés pour une langue particulière. La classe **LanguageFont** fournit l’accès aux informations de police correctes pour différentes catégories de contenu, notamment les en-têtes d’interface utilisateur, les notifications, le corps de texte et les polices de corps de document modifiables par l’utilisateur.

## <a name="mirroring-images"></a>Mise en miroir des images
Si votre application contient des images qui doivent être mises en miroir (autrement dit, la même image peut être retournée) pour RTL, vous pouvez utiliser **FlowDirection** .

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

Si votre application requiert une image différente pour retourner correctement l’image, vous pouvez utiliser le système de gestion des ressources avec le `LayoutDirection` qualificateur (consultez la section LayoutDirection de [adapter vos ressources pour connaître la langue, la mise à l’échelle et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)). Le système choisit une image nommée `file.layoutdir-rtl.png` lorsque la langue du runtime de l’application (voir présentation des langages de [profil utilisateur et des langages du manifeste d’application](manage-language-and-region.md)) est définie sur une langue de droite à gauche. Cette approche peut s’avérer nécessaire lorsqu’une certaine partie de l’image est pivotée, alors qu’une autre partie ne l’est pas.

## <a name="handling-right-to-left-rtl-languages"></a>Gestion des langues de droite à gauche (RTL)
Lorsque votre application est localisée pour les langues de droite à gauche, utilisez la propriété [**FrameworkElement. FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) , puis définissez le remplissage symétrique et les marges. Des panneaux de disposition tels que l’échelle de la [**grille**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) et le retournement automatique avec la valeur de **FlowDirection** que vous définissez.

Définissez **FlowDirection** dans le panneau de disposition racine (ou le cadre) de votre page, ou sur la page elle-même. Ainsi, tous les contrôles contenus dans héritent de cette propriété.

> [!IMPORTANT]
> Toutefois, **FlowDirection** n’est *pas* défini automatiquement en fonction de la langue d’affichage sélectionnée par l’utilisateur dans les paramètres Windows ; Il n’est pas non plus modifié de manière dynamique en réponse à la langue d’affichage du changement d’utilisateur. Si l’utilisateur bascule les paramètres Windows de l’anglais vers l’arabe, par exemple, la propriété **FlowDirection** ne passe *pas* automatiquement de gauche à droite à droite à gauche. En tant que développeur d’applications, vous devez définir **FlowDirection** de manière appropriée pour la langue que vous affichez actuellement.

La technique de programmation consiste à utiliser la `LayoutDirection` propriété de la langue d’affichage utilisateur par défaut pour définir la propriété [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (Voir l’exemple de code ci-dessous). La plupart des contrôles inclus dans Windows utilisent déjà **FlowDirection** . Si vous implémentez un contrôle personnalisé, il doit utiliser **FlowDirection** pour apporter des modifications de disposition appropriées pour les langues RTL et LTR.

```csharp    
this.languageTag = Windows.Globalization.ApplicationLanguages.Languages[0];

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

var flowDirectionSetting = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
if (flowDirectionSetting == "LTR")
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.LeftToRight;
}
else
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.RightToLeft;
}
```

```cppwinrt
#include <winrt/Windows.ApplicationModel.Resources.Core.h>
#include <winrt/Windows.Globalization.h>
...

m_languageTag = Windows::Globalization::ApplicationLanguages::Languages().GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView().QualifierValues().Lookup(L"LayoutDirection");
if (flowDirectionSetting == L"LTR")
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::LeftToRight);
}
else
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::RightToLeft);
}
```

```cpp
this->languageTag = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView()->QualifierValues->Lookup("LayoutDirection");
if (flowDirectionSetting == "LTR")
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::LeftToRight;
}
else
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::RightToLeft;
}
```

La technique ci-dessus rend **FlowDirection** une fonction de la `LayoutDirection` propriété de la langue d’affichage par défaut de l’utilisateur. Si, pour une raison quelconque, vous ne souhaitez pas cette logique, vous pouvez exposer une propriété FlowDirection dans votre application en tant que ressource que les localisateurs peuvent définir de manière appropriée pour chaque langue dans laquelle ils traduisent.

Tout d’abord, dans le fichier de ressources de votre application (. resw), ajoutez un identificateur de propriété portant le nom « MainPage. FlowDirection » (au lieu de « MainPage », vous pouvez utiliser n’importe quel nom de votre choix). Utilisez ensuite un **x :uid** pour associer votre élément de **page** principale (ou son panneau de disposition racine ou son cadre) à cet identificateur de propriété.

```xaml
<Page x:Uid="MainPage">
```

Au lieu d’une seule ligne de code pour tous les langages, cela dépend du traducteur « traduisant » cette ressource de propriété correctement pour chaque langue traduite. n’oubliez pas qu’il existe une opportunité supplémentaire pour les erreurs humaines lorsque vous utilisez cette technique.

## <a name="important-apis"></a>API importantes
* [FrameworkElement. FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Rubriques connexes
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md)
* [Adaptez vos ressources à la langue, à la mise à l’échelle et à d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md)
