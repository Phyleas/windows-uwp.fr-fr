---
title: Bibliothèque d’interface utilisateur Windows (WinUI)
description: Bibliothèques WinUI pour le développement d’applications Windows.
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, sdk kit de ressources, winui, bibliothèque d’interface utilisateur Windows
ms.openlocfilehash: 3b2b18ab35f46b132ec3017bb0f3c6564b7be9ee
ms.sourcegitcommit: 67c4d4ecda4ffe5f1a233de5e8555ca2228e8489
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94933124"
---
# <a name="windows-ui-library-winui"></a>Bibliothèque d’IU Windows (WinUI)

![Logo WinUI](../images/logo-winui.png)

La bibliothèque d’interface utilisateur Windows (WinUI) est un framework d’expérience utilisateur natif pour les applications de bureau Windows et UWP.

Avec l’incorporation du [système Fluent Design](https://www.microsoft.com/design/fluent/#/) à l’ensemble des expériences, des contrôles et des styles, WinUI fournit des expériences cohérentes, intuitives et accessibles à l’aide des modèles d’interface utilisateur les plus récents.

Avec la prise en charge des applications de bureau et UWP, vous pouvez effectuer des générations complètes avec WinUI ou migrer progressivement vos applications MFC, WinForms ou WPF existantes en utilisant des langages connus comme C++, C#, Visual Basic et JavaScript (par le biais de [React Native pour Windows](https://microsoft.github.io/react-native-windows/)).

> [!Important]
> Deux versions de WinUI sont disponibles : **WinUI 2.x** et **WinUI 3**.

## <a name="windows-ui-2x-library"></a>Bibliothèque Windows UI 2.x

Vous pouvez utiliser WinUI 2.x dans les applications UWP et l’incorporer à vos applications de bureau nouvelles ou existantes à l’aide de [XAML Islands](../desktop/modernize/xaml-islands.md).

> [!NOTE]
> WinUI 2.4 est la version la plus récente de WinUI 2.x. Pour obtenir la liste des travaux planifiés dans la prochaine version, consultez la page du [jalon WinUI 2.5](https://github.com/microsoft/microsoft-ui-xaml/milestone/10).

La bibliothèque WinUI 2.x est étroitement couplée au [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) et fournit des contrôles d’interface utilisateur Windows natifs officiels ainsi que d’autres éléments d’interface utilisateur pour les applications UWP.

Avec la compatibilité de bas niveau avec les versions antérieures de Windows 10, vos contrôles WinUI 2.x fonctionnent même si les utilisateurs n’ont pas le dernier système d’exploitation.

![Prise en charge des plateformes par WinUI 2.x](../images/platforms-winui2.png)

**Pour obtenir des instructions d’installation, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](winui2/getting-started.md).**

### <a name="related-links-for-winui-2x"></a>Liens associés pour WinUI 2.x

- [Vue d’ensemble de la bibliothèque WinUI 2.x](winui2/index.md)
- [Documentation sur les API](/windows/winui/api/)
- [Code source](https://aka.ms/winui)
- [Application XAML Controls Gallery](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-preview-3"></a>Bibliothèque d’interface utilisateur Windows 3 (Preview 3)

WinUI 3 est la prochaine version de WinUI, une plateforme d’interface utilisateur Windows 10 native entièrement découplée du [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).

> [!Important]
> Cette préversion de WinUI 3 est destinée à une évaluation anticipée et à la collecte de commentaires de la communauté des développeurs. Il ne doit **PAS** être utilisé pour les applications de production.
>
> Nous continuerons à fournir des préversions de WinUI 3 tout au long de 2020 et au début de 2021, après quoi la première version officielle sera rendue disponible.
>
> Utilisez le [dépôt GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml) pour fournir des commentaires et journaliser des suggestions et des problèmes.

En découplant entièrement le fichier XAML, la composition et les API d’entrée du [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk/), l’étendue de WinUI 3 inclut l’intégralité de la plateforme d’interface utilisateur native Windows 10.

WinUI est la plateforme d’avenir par excellence pour toutes les applications Windows. Vous pouvez l’utiliser comme couche d’interface utilisateur sur votre application UWP ou Win32 native, ou pour moderniser progressivement votre application de bureau, composant par composant, avec [XAML Islands](../desktop/modernize/xaml-islands.md).

Toutes les nouvelles fonctionnalités XAML seront finalement fournies dans le cadre de WinUI. Les API XAML UWP existantes fournies dans le cadre du système d’exploitation ne recevront plus de nouvelles mises à jour des fonctionnalités. Toutefois, elles continueront à recevoir des mises à jour de sécurité et des correctifs critiques conformément au cycle de vie de prise en charge de Windows 10.

Notez que la plateforme Windows universelle contient plus que le simple framework XAML. Les fonctionnalités comme les modèles d’application et de sécurité, le pipeline multimédia, l’intégration de l’interpréteur de commandes Xbox et Windows 10, et la compatibilité avec un large éventail d’appareils continueront d’être développées et prises en charge.

![Prise en charge de la plateforme WinUI 3](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>Liens associés pour WinUI 3

- [Bibliothèque d’interface utilisateur Windows 3 Preview 3 (novembre 2020)](winui3/index.md)
- [Application XAML Controls Gallery (WinUI 3 Preview 3)](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>Ressources WinUI

**Github** : WinUI est un projet open source hébergé sur GitHub. Utilisez le [dépôt WinUI](https://github.com/microsoft/microsoft-ui-xaml) pour soumettre des demandes de fonctionnalités ou des bogues, interagir avec l’équipe WinUI et consulter les plans de l’équipe pour WinUI 3 et les versions ultérieures sur leur [feuille de route](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md).

**Site web** : le site web [WinUI](https://aka.ms/winui) contient des comparaisons de produits, présente les différents avantages de WinUI et permet de se tenir informé au sujet du produit et de l’équipe du produit.