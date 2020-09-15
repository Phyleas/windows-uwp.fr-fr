---
description: Découvrez comment commencer à créer des applications de bureau pour les PC Windows, notamment comment choisir la plateforme idéale pour les nouvelles applications et comment moderniser des applications existantes pour Windows 10.
title: Créer des applications de bureau pour les PC Windows
ms.topic: article
ms.date: 9/10/2020
keywords: développement d’applications de bureau Win32 pour Windows
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 6539f436e46f351efc355d361c0677b9fedee546
ms.sourcegitcommit: 2bb975d5df38d294277f57bf8b6a06d9bf87ca9b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/12/2020
ms.locfileid: "90046815"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>Créer des applications de bureau pour les PC Windows

Cet article fournit les informations dont vous avez besoin pour commencer à créer des applications de bureau pour Windows ou pour mettre à jour des applications de bureau existantes afin d’adopter les dernières expériences de Windows 10.

## <a name="platforms-for-desktop-apps"></a>Plateformes pour les applications de bureau

Il existe quatre plateformes principales pour créer des applications de bureau pour les PC Windows. Chaque plateforme fournit un modèle d’application qui définit le cycle de vie de l’application, un framework d’interface utilisateur complet et un ensemble de contrôles d’interface utilisateur qui vous permettent de créer des applications de bureau comme Word, Excel et Photoshop, et un accès à un ensemble complet d’API managées ou natives pour utiliser les fonctionnalités Windows. Pour une comparaison détaillée de ces plateformes ainsi que des ressources supplémentaires pour chaque plateforme, consultez [Choisir votre plateforme d’application](choose-your-platform.md).

<br/>

<table>
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>Plate-forme</th>
<th>Description</th>
<th>Documentation et ressources</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://docs.microsoft.com/windows/uwp/">Plateforme Windows universelle (UWP)</a></td>
<td><p>La plateforme de pointe pour les applications et jeux Windows 10. Vous pouvez créer des applications UWP qui utilisent exclusivement des contrôles et des API UWP, ou vous pouvez utiliser des contrôles et des API UWP dans des applications de bureau qui sont créées à l’aide de l’une des autres plateformes.</p></td>
<td><a href="/windows/uwp/get-started/">Bien démarrer</a><br/><a href="/uwp/">Informations de référence sur les API</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">Exemples</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/windows/win32/">C++/Win32</a></td>
<td><p>Plateforme préférée pour les applications Windows natives qui nécessitent un accès direct à Windows et à du matériel.</p></td>
<td><a href="/windows/win32/desktop-programming/">Bien démarrer</a><br/><a href="/windows/win32/apiindex/windows-api-list/">Informations de référence sur les API</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">Exemples</a></td>
</tr>
<tr class="odd">
<td><a href="/dotnet/framework/wpf/">WPF</a></td>
<td><p>Plateforme .NET établie pour les applications Windows managées graphiquement riches avec un modèle d’interface utilisateur XAML. Ces applications peuvent cibler <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> ou le .NET Framework complet.</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">Bien démarrer</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">Informations de référence sur les API (.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">Exemples</a></td>
</tr>
<tr class="even">
<td><a href="/dotnet/framework/winforms/">Windows Forms</a></td>
<td><p>Une plateforme .NET conçue pour les applications métier managées qui offre un modèle d’interface utilisateur léger. Ces applications peuvent cibler <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> ou le .NET Framework complet.</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">Bien démarrer</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">Informations de référence sur les API (.NET)</a></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Sur Windows 10, chacune de ces plateformes prend également en charge l’utilisation de la bibliothèque d’interface utilisateur Windows (WinUI) pour créer des interfaces utilisateur. Pour plus d’informations sur WinUI pour les applications de bureau, consultez [cette section](choose-your-platform.md#windows-ui-library).

## <a name="update-existing-desktop-apps-for-windows-10"></a>Mettre à jour les applications de bureau existantes pour Windows 10

Si vous avez déjà une application de bureau WPF, Windows Forms ou Win32 native, Windows 10 et la plateforme Windows universelle (UWP) offrent de nombreuses fonctionnalités qui vous permettent de moderniser l’expérience dans votre application. La plupart de ces fonctionnalités sont disponibles sous forme de composants modulaires que vous pouvez incorporer dans votre application, à votre rythme, sans avoir à réécrire votre application pour une autre plateforme.

Voici quelques-unes des fonctionnalités disponibles pour améliorer vos applications de bureau existantes :

* Utilisez [MSIX](/windows/msix/) pour empaqueter et déployer vos applications de bureau. MSIX est un format de package d’application Windows moderne qui permet de créer des packages universels pour toutes les applications Windows. MSIX réunit les meilleurs aspects des technologies d’installation MSI, .appx, App-V et ClickOnce, et a été conçu pour être sûr, sécurisé et fiable.
* Intégrez votre application de bureau aux expériences Windows 10 à l’aide des [extensions de package](./modernize/desktop-to-uwp-extensions.md). Par exemple, faites pointer les vignettes de démarrage vers votre application, transformez votre application en cible de partage ou envoyez des notifications toast à partir de votre application.
* Utilisez [XAML Islands](./modernize/xaml-islands.md) pour héberger des contrôles XAML UWP dans votre application de bureau. La plupart des dernières fonctionnalités de l’interface utilisateur de Windows 10 sont uniquement disponibles pour les contrôles XAML UWP.

Pour plus d’informations, consultez ces articles.

<br/>

| Article | Description |
|---------|-------------|
| [Moderniser les applications de bureau](./modernize/index.md) | Décrit les dernières fonctionnalités de développement Windows 10 et UWP que vous pouvez utiliser dans n’importe quelle application de bureau, y compris les applications WPF, Windows Forms et C++ Win32. |
| [Tutoriel : Moderniser une application WPF](./modernize/modernize-wpf-tutorial.md) | Suivez les instructions pas à pas pour moderniser un exemple d’application métier WPF existant en ajoutant des contrôles d’entrée manuscrite et de calendrier UWP à l’application, et en l’empaquetant dans un package MSIX.  |

## <a name="create-new-desktop-apps"></a>Créer des applications de bureau

Si vous créez une application de bureau pour Windows, voici des ressources pour vous aider à démarrer.

<br/>

| Article | Description |
|---------|-------------|
| [Choisir votre plateforme d’application](choose-your-platform.md) | Fournit une comparaison détaillée des plateformes d’application de bureau principales et peut vous aider à choisir la plateforme qui correspond à vos besoins. Cet article fournit également des liens utiles vers la documentation de chaque plateforme. |
| [Modèles de projet Visual Studio pour les applications Windows](visual-studio-templates.md) | Décrit les modèles de projet et d’élément que Visual Studio fournit pour vous aider à générer des applications pour les appareils Windows 10 à l’aide de C\# ou de C++. |
| [Moderniser les applications de bureau](./modernize/index.md) | Décrit les dernières fonctionnalités de développement Windows 10 et UWP que vous pouvez utiliser dans n’importe quelle application de bureau, y compris les applications WPF, Windows Forms et C++ Win32. |
| [Fonctionnalités et technologies](../features-and-technologies.md) | Fournit une vue d’ensemble des fonctionnalités Windows qui sont accessibles via chacune des plateformes d’application de bureau principales, ainsi que des liens vers la documentation associée. |

## <a name="related-documentation-and-technologies"></a>Documentation et technologies connexes

| Resource | Description |
|---------|-------------|
| [.NET Core 3.1](/dotnet/core/whats-new/dotnet-core-3-1) | Découvrez les dernières fonctionnalités de .NET Core 3.1, notamment les améliorations pour les applications WPF et Windows Forms. |
| [.NET 5](/dotnet/core/dotnet-five) | Cet article détaille ce que vous trouverez dans .NET 5, prochaine version de .NET Core après la version 3.1. |
| [Guide du bureau pour WPF et .NET Core](/dotnet/desktop-wpf/overview/index) | Développez des applications WPF qui ciblent .NET Core au lieu du .NET Framework complet.  |
| [Azure](/azure/) | Étendez la portée de vos applications avec les services cloud Azure. |
| [Visual Studio](/visualstudio/) | Découvrez comment utiliser Visual Studio pour développer des applications et des services. |
| [MSIX](/windows/msix/) | Empaquetez et déployez une application Windows dans un format de package moderne et universel. |
| [Windows IA](/windows/ai/) | Utilisez Windows AI afin de créer des solutions intelligentes pour les problèmes complexes de vos applications. |
| [Conteneurs Windows](/virtualization/windowscontainers/) | Empaquetez vos applications avec leurs dépendances dans des environnements Windows rapides et entièrement isolés. |
| [Applications web progressives](/microsoft-edge/progressive-web-apps) | Convertissez vos applications web en applications web progressives pouvant être distribuées et exécutées comme des applications UWP sur Windows 10. |
| [Xamarin](/xamarin/) | Créez des applications multiplateformes pour Windows, Android, iOS et macOS à l’aide de code .NET et d’interfaces utilisateur spécifiques de la plateforme. |
| [Archives de documentation pour Windows 8.x et antérieur](/previous-versions/windows/) | Accédez à la documentation archivée sur la création d’applications pour Windows 8. x et versions antérieures. |
