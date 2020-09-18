---
title: Configurer votre environnement de développement sur Windows 10
description: Guide pour vous aider à configurer votre environnement de développement sur Windows et à installer vos outils et langages de code préférés. Que vous préfériez utiliser des commandes et des outils Python, NodeJS, VS Code, Git, Bash, Linux ou Android Studio, nous vous proposons de nouveaux outils efficaces tels que Terminal Windows et WSL.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Configurer Windows, environnement de développement, outils de développement, chemins de développement, Microsoft, Windows, développeur, conseils, performances, WSL, terminal, NodeJS, Python
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: b1b13635da3b5b06b43d1e6b009399da5a916094
ms.sourcegitcommit: ef3cdca5e9b8f032f46174da4574cb5593d32d56
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90593463"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>Configurer votre environnement de développement sur Windows 10

Ce guide vous permet de commencer à installer et configurer les langages et les outils dont vous avez besoin pour développer avec Windows ou le Sous-système Windows pour Linux.

## <a name="development-paths"></a>Chemins de développement

:::row:::
    :::column:::
       [![Icône NodeJS JavaScript](../images/nodejs-logo.png)](../nodejs/index.yml)<br>
        **[Bien démarrer avec NodeJS](../nodejs/index.yml)**<br>
        Installez NodeJS et configurez votre environnement de développement sur Windows ou le Sous-système Windows pour Linux.
    :::column-end:::
    :::column:::
       [![Icône Python](../images/python-logo.png)](../python/index.yml)<br>
        **[Bien démarrer avec Python](../python/index.yml)**<br>
        Installez Python et configurez votre environnement de développement sur Windows ou le Sous-système Windows pour Linux.
    :::column-end:::
    :::column:::
       [![Icône Android](../images/android-logo.png)](/windows/android)<br>
        **[Bien démarrer avec Android](/windows/android)**<br>
        Installez Android Studio ou choisissez une solution multiplateforme telle que Xamarin, React ou Cordova, et configurez votre environnement de développement sur Windows.
    :::column-end:::
    :::column:::
       [![Icône Bureau Windows](../images/windows-logo.png)](../apps/index.yml)<br>
        **[Bien démarrer avec le bureau Windows](../apps/index.yml)**<br>
        Commencez à créer des applications de bureau pour Windows 10 en utilisant UWP, Win32, WPF ou Windows Forms, ou commencez à mettre à jour et déployer des applications de bureau existantes avec MSIX et XAML Islands.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C/C++](../images/c-logo.png)](/cpp/)<br>
        **[Bien démarrer avec C++ et C](/cpp/)**<br>
        Commencez à utiliser C++, C et l’assembly pour développer des applications, des services et des outils.
    :::column-end:::
    :::column:::
       [![Icône C#](../images/csharp-logo.png)](/dotnet/csharp/)<br>
        **[Bien démarrer avec C#](/dotnet/csharp/)**<br>
        Commencez à générer des applications à l’aide de C# et de .NET Core.
    :::column-end:::
    :::column:::
       [![Icône Azure pour Java](../images/java-logo.png)](/azure/developer/java/)<br>
        **[Bien démarrer avec Java sur Azure](/azure/developer/java/)**<br>
        Commencez à générer des applications pour le cloud avec ces tutoriels et outils pour les développeurs Java.
    :::column-end:::
    :::column:::
       [![Icône PowerShell](../images/powershell.png)](/powershell/)<br>
        **[Bien démarrer avec PowerShell](/powershell/)**<br>
        Commencez la gestion de la configuration et l’automatisation des tâches multiplateformes avec PowerShell, un shell en ligne de commande et un langage de script.
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>Outils et plateformes

:::row:::
    :::column:::
       [![Icône WSL](../images/windows-linux-dev-env.png)](/windows/wsl/)<br>
        **[Sous-système Windows pour Linux](/windows/wsl/)**<br>
        Utilisez votre distribution Linux préférée, entièrement intégrée à Windows (plus besoin de double démarrage).<br>
        [Installer WSL](/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Icône Terminal Windows](../images/terminal.png)](/windows/terminal/)<br>
        **[Terminal Windows](/windows/terminal/)**<br>
        Personnalisez l’environnement de votre Terminal pour utiliser plusieurs shells en ligne de commande.
        <br>
        [Installer le Terminal](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Icône Gestionnaire de package Windows](../images/winget.png)](../package-manager/index.md)<br>
        **[Gestionnaire de package Windows](../package-manager/index.md)**<br>
        Utilisez le client winget.exe, un gestionnaire de package complet, avec votre ligne de commande pour installer des applications sur Windows 10.<br>
        [Installer le gestionnaire de packages Windows (préversion publique)](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![Icône PowerToys](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[PowerToys Windows](https://github.com/microsoft/PowerToys)**<br>
        Simplifiez et personnalisez votre expérience Windows pour gagner en productivité avec cet ensemble d’utilitaires pour utilisateurs chevronnés.<br>
        [Installer PowerToys (préversion publique)](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![Icône VS Code](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        Un éditeur de code source léger offrant la prise en charge intégrée de JavaScript, TypeScript et Node.js, un écosystème d’extensions complet (C++, C#, Java, Python, PHP et Go) et des runtimes (comme .NET et Unity).<br>
        [Installer VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Icône Visual Studio](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio](/visualstudio/windows/)**<br>
        Un environnement de développement intégré que vous pouvez utiliser pour modifier, déboguer et générer du code et pour publier des applications, avec compilateurs, complétion de code IntelliSense et bien d’autres fonctionnalités.<br>
        [Installer Visual Studio](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![icône Azure](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        Une plateforme cloud complète pour héberger vos applications existantes et simplifier vos nouveaux projets de développement. Les services Azure intègrent tout ce dont vous avez besoin pour développer, tester, déployer et gérer vos applications.<br>
        [Configurer un compte Azure](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![Icône .NET](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](/dotnet/standard/get-started/)**<br>
        Une plateforme de développement open source incluant des outils et des bibliothèques vous permettant de créer tout type d’application, y compris des applications web, mobiles, de bureau, de jeu, IoT, cloud et de microservices.<br>
        [Installer .NET](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

## <a name="run-windows-and-linux"></a>Exécuter Windows et Linux

Le sous-système Windows pour Linux (WSL) permet aux développeurs d’exécuter un système d’exploitation Linux avec Windows. Les deux partagent le même disque dur (et peuvent accéder à leurs fichiers réciproques), le Presse-papiers prend en charge la copie et le collage entre les deux de façon naturelle et il n’est pas nécessaire d’utiliser le démarrage de plusieurs systèmes. WSL vous permet d’utiliser BASH et offre aux utilisateurs de Mac le type d’environnement qui leur est familier.
- Pour en savoir plus, consultez la [documentation sur WSL](/windows/wsl) ou des [vidéos WSL sur Channel 9](https://channel9.msdn.com/Search?term=wsl&lang-en=true).

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

Vous pouvez également utiliser le Terminal Windows pour ouvrir tous vos outils en ligne de commande préférés dans la même fenêtre avec plusieurs onglets, ou dans plusieurs volets, qu’il s’agisse de PowerShell, de l’invite de commandes Windows, d’Ubuntu, de Debian, d’Azure CLI, d’Oh-My-zsh, de Git Bash ou de tous les éléments ci-dessus.

Pour en savoir plus, consultez la [documentation sur le Terminal Windows](/windows/terminal) ou visionnez des [vidéos relatives au Terminal Windows sur Channel 9](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true).

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>Transition entre Mac et Windows

Consultez notre [Guide relatif au changement d’environnement de développement de Mac vers Windows](./mac-to-windows.md) (ou le Sous-système Windows pour Linux). Il vous permettra d’identifier les différences et correspondances concernant les aspects suivants :

- [Raccourcis clavier](./mac-to-windows.md#keyboard-shortcuts)
- [Raccourcis sur pavé tactile](./mac-to-windows.md#trackpad-shortcuts)
- [Terminal et outils shell](./mac-to-windows.md#command-line-shells-and-terminals)
- [Applications et utilitaires](./mac-to-windows.md#apps-and-utilities)

![Image Office](../images/flashy-office3.png)

## <a name="additional-resources"></a>Ressources supplémentaires

- [Conseils pour l’amélioration de votre workflow](./tips.md)
- [Témoignages de développeurs qui sont passés de Mac à Windows](./dev-stories.md)
- [Tutoriels populaires, cours et exemples de code](./tutorials.md)
- [Documentation sur la pile de jeux de Microsoft](/gaming/)