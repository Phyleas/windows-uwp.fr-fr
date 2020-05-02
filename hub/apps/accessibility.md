---
title: Accessibilité dans Windows 10
description: Cette page fournit les informations qui vous permettront de commencer à développer des applications Windows accessibles.
ms.topic: article
ms.date: 09/12/2019
keywords: Accessibilité dans Windows 10, Accessibilité, conception d’applications win32 accessibles, conception d’applications UWP accessibles, conception d’applications WPF accessibles, conception d’applications WinForms accessibles
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 2739546061febcfc96403ed5520fdcbd8bc2c462
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76725932"
---
# <a name="accessibility-in-windows-10"></a>Accessibilité dans Windows 10

![Image de bannière d’accessibilité](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>Créez une accessibilité dans vos applications pour la mettre à portée de tous les utilisateurs

Les produits et services, y compris les supports électroniques, sont accessibles lorsqu’ils sont conçus pour fournir une expérience complète et réussie pour autant de personnes que possible.

Créez des applications Windows accessibles et inclusives, avec des fonctionnalités améliorées et une plus grande facilité d’utilisation, adaptées aux personnes souffrant d’un handicap (temporaire et permanent), aux préférences personnelles, aux styles de travail spécifiques ou contraintes situationnelles (par exemple, pour les espaces de travail partagés, la conduite, la cuisine, les reflets, etc.). Certaines solutions courantes incluent la fourniture d’informations dans d’autres formats (tels que les sous-titres d’une vidéo) ou l’activation de l’utilisation de technologies d’assistance (telles que les lecteurs d’écran).

**Tout le monde devrait avoir accès aux mêmes salles dans un bâtiment, qu’on doive utiliser les escaliers ou l’ascenseur.**

Cette page fournit des informations sur la façon dont les différentes infrastructures de développement Windows assurent la prise en charge de l’accessibilité pour les développeurs qui créent des applications Windows, les développeurs de technologie d’assistance qui créent des outils tels que des lecteurs d’écran et des loupes, et les ingénieurs de tests logiciels qui créent des scripts automatisés pour tester les applications.

## <a name="platform-specific-documentation"></a>Documentation spécifique à la plateforme

:::row:::
   :::column:::
      ![Plateforme Windows universelle (UWP)](images/platform-uwp.png)

      **Plateforme Windows universelle (UWP)**

      Développez des applications et des outils accessibles sur la plateforme moderne pour les applications et jeux Windows 10 de n’importe quel appareil Windows (notamment les PC, les téléphones, Xbox One, HoloLens, etc.) et publiez-les sur le Microsoft Store.

      [Conception de logiciels inclusifs](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [Développement d’applications Windows inclusives](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [Test de l’accessibilité](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [Accessibilité dans le Store](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Applications de plateforme Win32](images/platform-win32.png)

      **plateforme Win32**

      Développez des applications et des outils accessibles sur la plateforme d’origine pour les applications Windows C/C++.

      [Nouveautés concernant l’accessibilité et l’automatisation de Windows](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [Développement d’applications accessibles pour Windows](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [Développement d’infrastructures d’interface utilisateur accessibles pour Windows](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [Développement des technologies d’assistance pour Windows](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [Tests d’accessibilité](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [Technologies d’accessibilité et d’automatisation héritées : MSAA vers UI Automation](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Fonctionnalités d’accessibilité Windows](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [Recommandations en matière de conception d’applications accessibles](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![Plateforme WPF](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      Développez des applications et des outils accessibles sur la plateforme établie pour les applications Windows gérées avec un modèle d’interface utilisateur XAML et le .NET Framework.

      [Meilleures pratiques en matière d’accessibilité](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [Notions de base d’UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [Fournisseurs UI Automation pour le code managé](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [Clients UI Automation pour le code managé](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [Modèles de contrôle UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [Modèles de texte UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [Types de contrôle UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [Spécification UI Automation et promesse communautaire](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Applications de plateforme Windows Forms](images/platform-winforms.png)

      **Windows Forms (WinForms)**

      Développez des applications et des outils accessibles pour les applications Windows gérées avec un modèle d’interface utilisateur XAML et le .NET Framework.

      [Accessibilité des Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [Création d’une application Windows accessible](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [Propriétés des contrôles Windows Forms qui prennent en charge les recommandations relatives à l’accessibilité](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Informations d’accessibilité sur les contrôles d’un Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Accessibilité web**

      Concevez, créez et testez des sites web accessibles dans Microsoft Edge.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Présentation de l’accessibilité web](https://docs.microsoft.com/microsoft-edge/accessibility)

      [Conception de sites web accessibles](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [Développement de sites web accessibles](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [Test de sites web accessibles](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>exemples

Téléchargez et exécutez des exemples Windows complets qui illustrent diverses fonctionnalités d’accessibilité.

:::row:::
   :::column:::
      [Navigateur d’exemples de code](https://docs.microsoft.com/samples/browse/)

      Le nouveau navigateur d’exemples qui remplace MSDN Code Gallery.
   :::column-end:::
   :::column:::
      [MSDN Code Gallery (hors service)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      Téléchargez des exemples pour Windows, Windows Phone, Microsoft Azure, Office, SharePoint, Silverlight et d’autres produits.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Exemples classiques Windows sur GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      Ces exemples illustrent les fonctionnalités et le modèle de programmation de Windows et Windows Server. 
   :::column-end:::
   :::column:::
      [Exemples de la plateforme Windows universelle (UWP) sur GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      Ces exemples illustrent les modèles d’utilisation des API pour la plateforme Windows universelle (UWP) dans le kit de développement logiciel (SDK) Windows pour Windows 10.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Galerie de contrôles XAML](https://github.com/microsoft/Xaml-Controls-Gallery)

      Cette application présente les différents contrôles XAML pris en charge dans le système Fluent Design.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Vidéos

Différentes vidéos abordant la création des applications Windows accessibles, les problèmes généraux concernant l’accessibilité et la manière dont Microsoft les résout.

:::row:::
   :::column:::
      **Comment bien démarrer avec l’accessibilité dans les applications Windows**
   :::column-end:::
   :::column:::
      **La minute du développeur : Développement d’applications pour l’accessibilité**
   :::column-end:::
   :::column:::
      **Introduction au handicap et à l’accessibilité**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Du piratage au produit, contrôle visuel pour Windows 10**
   :::column-end:::
   :::column:::
      **Accessibilité sur Windows 10**
   :::column-end:::
   :::column:::
      **Présentation de la conception d’applications UWP accessibles**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Conception de fonctionnalités d’accessibilité Windows**
   :::column-end:::
   :::column:::
      **Les fonctionnalités d’accessibilité Windows 10 automatisent tout le monde**
   :::column-end:::
   :::column:::
      **Augmenter la visibilité des pointeurs de souris**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Autres ressources

:::row:::
   :::column span="3":::
      **Blogs et actualités**

      Les dernières nouvelles du monde concernant l’accessibilité Microsoft.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [À la une](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs relatifs à l’accessibilité](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs Windows UI Automation](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      **Communauté et support**

      Lieu où les développeurs et utilisateurs Windows se rencontrent et apprennent ensemble.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Communauté Windows : Accessibilité](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Forum de développement de l’accessibilité et de l’automatisation de Windows](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [Answer Desk Accessibilité](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
