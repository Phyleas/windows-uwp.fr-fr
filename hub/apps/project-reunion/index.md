---
description: Découvrez Project Reunion, les avantages qu’il offre aux développeurs, ce qui est d’ores et déjà prêt pour eux et comment donner leur avis.
title: Réunion de projet
ms.topic: article
ms.date: 11/17/2020
keywords: Win32 pour Windows, développement d’applications de bureau, Project Reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 7d5969afacc0f811e3a5c40488f45bb5b421c4b3
ms.sourcegitcommit: cddc595969c658ce30fbc94ded92db4a8ad1bf66
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349356"
---
# <a name="build-windows-apps-with-project-reunion-prerelease"></a>Générer des applications Windows avec Project Reunion (version préliminaire)

Project Reunion 0.1 en version préliminaire est une préversion d’un nouvel ensemble de composants et d’outils de développement qui représentent l’évolution à venir de la plateforme de développement d’applications Windows. Project Reunion fournit un ensemble unifié d’API et d’outils qui peuvent être utilisés de manière cohérente par n’importe quelle application sur un large éventail de versions cibles du système d’exploitation Windows 10. Project Reunion ne remplace pas les plateformes et frameworks d’applications Windows existants, comme UWP, Win32 native et .NET. Au lieu de cela, il les complète avec un ensemble commun d’API et d’outils que les développeurs peuvent utiliser sur ces plateformes.

> [!NOTE]
> Project Reunion 0.1 en version préliminaire est une préversion bêta pour développeurs. Vous êtes encouragé à essayer cette version dans votre environnement de développement. Toutefois, sachez que Project Reunion sera modifié de nombreuses façons entre maintenant et la version finale. Project Reunion 0.1 en version préliminaire n’est pas pris en charge pour les applications utilisées dans les environnements de production. **Project Reunion** est un nom de code qui peut changer dans une version ultérieure.

## <a name="goals-of-project-reunion"></a>Objectifs de Project Reunion

Project Reunion fournit un large éventail d’API Windows avec des implémentations qui sont découplées du système d’exploitation et mises à la portée des développeurs par le biais de packages NuGet. Project Reunion n’est pas destiné à remplacer le SDK Windows. Le SDK Windows continuera de fonctionner en l’état et un grand nombre des principaux composants de Windows continueront d’évoluer via des API fournies par les versions de système d’exploitation et de SDK Windows. Les développeurs sont encouragés à adopter Project Reunion à leur propre rythme.

Project Reunion a été créé pour prendre en charge les objectifs suivants.

#### <a name="unified-api-surface-across-different-types-of-windows-apps"></a>Surface d’API unifiée entre différents types d’applications Windows

Les développeurs qui souhaitent créer des applications Windows doivent choisir entre plusieurs plateformes et frameworks d’applications. Bien que chaque plateforme fournisse de nombreuses fonctionnalités et API qui peuvent être utilisées par des applications générées à l’aide d’autres plateformes, certaines fonctionnalités et API ne peuvent être utilisées que par des plateformes spécifiques. Project Reunion unifie l’accès aux API Windows pour toutes les applications Windows 10. Quel que soit le modèle d’application que vous choisissez, vous aurez accès au même ensemble d’API Windows que celles disponibles dans Project Reunion.

Au fil du temps, nous prévoyons d’investir davantage dans Project Reunion pour supprimer d’autres différences entre les modèles d’application. Project Reunion inclut à la fois les API WinRT et les API C natives.

#### <a name="consistent-support-across-windows-10-versions"></a>Prise en charge cohérente entre les versions de Windows 10

À mesure que les API Windows continuent d’évoluer avec les nouvelles versions de système d’exploitation, les développeurs doivent utiliser des techniques telles que le [code adaptatif de version](/windows/uwp/debug-test-perf/version-adaptive-code) pour tenir compte de toutes les différences dans les versions et atteindre le public de leur application. Cela peut compliquer le code et l’expérience de développement. Project Reunion nous permettra d’unifier l’accès aux API Project Reunion sur différentes versions de système d’exploitation et tous les appareils Windows 10, et nous pourrons ainsi le cas échéant mettre les mises à jour, et pas seulement la version la plus récente de Windows, à la disposition d’un plus grand nombre de développeurs. Nous projetons pour l’instant de faire en sorte que Project Reunion prenne en charge Windows 10, version 1809, et toutes les versions ultérieures de Windows 10.

#### <a name="faster-release-cadence"></a>Cadence de publication plus rapide

Les nouvelles API et fonctionnalités Windows sont généralement liées à des versions de système d’exploitation qui sont publiées une ou deux fois par an. Project Reunion nous permettra de publier de nouvelles API et fonctionnalités de qualité production de manière plus fréquente et souple.

## <a name="get-started"></a>Bien démarrer

Project Reunion 0.1 en version préliminaire comprend de nouvelles API pour les domaines de fonctionnalités suivants.

| Fonctionnalité | Description |
|---------|-------------|
| [MRT Core (système de gestion des ressources moderne)](mrtcore/mrtcore-overview.md) | MRT Core est une version simplifiée du [système de gestion des ressources Windows](/windows/uwp/app-resources/resource-management-system) moderne qui est distribué dans le cadre de Project Reunion. |
| [DWriteCore](dwritecore.md) | DWriteCore est une forme de DirectWrite qui s’exécute sur les versions de Windows jusqu’à Windows 8, et qui vous permet de l’utiliser sur plusieurs plateformes. |

Vous pouvez en apprendre davantage sur les futurs plans d’ajout d’autres composants à Project Reunion [ici](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md).

> [!NOTE]
> Certains autres composants existants, dont la bibliothèque d’interface utilisateur Windows (WinUI), MSIX et WebView2, sont déjà découplés du système d’exploitation et suivent les instructions de Project Reunion (par exemple, ils sont pris en charge sur Windows 10, version 1809, et les versions ultérieures du système d’exploitation). Toutefois, ces autres composants ne font pas partie du package NuGet Project Reunion.  

### <a name="set-up-your-development-environment"></a>Configurer l''environnement de développement

Si vous souhaitez essayer Project Reunion 0.1 en version préliminaire, vous devez commencer par l’un des exemples C++ fournis. Ces exemples sont préconfigurés pour utiliser le package NuGet Project Reunion. Cette préversion ne prend pas en charge l’installation du package NuGet Project Reunion dans vos propres projets. Suivez ces étapes pour configurer votre environnement de développement afin d’utiliser l’un des exemples.

1. Vérifiez que votre ordinateur de développement dispose de Windows 10, version 1809 (build 17763) ou d’une version plus récente du système d’exploitation.

2. Installez [Visual Studio 2019, version 16.9 Preview 2 (ou ultérieure)](https://visualstudio.microsoft.com/vs/preview/). Vous devez inclure les charges de travail suivantes lors de l’installation de Visual Studio :
    - Développement .NET Desktop
    - Développement pour la plateforme Windows universelle
    - Développement Desktop en C++
    - Composant facultatif **Outils de plateforme Windows universelle C++ (v142)** pour la charge de travail Plateforme Windows universelle (voir **Détails de l’installation** sous la section **Développement pour plateforme Windows universelle**, dans le volet de droite du programme d’installation)

3. Installez la dernière version de l’[extension Visual Studio (VSIX) C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) à partir de Visual Studio Marketplace.

4. Assurez-vous que votre système a une source de package NuGet activée pour **nuget.org**. Pour plus d’informations, consultez [Configurations NuGet courantes](/nuget/consume-packages/configuring-nuget-behavior).

5. Téléchargez et installez le [package VSIX WinUI 3 Preview 3](https://aka.ms/winui3/preview3-download). Cette étape est requise uniquement pour les exemples Hello World et MRT Core, qui sont déjà configurés pour utiliser WinUI 3. Pour obtenir des instructions sur la façon d’ajouter le package VSIX à Visual Studio, consultez [Recherche et utilisation des extensions Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box).

6. Clonez et explorez les exemples suivants :
    - [Exemple de galerie DWriteCore](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery) : Cet exemple d’application illustre l’API [DWriteCore](dwritecore.md).
    - [Exemple MRT Core](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore) : Cet exemple d’application illustre l’API [MRT Core](mrtcore/mrtcore-overview.md).
    - [Exemple Hello World](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp) : Cet exemple illustre une intégration de base au package NuGet Project Reunion.

## <a name="known-issues"></a>Problèmes connus

Project Reunion 0.1 en version préliminaire présente les limitations suivantes :

 - Cette version est prise en charge uniquement pour les applications C++/Win32 empaquetées avec MSIX.
 - Cette version ne prend pas en charge C#.
 - Cette version ne prend pas en charge .NET 5.

## <a name="developer-roadmap"></a>Plan de mise en œuvre du développeur

Pour obtenir les derniers plans relatifs à Project Reunion, consultez notre [page GitHub](https://github.com/microsoft/ProjectReunion).

## <a name="give-feedback-and-contribute"></a>Envoyer des commentaires et contribuer

Nous générons Project Reunion en tant que projet open source. Beaucoup plus d’informations sont disponibles dans notre [page GitHub](https://github.com/microsoft/ProjectReunion) sur la façon dont nous envisageons de faire de Project Reunion une réalité, et nous vous invitons à participer au processus de développement. Consultez notre [guide du contributeur](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md) pour poser des questions, commencer des discussions ou proposer des fonctionnalités. Nous voulons nous assurer que Project Reunion apporte des avantages majeurs aux développeurs comme vous.
