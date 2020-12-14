---
title: Notes de publication de WinUI 2.5
description: Notes de publication de WinUI 2.5, incluant les nouvelles fonctionnalités et les résolutions de bogues.
ms.date: 12/01/2020
ms.topic: reference
ms.openlocfilehash: d1b7873e874ff38037fbf766ef16b1e924edec16
ms.sourcegitcommit: 03308873eafd0f768e1c518f4d1cc4e4fe0b70b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96606027"
---
# <a name="windows-ui-library-25"></a>Bibliothèque d’interface utilisateur Windows 2.5

WinUI 2.5 est la dernière version officielle de la bibliothèque d’interface utilisateur Windows (WinUI).

WinUI est un projet open source hébergé sur GitHub dans le [dépôt de la bibliothèque d’IU Windows](https://aka.ms/winui). Inscrivez tous les rapports de bogues, demandes de fonctionnalités et contributions de code de la Communauté dans ce dépôt.

Versions de WinUI : [Page des versions sur GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

Les packages WinUI peuvent être ajoutés aux projets Visual Studio par le biais du gestionnaire de package NuGet. Pour plus d’informations, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](../getting-started.md).

Téléchargement du package NuGet : [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Nouvelles fonctionnalités

### <a name="infobar"></a>InfoBar

Le contrôle [InfoBar](/uwp/design/controls-and-patterns/infobar) est utilisé pour afficher des messages d’état à l’échelle de l’application qui sont très visibles par les utilisateurs, mais toutefois non intrusifs. Le contrôle comprend une propriété Severity qui permet d’indiquer le type de message affiché et une option pour spécifier votre propre appel à un bouton d’action ou de lien hypertexte. Comme le contrôle InfoBar (barre d’informations) est inséré avec d’autres contenus d’interface utilisateur, vous pouvez également spécifier s’il est toujours visible ou si l’utilisateur peut le faire disparaître.

Cet exemple montre un contrôle InfoBar (barre d’informations) dans l’état par défaut avec un bouton de fermeture et un message.

:::image type="content" source="../images/infobar-default-title-message.png" alt-text="Exemple de contrôle InfoBar dans l’état par défaut avec un bouton de fermeture et un message.":::

Cet exemple animé illustre un contrôle InfoBar (barre d’informations) avec différents états de gravité et des messages personnalisés.

:::image type="content" source="../images/infobar-severity-animated.gif" alt-text="Exemple animé d’un contrôle InfoBar avec différents états de gravité et des messages personnalisés.":::

[Indications d’utilisation](/windows/uwp/design/controls-and-patterns/infobar)

[Informations de référence sur les API](/windows/winui/api/microsoft.ui.xaml.controls.infobar)

### <a name="determinate-progressring"></a>ProgressRing - Déterminé

L’état Déterminé du contrôle [ProgressRing](/uwp/design/controls-and-patterns/progress-controls) indique le pourcentage d’achèvement d’une tâche. Ce contrôle doit être utilisé au cours d’une opération pour laquelle la durée est connue, mais dont la progression ne doit pas bloquer l’interaction utilisateur avec l’application.

L’image animée suivante illustre un contrôle ProgressRing avec l’état Déterminé.

:::image type="content" source="../images/progressring-determinate-mode-animated.gif" alt-text="Exemple animé d’un contrôle ProgressRing avec l’état Déterminé.":::<br>

[Indications d’utilisation](/windows/uwp/design/controls-and-patterns/progress-controls#progress-controls-best-practices)

[Informations de référence sur les API](/windows/winui/api/microsoft.ui.xaml.controls.progressring)


### <a name="navigationview-footermenuitems"></a>FooterMenuItems de NavigationView

Utilisez la propriété FooterMenuItems du contrôle NavigationView pour placer des éléments de navigation à la fin du volet de navigation (à l’inverse de la propriété MenuItems, qui place des éléments au début du volet).

L’image suivante montre un contrôle NavigationView avec les éléments de navigation *Compte*, *Votre panier* et *Aide* dans le menu de pied de page.

:::image type="content" source="../images/navigationview-footermenuitems.png" alt-text="Exemple d’image d’un contrôle NavigationView avec les éléments de navigation Compte, Votre panier et Aide dans le menu de pied de page.":::

[Indications d’utilisation](/windows/uwp/design/controls-and-patterns/navigationview?#footer-menu-items)

[Informations de référence sur les API](/windows/winui/api/microsoft.ui.xaml.controls.navigationview.footermenuitems)

## <a name="samples"></a>exemples

L’exemple d’application **Galerie de contrôles XAML** contient des exemples de chacun de ces contrôles et fonctionnalités WinUI.

Si l’application **Galerie de contrôles XAML** est installée et mise à jour vers la version la plus récente :

- Regardez le contrôle [InfoBar](xamlcontrolsgallery:/item/InfoBar) à l’œuvre.
- Regardez [ProgressRing](xamlcontrolsgallery:/item/ProgressRing) à l’œuvre.
- Regardez le contrôle [NavigationView](xamlcontrolsgallery:/item/NavigationView) à l’œuvre.

Si vous n’avez pas installé l’application XAML Controls Gallery, téléchargez-la à partir du [Microsoft Store](https://aka.ms/xamlgalleryapp).

Vous pouvez également consulter, cloner et générer le code source de la Galerie de contrôles XAML à partir de [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="other-updates"></a>Autres mises à jour

Consultez notre liste de [changements notables](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.5.0) pour un grand nombre des problèmes GitHub résolus dans cette version.
