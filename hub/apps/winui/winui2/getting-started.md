---
title: Bien démarrer avec la bibliothèque d’interface utilisateur Windows
description: Guide pratique pour installer et utiliser la bibliothèque d’interface utilisateur Windows.
ms.topic: reference
ms.date: 07/15/2020
keywords: windows 10, uwp, sdk kit de ressources
ms.openlocfilehash: e3b0daae3f053daabe153f8a0058953ff76e3b90
ms.sourcegitcommit: 75e1f49be211e8b4b3e825978d67625776f992f5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94691567"
---
# <a name="getting-started-with-the-windows-ui-2x-library"></a>Bien démarrer avec la bibliothèque d’IU Windows 2.x

[WinUI 2.4](release-notes/winui-2.4.md) est la dernière version stable de WinUI et doit être utilisée pour les applications en production.

La bibliothèque est disponible sous forme de package NuGet qui peuvent être ajoutés à un projet Visual Studio nouveau ou existant.

> [!NOTE]
> Pour plus d’informations sur l’essai des préversions anticipées de WinUI 3, consultez [Bibliothèque d’interface utilisateur Windows 3 Preview 3 (novembre 2020)](../winui3/index.md).

## <a name="download-and-install-the-windows-ui-library"></a>Télécharger et installer la bibliothèque d’IU Windows

1. Téléchargez [Visual Studio 2019](https://developer.microsoft.com/windows/downloads) et assurez-vous de choisir la charge de travail **Développement pour la plateforme Windows universelle** dans le programme d’installation de Visual Studio.

2. Ouvrez un projet existant ou créez un projet à l’aide du modèle Application vide sous Visual C# -> Windows-> Universel, ou le modèle approprié pour la projection de votre langage.  

    > [!IMPORTANT]
    > Pour utiliser WinUI 2.4, vous devez définir TargetPlatformVersion >= 10.0.18362.0 et TargetPlatformMinVersion >= 10.0.15063.0 dans les propriétés du projet.

3. Dans le volet Explorateur de solutions, cliquez avec le bouton droit sur le nom de votre projet, puis sélectionnez **Gérer les packages NuGet**. Sélectionnez l’onglet **Parcourir** et recherchez **Microsoft.UI.Xaml** ou **WinUI**. Choisissez ensuite les [packages NuGet de la bibliothèque d’IU Windows](nuget-packages.md) que vous souhaitez utiliser.
Le package **Microsoft.UI.Xaml** contient des fonctionnalités et des contrôles Fluent adaptés à toutes les applications.  
Vous pouvez éventuellement cocher la case « Inclure la version préliminaire » pour voir les dernières préversions qui incluent de nouvelles fonctionnalités expérimentales.

    ![Capture d’écran du panneau Explorateur de solutions, montrant l’utilisateur cliquant avec le bouton droit sur le projet, et l’option Gérer les packages NuGet mise en évidence.](images/ManageNugetPackages.png "Image Gérer les packages NuGet")

    ![Capture d’écran de la boîte de dialogue Gestionnaire de package NuGet, avec l’onglet Parcourir et « winui » dans le champ de recherche.](images/NugetPackages.png)

4. Ajoutez les ressources de thème d’IU Windows (WinUI) à vos ressources App.xaml. Il existe deux façons de procéder, selon que vous disposez ou non de ressources d’application supplémentaires.

    a. Si vous n’avez pas d’autres ressources d’application, ajoutez `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` à Application.Resources :

    ``` XAML
    <Application>
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </Application>
    ```

    b. Sinon, si vous avez plusieurs jeux de ressources d’application, ajoutez `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` à Application.Resources.MergedDictionaries :

    ``` XAML
    <Application>
        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>
    </Application>
    ```

    > [!IMPORTANT]
    > L’ordre des ressources ajoutées à ResourceDictionary affecte l’ordre dans lequel elles sont appliquées. Le dictionnaire `XamlControlsResources` remplace de nombreuses clés de ressources par défaut et doit donc être ajouté à `Application.Resources` d’abord afin qu’il ne remplace pas d’autres styles ou ressources personnalisés dans votre application. Pour plus d’informations sur le chargement des ressources, consultez [Références aux ressources ResourceDictionary et XAML](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

5. Ajoutez une référence au kit de ressources dans les pages XAML et vos pages code-behind.

    * Dans votre page XAML, ajoutez une référence en haut de votre page.

        ```xaml
        xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
        ```

    * Dans votre code (si vous souhaitez utiliser les noms de types sans les qualifier), vous pouvez ajouter une directive using.

        ```csharp
        using MUXC = Microsoft.UI.Xaml.Controls;
        ```

## <a name="additional-steps-for-a-cwinrt-project"></a>Étapes supplémentaires pour un projet C++/WinRT

Quand vous ajoutez un package NuGet à un projet C++/WinRT, les outils génèrent un ensemble d’en-têtes de projection dans le dossier `\Generated Files\winrt` de votre projet. Pour placer ces fichiers d’en-têtes dans votre projet et permettre la résolution des références vers ces nouveaux types, vous pouvez les inclure dans votre fichier d’en-têtes précompilé (généralement `pch.h`). Voici un exemple qui contient les fichiers d’en-têtes générés pour le package **Microsoft.UI.Xaml**.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

Pour obtenir une procédure pas à pas complète sur l’ajout d’une prise en charge simple de la bibliothèque d’IU Windows à un projet C++/WinRT, consultez [Exemple de bibliothèque d’IU Windows C++/WinRT simple](/windows/uwp/cpp-and-winrt-apis/simple-winui-example).

## <a name="contributing-to-the-windows-ui-library"></a>Contribution à la bibliothèque d’IU Windows

WinUI est un projet open source hébergé sur GitHub.

Vous pouvez nous faire part de vos rapports de bogues, demandes de fonctionnalités et contributions de code de la Communauté dans le [dépôt de la bibliothèque d’interface utilisateur Windows](https://aka.ms/winui).

## <a name="other-resources"></a>Autres ressources

Si vous débutez avec UWP, nous vous recommandons de consulter les pages [Prise en main du développement UWP](https://developer.microsoft.com/windows/getstarted) sur le portail des développeurs.