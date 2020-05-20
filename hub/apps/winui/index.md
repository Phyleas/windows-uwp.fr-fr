---
title: Bibliothèque d’interface utilisateur Windows (WinUI)
description: Bibliothèques WinUI pour le développement d’applications Windows.
ms.topic: article
ms.date: 05/11/2020
keywords: windows 10, uwp, sdk kit de ressources, winui, bibliothèque d’interface utilisateur Windows
ms.openlocfilehash: 2afa6b1eadc98300e3de76a1dfc6ede66a2a56e5
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580176"
---
# <a name="windows-ui-library-winui"></a>Bibliothèque d’interface utilisateur Windows (WinUI)

![Image Hero de kits de ressources](../images/logo-winui.png)

La bibliothèque d’interface utilisateur Windows (WinUI) est la plateforme d’interface utilisateur native moderne pour les applications Windows sur Win32 et UWP.

Avec l’incorporation du [système Fluent Design](https://www.microsoft.com/design/fluent/#/) dans l’ensemble des contrôles et des styles, WinUI fournit des expériences cohérentes, intuitives et accessibles à l’aide des modèles d’interface utilisateur les plus récents.

Et avec la prise en charge des applications Win32 et UWP, vous pouvez créer avec WinUI dès le départ ou migrer vos applications MFC, WinForms ou WPF existantes à votre rythme et en utilisant des langages connus comme C++, C#, Visual Basic et même Javascript par le biais de [React Native pour Windows](https://microsoft.github.io/react-native-windows/).

Il existe deux versions de WinUI : **WinUI 2.x** et **WinUI 3.0**.

## <a name="windows-ui-2x-library"></a>Bibliothèque Windows UI 2.x

Vous pouvez utiliser WinUI 2.x dès maintenant dans les applications UWP et l’incorporer dans vos applications de bureau existantes par le biais de [XAML Islands](/windows/apps/desktop/modernize/xaml-islands).

La bibliothèque WinUI 2.x est étroitement couplée au SDK UWP et fournit des contrôles d’interface utilisateur Windows natifs officiels et d’autres éléments d’interface utilisateur pour les applications UWP.

Avec la compatibilité de bas niveau avec les versions antérieures de Windows 10, vos contrôles WinUI 2.x fonctionnent même si les utilisateurs n’ont pas le dernier système d’exploitation.

![Prise en charge des plateformes par WinUI 2.x](../images/platforms-winui2.png)

> [!NOTE]
> WinUI 2.4 est la version la plus récente de WinUI 2.x. Pour obtenir la liste des travaux planifiés dans la prochaine version, consultez la page du [jalon WinUI 2.5](https://github.com/microsoft/microsoft-ui-xaml/milestone/10).

Pour obtenir des instructions d’installation, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](winui2/getting-started.md).

### <a name="related-links"></a>Liens connexes

- [Vue d’ensemble de la bibliothèque WinUI 2.x](winui2/index.md)
- [Documentation sur les API](https://docs.microsoft.com/uwp/api/overview/winui/)
- [Code source](https://aka.ms/winui)
- [Application XAML Controls Gallery](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-30-library-preview-1"></a>Bibliothèque d’interface utilisateur Windows 3.0 (Preview 1)

WinUI 3 est la prochaine version de WinUI, une plateforme d’interface utilisateur Windows 10 native entièrement découplée du SDK UWP.

En découplant entièrement le XAML, la composition et les API d’entrée du SDK UWP, la bibliothèque d’interface utilisateur Windows 3.0 peut étendre de façon considérable l’étendue de WinUI pour inclure la plateforme d’interface utilisateur native Windows 10 complète.

WinUI est la plateforme d’avenir par excellence pour toutes les applications Windows ; vous pouvez l’utiliser comme couche d’interface utilisateur sur votre application UWP ou Win32 native, ou pour moderniser votre application de bureau composant par composant avec [Xaml Islands](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands).
 
> [!NOTE]
> WinUI 3.0 Preview 1 est une build de préversion de WinUI 3.0. Nous serions heureux de recevoir vos commentaires dans le [dépôt GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml).

Toutes les nouvelles fonctionnalités XAML seront fournies dans le cadre de WinUI. Les API XAML UWP existantes fournies dans le cadre du système d’exploitation ne recevront plus de nouvelles mises à jour de fonctionnalités. Toutefois, elles continueront à recevoir des mises à jour de sécurité et des correctifs critiques conformément au cycle de vie de prise en charge de Windows 10.

La plateforme Windows universelle contient davantage que le simple framework XAML (par exemple des modèles d’application et de sécurité, un pipeline multimédia, les intégrations de l’interpréteur de commandes Windows 10 et Xbox ou la prise en charge étendue des appareils) et continuera à être prise en charge.

![Prise en charge des plateformes par WinUI 3.0](../images/platforms-winui3.png)

> [!Important]
> WinUI 3.0 Preview 1 est destiné à une évaluation anticipée et à la collecte de commentaires de la communauté des développeurs. Il ne doit **PAS** être utilisé pour les applications de production.

### <a name="related-links"></a>Liens connexes

- [WinUI 3.0 Preview 1 (Mai 2020)](winui3/index.md)
- [Application XAML Controls Gallery (WinUI 3.0 Preview 1)](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3alpha)

## <a name="winui-resources"></a>Ressources WinUI

**Github** : WinUI est un projet open source hébergé sur GitHub. Dans le [dépôt WinUI](https://github.com/microsoft/microsoft-ui-xaml), vous pouvez soumettre des demandes de fonctionnalités ou des bogues, interagir avec l’équipe WinUI et consulter les plans de l’équipe pour WinUI 3 et au-delà dans la [feuille de route](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md).

**Site web** : le site web [WinUI](https://aka.ms/winui) contient des informations utiles telles que des comparaisons de produits, les avantages de WinUI et les moyens de se tenir informé au sujet du produit.
