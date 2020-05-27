---
title: Bibliothèque d’interface utilisateur Windows
description: Fournit des informations pour le développement d’applications WinUI 2.x et Windows.
ms.topic: article
ms.date: 04/15/2020
keywords: windows 10, uwp, sdk kit de ressources, winui, bibliothèque d’interface utilisateur Windows
ms.custom: RS5
ms.openlocfilehash: c1828405c424ca54dcb70e587479fd5307b1046d
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775857"
---
# <a name="windows-ui-library-2x"></a>Bibliothèque d’interface utilisateur Windows 2.x

![Contrôles WinUI](images/winUI-library-767.png)

La bibliothèque d’interface utilisateur Windows fournit des contrôles d’interface utilisateur Windows natifs officiels et d’autres éléments d’interface utilisateur pour les applications Windows.

Elle maintient la compatibilité de bas niveau avec les versions antérieures de Windows 10, pour que votre application fonctionne même si les utilisateurs n’ont pas le dernier système d’exploitation.

> [!NOTE]
> Consultez [WinUI 3.0 Preview 1](../winui3/index.md), mise à jour majeure de la plateforme d’interface utilisateur Windows 10 dont la publication est prévue en 2020.

## <a name="features"></a>Fonctionnalités

* **Nouveaux contrôles** : La bibliothèque d’interface utilisateur Windows contient de nouveaux contrôles qui ne sont pas fournis avec la plateforme Windows par défaut.

* **Versions mises à jour de contrôles existants** : la bibliothèque contient également des versions mises à jour de contrôles de plateforme Windows existants que vous pouvez utiliser avec des versions antérieures de Windows 10.

* **Prise en charge de versions antérieures de Windows 10** : les API de la bibliothèque d’interface utilisateur Windows s’exécutent sur les versions antérieures de Windows 10. Ainsi, vous n’êtes pas obligé d’inclure des vérifications de version ou du code XAML conditionnel pour prendre en charge les utilisateurs susceptibles de ne pas disposer du système d’exploitation le plus récent.

* **Prise en charge de XamlDirect** : les API Xaml Direct, conçues pour les développeurs d’intergiciels (middleware), vous donnent accès à des fonctionnalités XAML de niveau inférieur qui offrent de meilleures performances de processeur et de jeu de travail. XamlDirect vous permet d’utiliser des API XamlDirect sur des versions antérieures de Windows 10 sans avoir à écrire de code spécial pour gérer plusieurs versions cibles de Windows 10.

## <a name="examples"></a>Exemples

L’exemple d’application de galerie de contrôles XAML contient des démonstrations interactives et des exemples de code pour l’utilisation des contrôles WinUI.

* Installez l’application XAML Controls Gallery à partir du [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* XAML Controls Gallery est également [open source sur GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentation

Des articles de procédures pour les contrôles de la bibliothèque d’interface utilisateur Windows sont inclus dans la [documentation sur les contrôles de la plateforme Windows universelle](/windows/uwp/design/controls-and-patterns/).

Les documents de référence sur les API se trouvent ici : [API de la bibliothèque d’interface utilisateur Windows](/uwp/api/overview/winui/).

## <a name="install-and-use-the-windows-ui-library"></a>Installer et utiliser la bibliothèque d’interface utilisateur Windows

Pour obtenir des instructions, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](getting-started.md).

## <a name="open-source-and-developer-roadmap"></a>Feuille de route open source et développeur

WinUI est un projet open source hébergé sur GitHub. Vous pouvez nous faire part de vos rapports de bogues, demandes de fonctionnalités et contributions de code de la Communauté dans le [dépôt de la bibliothèque d’interface utilisateur Windows](https://aka.ms/winui).

Nous continuons à développer et à faire évoluer WinUI afin de prendre en charge davantage de scénarios de développement. Pour obtenir les informations les plus récentes sur nos plans pour WinUI, consultez notre [feuille de route](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) dans le dépôt de la bibliothèque d’interface utilisateur Windows.

## <a name="nuget-package-list"></a>Liste de packages NuGet

La bibliothèque d’interface utilisateur Windows contient plusieurs packages NuGet : [Liste des packages NuGet de la bibliothèque d’interface utilisateur Windows](nuget-packages.md).

## <a name="see-also"></a>Voir aussi

[Notes de publication de la bibliothèque d’interface utilisateur Windows 2.x](release-notes/index.md)
