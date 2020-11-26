---
title: Notes de publication de WinUI 2.3
description: Notes de publication de WinUI 2.3, nouvelles fonctionnalités et corrections de bogues incluses.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: ad589b15b5481b5b7ff402fc2043afd71c139479
ms.sourcegitcommit: 67c4d4ecda4ffe5f1a233de5e8555ca2228e8489
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94933064"
---
# <a name="windows-ui-library-23"></a>Bibliothèque d’IU Windows 2.3

WinUI 2.3 est la dernière version officielle de la bibliothèque d’IU Windows (WinUI).

WinUI est un projet open source hébergé sur GitHub dans le [dépôt de la bibliothèque d’IU Windows](https://aka.ms/winui). Inscrivez tous les rapports de bogues, demandes de fonctionnalités et contributions de code de la Communauté dans ce dépôt.

Versions de WinUI : [Page des versions sur GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

Les packages WinUI peuvent être ajoutés aux projets Visual Studio par le biais du gestionnaire de package NuGet. Pour plus d’informations, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](../getting-started.md).

Téléchargement du package NuGet : [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Nouvelles fonctionnalités

### <a name="progress-bar-visual-refresh"></a>Actualisation visuelle de la barre de progression

La barre de progression (**ProgressBar**) a deux représentations visuelles différentes.

#### <a name="indeterminate-progress-bar"></a>Barre de progression indéterminée

Indique qu’une tâche est en cours, mais ne bloque pas l’interaction avec l’utilisateur.

![Barre de progression indéterminée](../images/IndeterminateProgressBar.gif)

#### <a name="determinate-progress-bar"></a>Barre de progression déterminée

Indique la progression d’une quantité de travail connue. 

![Barre de progression déterminée](../images/DeterminateProgressBar.gif)

[Lien vers la documentation](/windows/uwp/design/controls-and-patterns/progress-controls)

[Lien vers des exemples](/windows/uwp/design/controls-and-patterns/progress-controls#examples)

### <a name="numberbox"></a>NumberBox

**NumberBox** représente un contrôle qui permet d’afficher et de modifier des nombres. Il prend en charge la validation, la définition d’un intervalle d’incrémentation et le calcul en ligne d’équations de base (multiplication, division, addition, soustraction, etc.).

![NumberBox](../images/NumberBoxGif.gif)

[Lien vers la documentation et des exemples](/windows/uwp/design/controls-and-patterns/number-box)

### <a name="radiobuttons"></a>RadioButtons

**RadioButtons** est un nouveau contrôle conteneur qui vous permet de créer facilement des groupes d’éléments RadioButton associés, tout en prenant en charge correctement les fonctionnalités du clavier et du narrateur/lecteur d’écran.

![Capture d’écran de trois cases d’option, avec la troisième case sélectionnée.](../images/RadioButtons.png)

[Lien vers la documentation et des exemples](https://github.com/microsoft/microsoft-ui-xaml-specs/blob/c8d3d3668af546091656dfc37436b13cd062f52d/active/radiobuttons/RadioButtons_Spec.md)

## <a name="examples"></a>Exemples

L’exemple d’application de galerie de contrôles XAML contient des démonstrations interactives et des exemples de code pour l’utilisation des contrôles WinUI.

* Installez l’application XAML Controls Gallery à partir du [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* XAML Controls Gallery est également [open source sur GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentation

Des articles de procédures pour les contrôles de la bibliothèque d’interface utilisateur Windows sont inclus dans la [documentation sur les contrôles de la plateforme Windows universelle](/windows/uwp/design/controls-and-patterns/).

Les documents de référence sur les API se trouvent ici : [API de la bibliothèque d’interface utilisateur Windows](/windows/winui/api/).