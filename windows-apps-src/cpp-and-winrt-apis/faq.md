---
author: stevewhims
description: Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT.
title: Forum aux questions sur C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, questions, fréquentes, FAQ, forum aux questions
ms.localizationpriority: medium
ms.openlocfilehash: 617f9ee49130a55cf0378f2a70b72296224dcefc
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
ms.locfileid: "1881020"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Forum aux questions sur [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT.

> [!NOTE]
> Si votre question concerne un message d’erreur que vous avez vu, consultez également la rubrique [Résolution des problèmes C++/WinRT](troubleshooting.md).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>Quelles sont les exigences pour l’[extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix)?
[VSIX](https://aka.ms/cppwinrt/vsix) applique une version cible du SDK Windows minimale de 10.0.17134.0 (Windows10, version1803). Vous avez également besoin de Visual Studio2017 (au moins la version15,6; nous recommandons au moins la version15.7). Vous pouvez identifier un projet qui utilise VSIX par la présence de `<CppWinRTEnabled>true</CppWinRTEnabled>` dans `<PropertyGroup Label="Globals">` dans le fichier `.vcxproj`. Pour plus d’informations, voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="whats-a-runtime-class"></a>Qu’est-ce qu’une *classe runtime*?
Une classe runtime est un type qui peut être activé et utilisé via des interfaces COM modernes, généralement au-delà des limites exécutables. Toutefois, une classe runtime peut également être utilisée au sein de l’unité de compilation qui l’implémente. Vous déclarez une classe runtime dans le langage IDL (Interface Definition Language), et vous pouvez l’implémenter en C++ standard à l’aide de C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Que signifient *type projeté* et *type d’implémentation*?
Si vous ne fait qu’*utiliser* une classe Windows Runtime (classe runtime), vous serez confronté exclusivement à des *types projetés*. C++/WinRT étant une *projection de langage*, les types projetés font partie de la surface de Windows Runtime qui est *projetée* en C++ avec C++/WinRT. Pour obtenir plus d’informations, voir [Utiliser des API avec C++/WinRT](consume-apis.md).

Le *type d’implémentation* contient l’implémentation d’une classe runtime, il est donc uniquement disponible dans le projet qui implémente la classe runtime. Lorsque vous travaillez dans un projet qui implémente des classes runtime (un projet de composant Windows Runtime ou un projet qui utilise l’interface utilisateur XAML), il est important d’être à l’aise avec la distinction entre votre type d’implémentation pour une classe runtime et le type projeté qui représente la classe runtime projetée en C++/WinRT. Pour obtenir plus d’informations, voir [Créer des API avec C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>Ai-je besoin de déclarer un constructeur dans le fichier IDL de ma classe runtime?
Uniquement si la classe runtime est conçue pour être utilisée depuis l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime). Pour plus d’informations sur l’objectif et les conséquences de la déclaration d’un ou de plusieurs constructeurs dans IDL, voir [Constructeurs de classe runtime](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Pourquoi l’éditeur de liens me donne-t-il une erreur «LNK2019: symbole externe non résolu»?
Si le symbole non résolu est une API des en-têtes d’espace de noms Windows pour la projection C++/WinRT (dans l'espace de noms **winrt**), l’API est déclarée en avance dans un en-tête que vous avez inclus, mais sa définition se trouve dans un en-tête que vous n’avez pas encore inclus. Incluez l’en-tête nommé pour l’espace de noms de l’API et régénérez. Pour plus d’informations, voir [en-têtes de projection C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Si le symbole non résolu est une fonction libre de Windows Runtime, telle que [RoInitialize](https://msdn.microsoft.com/library/br224650), vous devez inclure explicitement la bibliothèque parapluie [WindowsApp.lib](/uwp/win32-and-com/win32-apis) dans votre projet. La projection C++/WinRT dépend de certains de ces points d’entrée et fonctions libres (non-membres). Si vous utilisez un des modèles de projet [extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix) pour votre application, `WindowsApp.lib` est lié automatiquement pour vous. Dans le cas contraire, vous pouvez utiliser des paramètres de lien entre projets pour l'inclure, ou le faire dans le code source.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Dois-je implémenter [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) et, si tel est le cas, comment?
Si vous disposez d’une classe runtime qui libère les ressources dans son destructeur, et que cette classe runtime est conçue pour être utilisée depuis l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime), nous vous recommandons d’implémenter également **IClosable** pour prendre en charge l’utilisation de votre classe runtime par les langues qui ne possèdent pas de finalisation déterministe. Assurez-vous que vos ressources sont libérées si le destructeur, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close) ou les deux sont appelés. **IClosable::Close** peut être appelé un nombre de fois arbitraire.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>Ai-je besoin d’appeler [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) sur les classes runtime que j’utilise?
**IClosable** existe pour prendre en charge les langues qui ne possèdent pas de finalisation déterministe. Par conséquent, vous ne devez pas appeler **IClosable::Close** à partir de C++/WinRT, sauf dans de très rares cas impliquant des concurrences de fermeture ou des étreintes semi-fatales (semi-deadly embraces). Si vous utilisez des types **Windows.UI.Composition**, par exemple, vous pouvez rencontrer des cas où vous souhaiterez disposer des objets dans une séquence définie, au lieu de laisser la destruction du wrapper C++/WinRT faire le travail pour vous.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>Puis-je utiliser LLVM/Clang pour compiler avec C++/WinRT?
Nous ne prenons pas en charge la chaîne d’outils LLVM et Clang pour C++/WinRT, mais nous l'utilisons en interne pour valider la conformité aux normes de C++/WinRT. Par exemple, si vous souhaitez émuler ce que nous faisons en interne, vous pouvez essayer une expérience telle que celle décrite ci-dessous.

Accédez à la [LLVM Download Page](https://releases.llvm.org/download.html), recherchez **Download LLVM 6.0.0** > **Pre-Built Binaries**et téléchargez **Clang for Windows (64-bit)**. Pendant l’installation, choisissez d’ajouter LLVM à la variable système PATH afin de pouvoir l'appeler à partir d’une invite de commandes. Dans le cadre de cette expérience, vous pouvez ignorer les erreurs «Failed to find MSBuild toolsets directory» (Impossible de trouver le répertoire des jeux d’outils MSBuild) ou MSVC integration install failed (Échec d'installation de l’intégration MSVC), si vous les voyez. Il existe plusieurs façons d’appeler LLVM/Clang; l’exemple ci-dessous illustre une seule méthode.

```
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include "winrt/Windows.Foundation.h"
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

Étant donné que C++/WinRT utilise les fonctionnalités de la norme C++17, vous devez utiliser les indicateurs de compilateur nécessaires pour obtenir cette prise en charge; ces indicateurs diffèrent d’un compilateur à un autre.

Visual Studio est l’outil de développement que nous prenons en charge et recommandons pour C++/WinRT. Voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!NOTE]
> Si cette rubrique n’a pas répondu à votre question, vous pouvez obtenir de l’aide en utilisant la [balise `c++-winrt` sur Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).