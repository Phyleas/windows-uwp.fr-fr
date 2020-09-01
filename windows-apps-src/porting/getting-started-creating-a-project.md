---
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: Microsoft Visual Studio représente pour Windows ce que Xcode représente pour iOS et Mac OS. Cette procédure pas à pas vous permet de vous familiariser avec Visual Studio.
title: Création d’un projet dans Visual Studio
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6be772573321e7f1cbf5e5861d5f7b15a1ba5183
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174893"
---
# <a name="getting-started-creating-a-project"></a>Bien démarrer : Création d’un projet

## <a name="creating-a-project"></a>Création d’un projet

Microsoft Visual Studio représente pour Windows ce que Xcode représente pour iOS et Mac OS. Cette procédure pas à pas vous permet de vous familiariser avec Visual Studio. Elle vous présente les notions de base essentielles que vous devez connaître pour débuter. Chaque fois que vous créez une application, vous exécutez des étapes semblables à ce qui suit.

La vidéo suivante compare Xcode à Visual Studio.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Comparing-Xcode-to-Visual-Studio/player]

Ce billet de blog [Building apps for Windows](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/) (en anglais) vous sera également très utile.

La création d’une application pour Windows 10, ou application de plateforme Windows universelle (UWP), est similaire à la création d’une application iOS à l’aide de tables de montage séquentiel. L’application Windows 10 est souvent construite sur plusieurs pages, dont chacune contient une partie de l’interface utilisateur, comme un site web. Généralement, chaque page possède deux fichiers sources associés : l’un pour stocker l’interface utilisateur au format [XAML](../xaml-platform/xaml-overview.md), et l’autre contenant le code source, souvent C#. En interagissant avec votre application, l’utilisateur navigue entre ces pages. Dans ce guide pas à pas, vous allez créer une application sur deux pages.

**Remarque**    Une fonctionnalité importante des applications Windows 10 est le fait que le même code source et le même ensemble d’API sont à votre disposition, quel que soit la plateforme. Comme vous le savez, lorsque vous écrivez une application iOS universelle pour iPhone et iPad, vous pouvez déterminer au moment de l’exécution la plateforme sur laquelle votre application s’exécute, et prendre les mesures nécessaires. De même, les applications Windows 10 peuvent déterminer au moment de l’exécution l’appareil sur lequel elles s’exécutent. Avec une application UWP, il n’est pas nécessaire d’utiliser \# des ifdef dans votre code source pour créer des builds de téléphone et de bureau. Heureusement, les applications Windows 10 utilisent intelligemment leurs contrôles d’interface utilisateur en fonction de l’appareil. Par exemple, votre application peut faire référence à un contrôle de sélecteur de dates. L’apparence et le fonctionnement du contrôle diffèrent selon que celui-ci s’exécute sur un ordinateur de bureau ou un écran de téléphone. Toutefois, le code source reste la même.

Voyons comment créer une application Windows 10. Commencez par exécuter Visual Studio. Lors de sa première exécution, Visual Studio vous demande de vous procurer une licence de développeur. Une licence de développeur vous permet d’installer et de tester des applications UWP sur votre ordinateur local avant de les soumettre au Microsoft Store. Pour obtenir une licence, suivez les instructions à l’écran pour vous connecter à l’aide d’un compte Microsoft. Si vous n’en possédez pas, cliquez sur le lien **S’inscrire** dans la boîte de dialogue **Licence de développeur** et suivez les instructions à l’écran.

Par comparaison, lorsque vous démarrez Xcode, la première chose que vos voyez est l’écran de **bienvenue dans Xcode** présenté dans la figure qui suit.

![écran de bienvenue Xcode](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio est très semblable. Vous allez voir la **page de démarrage**, présentée dans la figure suivante.

![écran d’accueil Visual Studio](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

Pour créer une application, commencez par créer un projet en effectuant l’une des opérations suivantes :

-   Dans la zone **Démarrer**, appuyez sur **Nouveau projet**.
-   Appuyez sur le menu **Fichier**, puis sur **Nouveau projet**.

Par comparaison, lorsque vous créez un projet dans Xcode, vous voyez une liste de modèles de projet apparaître comme dans la figure ci-dessous.

![boîte de dialogue nouveau projet dans xcode](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

Dans Visual Studio, plusieurs modèles de projet peuvent être choisis comme le montre la figure qui suit.

![boîte de dialogue Nouveau projet Visual Studio ](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png) pour cette procédure pas à pas, appuyez sur **Visual C#**, puis sur **Windows**, **Windows universelle** et **application vide (Windows universel)**. Dans la zone **Nom**, tapez « MyApp », puis appuyez sur **OK**. Visual Studio crée, puis affiche votre premier projet. Vous êtes maintenant prêt à concevoir votre application et à y ajouter du code.

## <a name="next-step"></a>Étape suivante

[Bien démarrer : Choix d’un langage de programmation](getting-started-choosing-a-programming-language.md)