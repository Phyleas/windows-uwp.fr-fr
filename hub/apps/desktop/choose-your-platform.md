---
Description: Quand vous souhaitez créer une application de bureau, la première chose à faire est de déterminer s’il faut utiliser les API Win32 et COM ou .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Choisir votre plateforme d’application
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 57f3101b1134b4fdceb6a38f392e54677d2b884d
ms.sourcegitcommit: f3dd633a3149d2e206981fa52ad424d408e5508c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799581"
---
# <a name="choose-your-app-platform"></a>Choisir votre plateforme d’application

Quand vous souhaitez créer une application de bureau, la première chose à faire est de choisir la plateforme d’application à utiliser. Windows propose quatre plateformes d’application principales, chacune ayant ses propres avantages :

* [Plateforme Windows universelle (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

Toutes ces plateformes d’application vous permettent de créer des applications de bureau telles que Word, Excel et Photoshop qui s’exécutent sur le bureau Windows classique et qui tirent pleinement parti des fonctionnalités spécifiques de cet environnement. Toutefois, certaines plateformes présentent des caractéristiques communes et sont mieux adaptées à certains types d’applications :

* **UWP, WPF et Windows Forms**. Ces plateformes fournissent aux environnements de runtime managés (Windows Runtime pour UWP et .NET pour Windows Forms et WPF) de nombreux avantages, en particulier dans les domaines de la productivité des développeurs, de l’interface utilisateur (sophistiquée et personnalisable) et de la sécurité des applications. Dans la mesure où ces frameworks permettent de créer rapidement une interface utilisateur, notamment en prenant en charge les concepteurs visuels et le balisage, ils sont particulièrement adaptés aux applications métier.

* **API Win32**. L’API Win32 (également appelée API Windows) est la plateforme d’origine pour les applications Windows C/C++ natives qui nécessitent un accès direct à Windows et au matériel. Elle fournit une expérience de développement de premier ordre, et ce indépendamment d’un environnement de runtime managé tel que .NET ou WinRT. L’API Win32 est donc la plateforme de choix pour les applications qui ont besoin de performances au plus haut niveau et d’un accès direct au matériel système.

Cet article décrit plus en détail ces plateformes et vous aide à déterminer celle qui convient le mieux à votre application. 

> [!NOTE]
> Quelle que soit la plateforme d’application que vous choisissez, vous pouvez utiliser de nombreuses fonctionnalités de la plateforme Windows universelle (UWP) pour faire bénéficier à votre application d’une expérience moderne dans Windows 10. Par exemple, même si votre application de bureau repose sur WPF, Windows Forms ou l’API Win32, vous pouvez utiliser les fonctionnalités introduites dans UWP telles que le déploiement de packages MSIX et les contrôles XAML UWP. Pour plus d’informations, consultez [Moderniser vos applications de bureau](modernize/index.md).

## <a name="uwp"></a>UWP

UWP est la plateforme de pointe pour les applications et les jeux dans Windows 10. Cette plateforme hautement personnalisable utilise le balisage XAML pour séparer l’interface utilisateur (présentation) du code (logique métier). UWP convient aux applications de bureau qui nécessitent une interface utilisateur sophistiquée, une personnalisation des styles ainsi que des scénarios avec beaucoup de graphisme. UWP intègre également la prise en charge du [système Fluent Design](/windows/uwp/design/fluent-design-system/) pour l’expérience utilisateur par défaut et fournit un accès aux [API Windows Runtime (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). En adoptant Fluent, UWP prend automatiquement en charge les méthodes d’entrée courantes, notamment les entrées manuscrites, l’interaction tactile, la manette, le clavier et la souris.

Outre le fait que pouvez utiliser UWP afin de créer des applications de bureau pour PC Windows, UWP est également la seule plateforme prise en charge pour les applications Xbox, HoloLens et Surface Hub. UWP est notre plateforme d’applications de pointe la plus récente.

Pour plus d’informations sur UWP, consultez [Bien démarrer avec les applications Windows 10](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

WPF est la plateforme établie pour les applications Windows managées. Elle offre un accès complet au .NET Framework et utilise également le balisage XAML pour séparer l’interface utilisateur du code. Cette plateforme est conçue pour les applications de bureau qui nécessitent une interface utilisateur sophistiquée, une personnalisation des styles ainsi que des scénarios avec beaucoup de graphisme. WPF et UWP exigeant des compétences en développement similaires, il est plus facile de migrer vers UWP à partir d’applications WPF qu’à partir de Windows Forms.

Pour plus d’informations sur WPF, consultez [Bien démarrer (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms est la plateforme d’origine pour les applications Windows managées. Elle offre un modèle léger d’interface utilisateur et un accès complet au .NET Framework. Son principal atout est de permettre aux développeurs, même à ceux qui débutent sur la plateforme, de créer rapidement des applications. Cette plateforme de développement d’applications rapide et basée sur des formulaires propose une vaste collection intégrée de contrôles visuels et non visuels accessibles par glisser-déposer. Windows Forms n’utilisant pas XAML, vous devrez réécrire la totalité de votre interface utilisateur si vous décidez par la suite d’étendre votre application à UWP.

Pour plus d’informations sur Windows Forms, consultez [Bien démarrer avec Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparaison des plateformes : UWP, WPF et Windows Forms

Le tableau suivant compare en détail les caractéristiques de Windows Forms, de WPF et d’UWP.

| Fonctionnalité ou scénario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versions prises en charge**      |  Windows 10   |  Windows 7 et ultérieur |  Windows 7 et ultérieur  |
| **Langages**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (extensions managées pour C++), F\#, VB |  C\#, C++/CLI (extensions managées pour C++), F\#, VB   |
| **Runtime d’interface utilisateur** |    Natif (C++/WinRT et C++/CX) et managé (.NET Native)  |  Managé (.NET Framework et .NET Core 3) |   Managé (.NET Framework et .NET Core 3)   |
| **Open source** | [Oui (bibliothèque d’interface utilisateur Windows uniquement)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Oui (.NET Core uniquement)](https://github.com/dotnet/wpf) | [Oui (.NET Core uniquement)](https://github.com/dotnet/winforms)  |
| **Prend en charge XAML** |   Oui   |  Oui  |   Non   |
| **Avantages**  |  <ul><li>Balisage XAML pour l’interface utilisateur</li><li>Expérience utilisateur riche et personnalisable</li><li>Vos bases de code existantes sont conformes à .NET Standard</li><li>Prise en charge de résolutions élevées</li><li>Prise en charge de plusieurs types d’entrée sur les appareils Windows (interaction tactile, stylet, manette, souris, clavier, etc.)</li><li>Prise en charge de Xbox, HoloLens, IoT ou Surface Hub</li><li>Interface utilisateur dense (compacte) facultative</li><li>Prise en charge du code C++ natif</li><li>Autonomie de batterie optimisée</li><li>Prise en charge d’outils d’accessibilité moderne (comme les lecteurs d’écran)</li><li>Fonctionnalités des données au format texte enrichi (comme la vérification orthographique intégrée)</li><li>Prise en charge des entrées manuscrites</li><li>Exécution sécurisée par le biais de conteneurs d’applications (par exemple, le contenu non approuvé est placé en mode bac à sable)</li></ul>  |  <ul><li>Balisage XAML pour l’interface utilisateur</li><li>Expérience utilisateur riche et personnalisable</li><li>Large collection de contrôles fournis par Microsoft et ses partenaires</li><li>Interface utilisateur dense</li><li>Prise en charge de Windows 7</li><li>Prise en charge de la plateforme concernant la validation des entrées</li></ul> | <ul><li>Développement rapide d’applications</li><li>Éditeur WYSIWYG pour générer l’interface utilisateur</li><li>Large collection de contrôles fournis par Microsoft et ses partenaires</li><li>Interface utilisateur dense</li><li>Prise en charge de Windows 7</li><li>Entrée au clavier et à la souris</li></ul>          |
| **Scénarios offrant une prise en charge limitée** |  <ul><li>Prise en charge de plusieurs fenêtres<sup>1</sup></li><li>Prise en charge par la plateforme de la validation des entrées<sup>1</sup></li><li>Windows 7 n’est pas pris en charge</li><li>Certaines API UWP nécessitent des versions minimales spécifiques de Windows 10</li><li>Prise en charge complète de la plateforme et intégration du shell (par exemple, UWP ne prend pas actuellement en charge l’intégration de la barre d’état système ou l’accès complet à tous les appareils)</li><li>Accès direct à tous les fichiers sur le disque</li><li>ADO.NET</li><li>Bibliothèques de classes de base de code existantes qui utilisent des API non conformes au .NET Standard ou au Kit de certification des applications Windows</li><li>Prise en charge du bouclage réseau local (autrement dit, si votre application doit communiquer avec localhost sans créer une exemption de bouclage sur l’appareil cible)</li><li>E/S intensives sur les fichiers</li></ul>     |  <ul><li>Prise en charge de résolutions élevées<sup>2</sup></li><li>Entrée tactile<sup>2</sup></li></ul>  |  <ul><li>Prise en charge de résolutions élevées<sup>2</sup></li><li>Entrée tactile<sup>2</sup></li><li>Interface utilisateur personnalisable</li><li>Graphisme et expériences utilisateur riches (par exemple, interactions tactiles et animations)</li><li>Description complète des vues et des modèles de données</li></ul>    |   |

<sup>1</sup> Nous avons annoncé publiquement des fonctionnalités qui traiteront de ce scénario dans une prochaine version de Windows 10.

<sup>2</sup> Même si la plateforme n’offre pas une prise en charge de premier ordre des API pour ce scénario, les développeurs peuvent recourir à des solutions de contournement pour le prendre en charge.

## <a name="win32"></a>Win32

L’utilisation de l’API Win32 avec C++ permet d’atteindre les plus hauts niveaux de performance et d’efficacité, ce qui s’explique par le fait que vous pouvez davantage contrôler la plateforme cible avec du code non managé que dans un environnement de runtime managé comme WinRT et .NET. Toutefois, l’exercice d’un tel niveau de contrôle sur l’exécution de votre application nécessite plus de soin et d’attention. Ainsi, les gains de performances du runtime se font aux dépens de la productivité du développement.

Voici quelques-unes des principales fonctionnalités que vous offrent l’API Win32 et C++ pour créer des applications hautes performances.

-   Optimisations au niveau du matériel, notamment grâce au contrôle étroit de l’allocation des ressources, de la durée de vie des objets, de la disposition des données, de l’alignement, de la compression d’octets, etc.
-   Accès aux jeux d’instructions orientés performances comme SSE et AVX par le biais de fonctions intrinsèques.
-   Programmation générique efficace de type sécurisé à l’aide de modèles.
-   Conteneurs et algorithmes efficaces et sûrs.
-   DirectX, en particulier Direct3D et DirectCompute (notez que UWP offre également l’interopérabilité avec DirectX).
-   C++ AMP.

Pour plus d’informations, consultez [Bien démarrer avec les applications de bureau Windows qui utilisent l’API Win32](/windows/desktop/desktop-programming) et [Technologies des applications de bureau](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 et C++ pour les applications de bureau traditionnelles

Quand vous écrivez une application de bureau en C++, vous pouvez choisir Win32 ou MFC pour l’interface utilisateur ou un hôte de frameworks d’application tiers qui prend également en charge les plateformes non-Windows.

-   **Win32 :** cette API en C basée sur des descripteurs de la plateforme Windows comprend, entre autres, des fonctionnalités d’interface utilisateur telles que des contrôles de fenêtrage, de dessin et d’interface utilisateur. Dans la mesure où il s’agit d’une API en C de bas niveau basée sur des descripteurs, elle est peu utilisée pour créer des applications modernes ayant fortement recours à l’interface utilisateur. Toutefois, elle fournit les API de base nécessaires à l’interaction avec la plateforme Windows et constitue un choix approprié pour les applications qui nécessitent une interface utilisateur simple ou celles, comme les jeux, qui veulent simplement tenir l’interface utilisateur Windows le plus à l’écart possible.
-   **MFC (bibliothèque Microsoft Foundation Class) :** cette vénérable bibliothèque de framework d’application et d’interface utilisateur est au service des développeurs Windows depuis 1992. Ce wrapper C++ léger qui repose sur l’API Win32 en C basée sur des descripteurs fournit des interfaces orientées objet pour la plupart des fenêtres, contrôles courants et autres objets Windows prédéfinis. Bien que de nombreux frameworks d’interface utilisateur modernes dans l’écosystème .NET dépassent la MFC sur le plan pratique, elle demeure le framework d’interface utilisateur natif de choix pour de nombreux développeurs C++ créant des applications pour le bureau Windows.
-   **Frameworks d’application tiers :** dans la mesure où C++ peut s’exécuter sur un large éventail de plateformes et n’est pas lié à Windows ou au runtime .NET, des tiers ont développé de nouveaux frameworks d’application et d’interface utilisateur pour C++ afin de faciliter le développement d’applications multiplateformes avec des interfaces utilisateur riches. Certains de ces frameworks ont leur propre apparence, tandis que d’autres tels que wxWidgets ou Qt utilisent ou émulent le jeu de contrôles natif de la plateforme. À l’aide de ces bibliothèques, il est possible de partager presque tout le code source d’une application entre les versions de l’application qui s’exécutent sur Windows ou d’autres plateformes, notamment OSX ou Linux.

## <a name="other-app-platforms"></a>Autres plateformes d’applications

### <a name="progressive-web-apps-pwas"></a>Applications web progressives (PWA)

Les PWA permettent aux développeurs d’empaqueter le code de leur site web pour pouvoir l’installer et l’exécuter comme une application sur des PC Windows 10. Pour plus d’informations, consultez [Applications web progressives](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Utilisez Xamarin pour créer des applications multiplateformes pour Windows 10 pouvant s’exécuter également sur iOS et Android. Pour plus d’informations, consultez [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
