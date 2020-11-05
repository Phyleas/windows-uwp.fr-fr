---
description: Quand vous souhaitez créer une application de bureau, la première chose à faire est de déterminer s’il faut utiliser les API Win32 et COM ou .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Choisir votre plateforme d’application
ms.topic: article
ms.date: 11/04/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
keywords: développement d’applications de bureau Win32 pour Windows
ms.openlocfilehash: 46cf8a8e9a57384b85b3156b87697f898ff08ee8
ms.sourcegitcommit: 37f570c7425a3fa953a0c375c19381bf9cf2b6a2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2020
ms.locfileid: "93191900"
---
# <a name="choose-your-app-platform"></a>Choisir votre plateforme d’application

Quand vous souhaitez créer une application de bureau, la première chose à faire est de choisir la plateforme d’application à utiliser. Windows propose quatre plateformes d’application principales, chacune ayant ses propres avantages :

* [Plateforme Windows universelle (UWP)](#uwp) : Cette plateforme fournit un système de type commun, des API et un modèle d’application pour tous les appareils qui exécutent Windows 10. Les applications UWP peuvent être natives ou managées.
* [WPF](#wpf) et [Windows Forms](#windows-forms) : ces plateformes .NET fournissent un système de type commun, des API et un modèle d’application pour les applications managées.
* [Win32](#win32) : il s’agit de la plateforme d’origine pour les applications Windows C/C++ natives qui nécessitent un accès direct à Windows et à du matériel. L’API Win32 est donc la plateforme de choix pour les applications qui ont besoin de performances au plus haut niveau et d’un accès direct au matériel système.

Chacune de ces plateformes comprend un framework d’interface utilisateur et un ensemble de contrôles d’interface utilisateur complets qui vous permettent de créer des applications de bureau telles que Word, Excel et Photoshop qui s’exécutent sur le bureau Windows classique et tirent pleinement parti des fonctionnalités propres à cet environnement. Sur Windows 10, chacune de ces plateformes prend également en charge l’utilisation de la [bibliothèque d’interface utilisateur Windows (WinUI)](#windows-ui-library) pour créer son interface utilisateur.

Certaines de ces plateformes présentent des caractéristiques communes et sont mieux adaptées à certains types d’applications. Par exemple, les plateformes UWP et .NET sont toutes deux étroitement intégrées à Visual Studio. Cela offre de nombreux avantages, en particulier en ce qui concerne la productivité des développeurs, l’interface utilisateur sophistiquée et personnalisable, et la sécurité des applications. Dans la mesure où ces frameworks permettent de créer rapidement une interface utilisateur, notamment en prenant en charge les concepteurs visuels et le balisage, ils sont particulièrement adaptés aux applications métier.

> [!NOTE]
> Quelle que soit la plateforme d’application que vous choisissez, vous pouvez utiliser de nombreuses fonctionnalités de Windows 10 pour offrir une expérience moderne dans votre application. Par exemple, même si votre application de bureau repose sur WPF, Windows Forms ou l’API Win32, vous pouvez utiliser le déploiement de packages MSIX. Pour plus d’informations sur toutes les façons de moderniser vos applications de bureau, consultez [Moderniser vos applications de bureau](modernize/index.md).

## <a name="uwp"></a>UWP

UWP est la plateforme de pointe pour les applications et les jeux dans Windows 10. Cette plateforme hautement personnalisable utilise le balisage XAML pour séparer l’interface utilisateur (présentation) du code (logique métier). UWP convient aux applications de bureau qui nécessitent une interface utilisateur sophistiquée, une personnalisation des styles ainsi que des scénarios avec beaucoup de graphisme. UWP intègre également la prise en charge du [système Fluent Design](/windows/uwp/design/fluent-design-system/) pour l’expérience utilisateur par défaut et fournit un accès aux [API Windows Runtime (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). En adoptant Fluent, UWP prend automatiquement en charge les méthodes d’entrée courantes, notamment les entrées manuscrites, l’interaction tactile, la manette, le clavier et la souris.

Outre le fait que pouvez utiliser UWP afin de créer des applications de bureau pour PC Windows, UWP est également la seule plateforme prise en charge pour les applications Xbox, HoloLens et Surface Hub. UWP est notre plateforme d’applications de pointe la plus récente.

Pour plus d’informations sur UWP, consultez les articles suivants :

* [Bien démarrer](/windows/uwp/get-started/)
* [Modèles de projet](visual-studio-templates.md#uwp-templates)
* [Conception et interface utilisateur](/windows/uwp/design/)
* [Fonctionnalités et technologies](/windows/uwp/develop/)
* [Informations de référence sur les API](/uwp/)
* [Exemples](https://github.com/Microsoft/Windows-universal-samples)

## <a name="wpf"></a>WPF

WPF est la plateforme établie pour les applications Windows managées. Elle offre un accès à .NET Core et au .NET Framework complet, et utilise également le balisage XAML pour séparer l’interface utilisateur du code. Cette plateforme est conçue pour les applications de bureau qui nécessitent une interface utilisateur sophistiquée, une personnalisation des styles ainsi que des scénarios avec beaucoup de graphisme. WPF et UWP exigeant des compétences en développement similaires, il est plus facile de migrer vers UWP à partir d’applications WPF qu’à partir de Windows Forms.

Pour plus d’informations sur WPF, consultez les articles suivants :

* [Bien démarrer (WPF)](/dotnet/framework/wpf/getting-started/)
* [Modèles de projet](visual-studio-templates.md#net-templates)
* [Créer votre première application (.NET Core)](/visualstudio/get-started/csharp/tutorial-wpf/)
* [Créer votre première application (.NET Framework)](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application/)
* [Migrer des applications WPF vers .NET Core](/dotnet/desktop-wpf/migration/convert-project-from-net-framework/)
* [Informations de référence sur les API (.NET)](/dotnet/api/index)
* [Exemples](https://github.com/Microsoft/WPF-Samples)

## <a name="windows-forms"></a>Windows Forms

Windows Forms est la plateforme d’origine pour les applications Windows managées. Elle offre un modèle léger d’interface utilisateur et un accès au .NET Framework complet. Son principal atout est de permettre aux développeurs, même à ceux qui débutent sur la plateforme, de créer rapidement des applications. Cette plateforme de développement d’applications rapide et basée sur des formulaires propose une vaste collection intégrée de contrôles visuels et non visuels accessibles par glisser-déposer. Windows Forms n’utilisant pas XAML, vous devrez réécrire la totalité de votre interface utilisateur si vous décidez par la suite d’étendre votre application à UWP.

Pour plus d’informations sur Windows Forms, consultez les articles suivants :

* [Bien démarrer avec Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
* [Modèles de projet](visual-studio-templates.md#net-templates)
* [Créer votre première application Windows Forms](/dotnet/framework/winforms/creating-a-new-windows-form)
* [Tutoriel : Créer une visionneuse d’images](/visualstudio/ide/tutorial-1-create-a-picture-viewer?view=vs-2019)
* [Informations de référence sur les API (.NET)](/dotnet/api/index)
* [Amélioration des applications Windows Forms](/dotnet/framework/winforms/advanced/)

## <a name="win32"></a>Win32

L’utilisation de l’API Win32 avec C++ permet d’atteindre les plus hauts niveaux de performance et d’efficacité, ce qui s’explique par le fait que vous pouvez davantage contrôler la plateforme cible avec du code non managé que dans un environnement de runtime managé comme WinRT et .NET. Toutefois, l’exercice d’un tel niveau de contrôle sur l’exécution de votre application nécessite plus de soin et d’attention. Ainsi, les gains de performances du runtime se font aux dépens de la productivité du développement.

Voici quelques-unes des principales fonctionnalités que vous offrent l’API Win32 et C++ pour créer des applications hautes performances.

* Optimisations au niveau du matériel, notamment grâce au contrôle étroit de l’allocation des ressources, de la durée de vie des objets, de la disposition des données, de l’alignement, de la compression d’octets, etc.
* Accès aux jeux d’instructions orientés performances comme SSE et AVX par le biais de fonctions intrinsèques.
* Programmation générique efficace de type sécurisé à l’aide de modèles.
* Conteneurs et algorithmes efficaces et sûrs.
* DirectX, en particulier Direct3D et DirectCompute (notez que UWP offre également l’interopérabilité avec DirectX).

Pour plus d’informations, consultez les articles suivants :

* [Bien démarrer](/windows/win32/desktop-programming/)
* [Modèles de projet](visual-studio-templates.md#cwin32-templates)
* [Créer votre première application Win32 et C++](/windows/win32/learnwin32/learn-to-program-for-windows/)
* [Fonctionnalités et technologies](/windows/win32/desktop-app-technologies)
* [Informations de référence sur les API](/windows/win32/apiindex/windows-api-list/)
* [Exemples](https://github.com/Microsoft/Windows-classic-samples)

## <a name="windows-ui-library"></a>Bibliothèque d’IU Windows

Sur Windows 10, chacune des principales plateformes de bureau prend également en charge l’utilisation de la [bibliothèque d’interface utilisateur Windows (WinUI)](../winui/index.md) pour créer son interface utilisateur. WinUI était initialement une boîte à outils qui offrait des versions nouvelles et mises à jour des contrôles UWP pour les applications UWP ciblant des versions de bas niveau de Windows 10. WinUI s’est depuis étendue et est devenue la plateforme d’interface utilisateur native moderne pour les applications Windows 10 sur UWP, .NET et Win32.

Vous pouvez utiliser WinUI des manières suivantes dans les applications de bureau :

* Les applications UWP peuvent utiliser des contrôles WinUI à la place des contrôles UWP fournis par le SDK Windows.
* Vous pouvez mettre à jour des applications WPF, Windows Forms et C++/Win32 existantes pour utiliser [XAML Islands](modernize/xaml-islands.md) afin d’héberger des contrôles WinUI 2.x dans les applications.
* À compter de [WinUi 3.0](../winui/winui3/index.md), vous pouvez créer des [applications .NET et C++/Win32 qui utilisent une interface utilisateur entièrement basée sur WinUI](../winui/winui3/get-started-winui3-for-desktop.md).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparaison des plateformes : UWP, WPF et Windows Forms

Le tableau suivant compare en détail les caractéristiques de Windows Forms, de WPF et d’UWP.

| Fonctionnalité ou scénario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versions prises en charge**      |  Windows 10   |  Windows 7 et ultérieur |  Windows 7 et ultérieur  |
| **Langages**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (extensions managées pour C++), F\#, VB |  C\#, C++/CLI (extensions managées pour C++), F\#, VB   |
| **Runtime d’interface utilisateur** |    Natif (C++/WinRT et C++/CX) et managé (.NET Native)  |  Managé (.NET Framework et .NET Core 3) |   Managé (.NET Framework et .NET Core 3)   |
| **Open source** | [Oui (bibliothèque d’interface utilisateur Windows uniquement)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Oui (.NET Core uniquement)](https://github.com/dotnet/wpf) | [Oui (.NET Core uniquement)](https://github.com/dotnet/winforms)  |
| **Prend en charge XAML** |   Oui   |  Oui  |   Non   |
| **Avantages**  |  <ul><li>Balisage XAML pour l’interface utilisateur</li><li>Expérience utilisateur riche et personnalisable</li><li>Vos bases de code existantes sont conformes à .NET Standard</li><li>Prise en charge de résolutions élevées</li><li>Prise en charge de plusieurs types d’entrée sur les appareils Windows (interaction tactile, stylet, manette, souris, clavier, etc.)</li><li>Prise en charge de Xbox, HoloLens, IoT ou Surface Hub</li><li>Interface utilisateur dense (compacte) facultative</li><li>Prise en charge du code C++ natif</li><li>Autonomie de batterie optimisée</li><li>Prise en charge d’outils d’accessibilité moderne (comme les lecteurs d’écran)</li><li>Fonctionnalités des données au format texte enrichi (comme la vérification orthographique intégrée)</li><li>Prise en charge des entrées manuscrites</li><li>Exécution sécurisée par le biais de conteneurs d’applications (par exemple, le contenu non approuvé est placé en mode bac à sable)</li></ul>  |  <ul><li>Balisage XAML pour l’interface utilisateur</li><li>Expérience utilisateur riche et personnalisable</li><li>Large collection de contrôles fournis par Microsoft et ses partenaires</li><li>Interface utilisateur dense</li><li>Prise en charge de Windows 7</li><li>Prise en charge de la plateforme concernant la validation des entrées</li></ul> | <ul><li>Développement rapide d’applications</li><li>Éditeur WYSIWYG pour générer l’interface utilisateur</li><li>Large collection de contrôles fournis par Microsoft et ses partenaires</li><li>Interface utilisateur dense</li><li>Prise en charge de Windows 7</li><li>Entrée au clavier et à la souris</li></ul>          |
| **Scénarios offrant une prise en charge limitée** |  <ul><li>Prise en charge de plusieurs fenêtres<sup>1</sup></li><li>Prise en charge par la plateforme de la validation des entrées<sup>1</sup></li><li>Windows 7 n’est pas pris en charge</li><li>Certaines API Windows Runtime nécessitent des versions minimales spécifiques de Windows 10</li><li>Prise en charge complète de la plateforme et intégration du shell (par exemple, UWP ne prend pas actuellement en charge l’intégration de la barre d’état système ou l’accès complet à tous les appareils)</li><li>Accès direct à tous les fichiers sur le disque</li><li>ADO.NET</li><li>Bibliothèques de classes de base de code existantes qui utilisent des API non conformes au .NET Standard ou au Kit de certification des applications Windows</li><li>Prise en charge du bouclage réseau local (autrement dit, si votre application doit communiquer avec localhost sans créer une exemption de bouclage sur l’appareil cible)</li><li>E/S intensives sur les fichiers</li></ul>     |  <ul><li>Prise en charge de résolutions élevées<sup>2</sup></li><li>Entrée tactile<sup>2</sup></li></ul>  |  <ul><li>Prise en charge de résolutions élevées<sup>2</sup></li><li>Entrée tactile<sup>2</sup></li><li>Interface utilisateur personnalisable</li><li>Graphisme et expériences utilisateur riches (par exemple, interactions tactiles et animations)</li><li>Description complète des vues et des modèles de données</li></ul>    |   |

<sup>1</sup> Nous avons annoncé publiquement des fonctionnalités qui traiteront de ce scénario dans une prochaine version de Windows 10.

<sup>2</sup> Même si la plateforme n’offre pas une prise en charge de premier ordre des API pour ce scénario, les développeurs peuvent recourir à des solutions de contournement pour le prendre en charge.

## <a name="other-app-platforms"></a>Autres plateformes d’applications

### <a name="progressive-web-apps-pwas"></a>Applications web progressives (PWA)

Les PWA permettent aux développeurs d’empaqueter le code de leur site web pour pouvoir l’installer et l’exécuter comme une application sur des PC Windows 10. Pour plus d’informations, consultez [Applications web progressives](/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Utilisez Xamarin pour créer des applications multiplateformes pour Windows 10 pouvant s’exécuter également sur iOS et Android. Pour plus d’informations, consultez [Xamarin](/xamarin/xamarin-forms/get-started/index).

### <a name="uno-platform"></a>Plateforme Uno

La plateforme Uno permet au code Windows UWP (C# et XAML) de s’exécuter sur iOS, Android, macOS, Linux et WebAssembly. Elle fournit des définitions d’API complètes pour UWP dans [Windows 10 2004 (19041)](/windows/uwp/whats-new/windows-10-build-19041), ainsi que l’implémentation de certaines parties de l’API UWP, comme [Windows.UI.Xaml](/uwp/api/windows.ui.xaml.documents?view=winrt-19041), pour permettre aux applications UWP de s’exécuter sur ces plateformes. Pour plus d’informations, consultez la [documentation relative à la plateforme Uno](https://platform.uno/docs/articles/intro.html).
