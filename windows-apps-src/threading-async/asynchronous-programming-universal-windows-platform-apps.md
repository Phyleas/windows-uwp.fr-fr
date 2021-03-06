---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: Programmation asynchrone
description: Cette rubrique décrit la programmation asynchrone dans le plateforme Windows universelle (UWP) et sa représentation en C#, Microsoft Visual Basic .NET, C++ et JavaScript.
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, asynchrone
ms.localizationpriority: medium
ms.openlocfilehash: 1c0798a1d58d3a97d02e030bcd0094f8a7364d16
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860383"
---
# <a name="asynchronous-programming"></a>Programmation asynchrone
Cette rubrique décrit la programmation asynchrone dans le plateforme Windows universelle (UWP) et sa représentation en C#, Microsoft Visual Basic .NET, C++ et JavaScript.

L’utilisation de la programmation asynchrone permet à votre application de rester réactive quand elle effectue un travail qui peut prendre un certain temps. Par exemple, une application qui télécharge du contenu à partir d’Internet peut passer plusieurs secondes à attendre que le contenu arrive. Si vous avez utilisé une méthode synchrone sur le thread d’interface utilisateur pour récupérer le contenu, l’application est bloquée jusqu’à ce que la méthode retourne un résultat. L’application ne répond pas à l’interaction utilisateur. De plus, comme elle semble non réactive, l’utilisateur peut se sentir frustré. Il est donc préférable d’utiliser la programmation asynchrone, où l’application continue à s’exécuter et à répondre à l’interface utilisateur pendant qu’elle attend qu’une opération se termine.

Pour les méthodes dont l’exécution peut prendre un certain temps, la programmation asynchrone est la norme, et non l’exception, dans UWP. JavaScript, C#, Visual Basic et C++ fournissent chacun la prise en charge du langage pour les méthodes asynchrones.

## <a name="asynchronous-programming-in-the-uwp"></a>Programmation asynchrone dans UWP
De nombreuses fonctionnalités UWP, telles que les API [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) et les API [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) , sont exposées en tant qu’API asynchrones. Par Convention, les noms des API asynchrones se terminent par « Async » pour indiquer qu’une partie de leur exécution est susceptible d’avoir lieu une fois que le contrôle est retourné à l’appelant.

Quand vous utilisez les API asynchrones dans votre application de plateforme Windows universelle (UWP), votre code effectue des appels non bloquants de manière cohérente. Quand vous implémentez ces modèles asynchrones dans vos propres définitions d’API, les appelants peuvent comprendre et utiliser votre code de façon prévisible.

Voici quelques tâches courantes qui requièrent l’appel d’API Windows Runtime asynchrones.

-   Affichage d’une boîte de message

-   Utilisation du système de fichier, avec affichage d’un sélecteur de fichiers

-   Envoi et réception de données sur Internet

-   Utilisation de sockets, de flux et d’une connectivité

-   Utilisation de rendez-vous, de contacts et d’un calendrier

-   Utilisation de types de fichiers, comme l’ouverture de fichiers PDF (Portable Document Format) ou le décodage de formats image ou multimédia

-   Interaction avec un dispositif ou un service

Avec le modèle asynchrone UWP, vous pouvez éviter complètement de gérer explicitement des threads. Chaque langage de programmation prend en charge le modèle asynchrone UWP d’une façon qui lui est propre :

| Langage de programmation | Représentation asynchrone           |
|----------------------|---------------------------------------|
| C#                   | Mot clé **async**, opérateur **await** |
| Visual Basic         | Mot clé **Async** , opérateur **await** |
| C++/WinRT            | opérateur Coroutine et **co_await**  |
| C++/CX               | classe **task**, méthode **.then**      |
| JavaScript           | objet promise, fonction **then**     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>Modèles asynchrones dans UWP utilisant C# et Visual Basic
Un segment de code classique écrit en C# ou Visual Basic s’exécute de façon synchrone, ce qui signifie que quand une ligne s’exécute, elle se termine avant que la ligne suivante s’exécute. Malgré l’existence de modèles de programmation Microsoft .NET pour l’exécution asynchrone, le code obtenu a tendance à mettre en évidence les mécanismes de l’exécution de code asynchrone au lieu de mettre l’accent sur la tâche que le code tente d’accomplir. Les compilateurs UWP, .NET framework, C# et Visual Basic ont ajouté des fonctionnalités qui extraient les mécanismes asynchrones de votre code. Dans le cas de .NET et d’UWP, vous pouvez écrire du code asynchrone qui met l’accent sur ce que fait votre code plutôt que sur la façon de le faire et la chronologie des opérations. Votre code asynchrone sera assez similaire à un code synchrone. Pour plus d’informations, voir [Appeler des API asynchrones en C# ou Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>Modèles asynchrones dans UWP avec C++/WinRT
Avec C++/WinRT, vous utilisez des coroutines et l’opérateur **co_await** . Pour plus d’informations et pour obtenir des exemples de code, consultez [programmation asynchrone en C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>Modèles asynchrones dans UWP avec C++/CX
En C++/CX, la programmation asynchrone est basée sur la [**task class**](/cpp/parallel/concrt/reference/task-class) et sur sa méthode [**then method**](/cpp/parallel/concrt/reference/task-class?view=vs-2017&preserve-view=true). La syntaxe est similaire à celle des promesses JavaScript. La classe **task** et ses types associés fournissent également la fonctionnalité d’annulation et de gestion du contexte de thread. Pour plus d’informations, consultez [programmation asynchrone en C++/CX](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

La [**fonction Create \_ Async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017&preserve-view=true) assure la prise en charge de la génération d’API asynchrones qui peuvent être consommées à partir de JavaScript ou de tout autre langage prenant en charge UWP. Pour plus d’informations, consultez [création d’opérations asynchrones en C++/CX](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps).

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>Modèles asynchrones dans UWP utilisant JavaScript
En JavaScript, la programmation asynchrone suit la norme [Common JS Promises/A](https://wiki.commonjs.org/wiki/Promises/A) proposée en ayant des méthodes asynchrones qui retournent des objets promise. Les promesses sont utilisées à la fois dans l’UWP et dans la Bibliothèque Microsoft Windows pour JavaScript.

Un objet promise représente une valeur qui sera traitée dans le futur. Dans l’UWP, vous obtenez un objet promise à partir d’une fonction factory, qui, par convention, a un nom se terminant par « Async ».

Dans de nombreux cas, il est presque aussi simple d’appeler une fonction asynchrone que d’appeler une fonction conventionnelle. La différence réside dans le fait que vous utilisez la méthode [**Then**](/previous-versions/windows/apps/br229728(v=win.10)) ou [**done**](/previous-versions/windows/apps/hh701079(v=win.10)) pour assigner les gestionnaires pour les résultats ou les erreurs et pour démarrer l’opération.

## <a name="related-topics"></a>Rubriques connexes
* [Appeler des API asynchrones en C# ou Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Programmation asynchrone avec Async et Await (C# et Visual Basic)](/previous-versions/visualstudio/visual-studio-2012/hh191443(v=vs.110))
* [Exemple de scénarios de fonctionnalité Reversi : code asynchrone](/previous-versions/windows/apps/jj712233(v=win.10))
