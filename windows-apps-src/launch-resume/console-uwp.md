---
title: Créer une application de console de plateforme Windows universelle
description: Cette rubrique explique comment écrire une application UWP qui s’exécute dans une fenêtre de console.
keywords: UWP de la console
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cab52a84e631404fc4cbd682d6cedef7e799d387
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162663"
---
# <a name="create-a-universal-windows-platform-console-app"></a>Créer une application de console de plateforme Windows universelle

Cette rubrique explique comment créer une application de console [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) ou c++/CX plateforme Windows universelle (UWP).

À compter de Windows 10, version 1803, vous pouvez écrire des applications console UWP C++/WinRT ou C++/CX qui s’exécutent dans une fenêtre de console, telle qu’une fenêtre de console DOS ou PowerShell. Les applications console utilisent la fenêtre de console pour l’entrée et la sortie, et peuvent utiliser des fonctions [Runtime C universelles](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) telles que **printf** et **GetChar**. Les applications de console UWP peuvent être publiées sur le Microsoft Store. Ils ont une entrée dans la liste des applications et une vignette principale qui peut être épinglée au menu Démarrer. Les applications de console UWP peuvent être lancées à partir du menu Démarrer, même si vous les lancez généralement à partir de la ligne de commande.

Pour en voir un en action, voici une vidéo sur la création d’une application console UWP.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>Utiliser un modèle d’application console UWP 

Pour créer une application console UWP, commencez par installer les **modèles de projet application console (universels)**, disponibles à partir de la [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal). Les modèles installés sont ensuite disponibles sous **nouveau projet**  >  **installé**dans d'  >  **autres langues**  >  **Visual C++**  >  **Windows Universal** As **console application console c++/WinRT (Universal Windows)** et **application console c++/CX (universel Windows)**.

## <a name="add-your-code-to-main"></a>Ajouter votre code à main ()

Les modèles ajoutent **Program. cpp**, qui contient la `main()` fonction. C’est là que l’exécution commence dans une application console UWP. Accédez aux arguments de ligne de commande avec `__argc` les `__argv` paramètres et. L’application console UWP se termine lorsque le contrôle retourne de `main()` .

L’exemple suivant de **Program. cpp** est ajouté par le modèle de **/WinRT C++ de l’application console** :

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>Comportement de l’application console UWP

Une application console UWP peut accéder au système de fichiers à partir du répertoire à partir duquel elle est exécutée et au-dessous. Cela est possible, car le modèle ajoute l’extension [AppExecutionAlias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) au fichier Package. appxmanifest de votre application. Cette extension permet également à l’utilisateur de taper l’alias à partir d’une fenêtre de console pour lancer l’application. L’application n’a pas besoin d’être dans le chemin d’accès système pour lancer.

Vous pouvez également accorder un accès étendu au système de fichiers à votre application de console UWP en ajoutant la fonctionnalité restreinte `broadFileSystemAccess` , comme décrit dans [autorisations d’accès aux fichiers](../files/file-access-permissions.md). Cette fonctionnalité fonctionne avec les API de l’espace de noms [**Windows. Storage**](/uwp/api/Windows.Storage) .

Plusieurs instances d’une application console UWP peuvent s’exécuter à la fois, car le modèle ajoute la fonctionnalité [SupportsMultipleInstances](multi-instance-uwp.md) au fichier Package. appxmanifest de votre application.

Le modèle ajoute également la `Subsystem="console"` fonctionnalité au fichier Package. appxmanifest, ce qui indique que cette application UWP est une application console. Notez les `desktop4` `namespace` préfixes et iot2. Les applications de console UWP sont uniquement prises en charge sur les projets de bureau et d’Internet des objets (IoT).

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>Considérations supplémentaires pour les applications de console UWP

- Seules les applications UWP C++/WinRT et C++/CX peuvent être des applications console.
- Les applications de console UWP doivent cibler le bureau ou le type de projet IoT.
- Les applications de console UWP ne peuvent pas créer une fenêtre. Ils ne peuvent pas utiliser MessageBox () ou location (), ou toute autre API qui peut créer une fenêtre pour une raison quelconque, telle que les invites de consentement de l’utilisateur.
- Les applications de console UWP peuvent ne pas consommer de tâches en arrière-plan ni servir de tâche en arrière-plan.
- À l’exception de l' [activation en ligne de commande](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97), les applications de console UWP ne prennent pas en charge les contrats d’activation, y compris l’Association de fichiers, l’Association de protocole, etc.
- Bien que les applications de console UWP prennent en charge l’instanciation multiple, elles ne prennent pas en charge [la redirection d’instanciation multiple](multi-instance-uwp.md)
- Pour obtenir la liste des API Win32 disponibles pour les applications UWP, consultez [API Win32 et com pour les applications UWP](/uwp/win32-and-com/win32-and-com-for-uwp-apps) .